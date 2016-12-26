# Windows核心编程笔记

## 第3章 内核对象

### 3.1
* 每个内核对象只是内核分配的一个**内存块**,并且**只能由该内核访问**。该内存块是一种**数据结构**,它的成员负责维护该对象的各种信息。有些数据成员(如安全性描述符、使用计数等)在所有对象类型中是相同的,但大多数数据成员属于特定的对象类型。例如,进程对象有一个进程 I D、一个基本优先级和一个退出代码,而文件对象则拥有一个字节位移、一个共享模式和一个打开模式。

* 我们**不能直接操纵**内核对象，Windows提供了一组函数,以便用定义得很好的方法来对这些结构进行操作。这些内核对象始终都可以通过这些函数进行访问。当调用一个用于创建内核对象的函数时,该函数就返回一个用于标识该对象的句柄。该句柄可以被视为一个**不透明值**,你的**进程中的任何线程**都可以使用这个值。如果将该句柄值传递给**另一个进程**中的一个线程(使用某种形式的进程间的通信)那么这另一个进程使用你的进程的句柄值所作的调用就会**失败**。但是有3种机制,能使多个进程能够成功地**共享**单个内核对象。

* 内核对象由内核所拥有,而不是由进程所拥有。如果你的进程调用了一个创建内核对象的函数,然后你的进程终止运行,那么内核对象**不一定被撤消**。内核知道有多少进程正在使用某个内核对象,因为每个对象包含一个**使用计数**。使用计数是所有内核对象类型常用的数据成员之一。如果内核对象的使用计数降为 0,内核就撤消该对象。这样可以确保在没有进程引用该对象时系统中不保留任何内核对象。

* 内核对象能够得到**安全描述符**的保护。安全描述符用于描述谁创建了该对象,谁能够访问或使用该对象,谁无权访问该对象。用于创建内核对象的函数几乎都有一个指向 SECURITY_ATTRIBUTES结构的指针作为其参数,下面显示了CreateFileMapping函数的指针:
```cpp
HANDLE CreateFileMapping(
	HANDLE hFile,
	PSECURITY_ATTRIBUTES psa,
	DWORD flProtect,
	DWORD dwMaximumSizeHigh,
	DWORD dwMaximumSizeLow,
	PCTSTR pszName);
```  
SECURITY_ATTRIBUTES结构类似下面的样子:  
```
typedef struct _SECURITY_ATTRIBUTES {
	DWORD nLength;
	LPVOID lpSecurityDescriptor;
	BOOL bInheritHandle;
} SECURITY_ATTRBUTES;
```
* 要确定一个对象是否属于内核对象,最容易的方法是观察创建该对象所用的函数。创建内核对象的所有函数几乎都有一个参数,你可以用来设定安全属性的信息,用于创建用户对象或GDI对象的函数都没有PSECURITY_ATTRIBUTES参数。

### 3.2
* 当一个进程被初始化时,系统要为它分配一个**句柄表**。该句柄表只用于内核对象 ,不用于用户对象或GDI对象。当进程初次被初始化时,它的句柄表是空的。每当调用一个将内核对象句柄接受为参数的函数时,就要传递由一个Create\*&函数返回的值。从内部来说,该函数要查看进程的句柄表,以获取要生成的内核对象的地址,然后按定义得很好的方式来生成该对象的数据结构。
如 果 传 递 了 一 个 无 效 索 引(句柄),该函数便返回失败,而GetLastError 则返回6(ERROR\_INVALID\_HANDLE)。由于句柄值实际上是放入进程句柄表的索引,因此这些句柄是与进程相关的,并且不能由其他进程成功地使用。

* 无论怎样创建内核对象,都要向系统指明将通过调用CloseHandle来结束对该对象的操作:  
```cpp
BOOL CloseHandle(HANDLE hobj);
```

* 应用程序在**运行时有可能**泄漏内核对象,但是当进程终止运行时,系统将能确保所有内容均被正确地清除。另外,这个情况适用于所有对象、资源和内存块,也就是说,当进程**终止运行**时,系统将保证进程**不会留下任何对象**。

### 3.3
* 许多情况下,在不同进程中运行的线程需要共享内核对象。下面是为何需要共享的原因:  
  * 文件映射对象使你能够在同一台机器上运行的两个进程之间共享数据块。
  * 邮箱和指定的管道使得应用程序能够在连网的不同机器上运行的进程之间发送数据块。
  * 互斥对象、信标和事件使得不同进程中的线程能够同步它们的连续运行,这与一个应用程序在完成某项任务时需要将情况通知另一个应用程序的情况相同。
  
* 只有当进程具有父子关系时,才能使用对象句柄的继承：
  * 首先,当父进程创建内核对象时,必须向系统指明,它希望对象的句柄是个可继承的句柄。请记住,虽然内核对象句柄具有继承性,但是内核对象本身不具备继承性。若要创建能继承的句柄,父进程必须指定一个 SECURITY\_ATTRIBUTES结构并对它进行初始化。如果将 bIheritHandle成员置为TRUE，对象变为可继承。
  * 使用对象句柄继承性时要执行的下一个步骤是让父进程生成子进程。这要使用Create Process函数来完成。对象句柄的继承性只有在生成子进程的时候才能使用。如果父进程准备创建带有可继承句柄的新内核对象,那么已经在运行的子进程将无法继承这些新句柄。
  * 子进程不知道它已经继承了任何句柄。只有在另一个进程生成子进程时记录了这样一个情况,即它希望被赋予对内核对象的访问权时,才能使用内核对象句柄的继承权。也就是说，必须传递给子进程句柄的值子进程才知道用这个值去访问对象。
  
* 共享跨越进程边界的内核对象的第二种方法是给对象命名。
  * 下列函数都可以创建命名的内核对象：
    * CreateMutex
    * CreateEvent
    * CreateSemaphore
    * CreateWaitableTimer
    * CreateFileMapping
    * CreateJobObject  
  通过传递参数给pszName这个最后的参数，可以给对象命名，但是无法保证一个同名对象是否已经存在。
  * 在第二个进程调用Create\*函数后，立刻调用GetLastError，可以知道是创建了新的对象还是打开了一个现有的对象：  
```
if (GetLastError() == ERROR\_ALREADY\_EXISTS) {
}
```  
  * 进程按名字共享对象的另一种方法是不适用Create\*函数，而是调用Open\*函数中的某一个来打开已存在的内核对象。Create\*函数跟Open\*函数的主要区别是如果对象不存在，Create\*函数将创建对象，而Open\*函数则运行失败。
  
* 共享跨越进程边界的内核对象的最后一个方法是使用DuplicateHandle函数:  
```cpp
BOOL DuplicateHandle(
	HANDLE hSourceProcessHandle,
	HANDLE hSourceHandle,
	HANDLE hTargetProcessHandle,
	PHANDLE phTargetHandle,
	DWORD dwDesiredAccess,
	BOOL bInheritHandle,
	DWORD dwOptions);
```
简单说来,该函数取出一个进程的句柄表中的项目,并将该项目拷贝到另一个进程的句柄表中。DuplicateHandle函数配有若干个参数,但是实际上它是非常简单的。 DuplicateHandle函数最普通的用法要涉及系统中运行的 3个不同进程。

## 第4章 进程
### 4.1
* **进程**通常被定义为一个正在运行的程序的实例,它由两个部分组成:
  1. 一个是操作系统用来管理进程的**内核对象**。内核对象也是系统用来存放关于进程的统计信息的地方。
  2. 另一个是地址空间,它包含所有可执行模块或 DLL模块的代码和数据。它还包含**动态内存分配**的空间。如线程堆栈和堆分配空间。
  
* 进程是不活泼的。若要使进程完成某项操作,它**必须拥有**一个在它的环境中运行的线程,该线程负责执行包含在进程的地址空间中的代码。**每个线程**都有它自己的一组**CPU寄存器**和它自己的**堆栈**。

* 用于CUI应用程序的链接程序开关是 `/SUBSYSTEM:CONDOLE` ,而用于GUI应用程序的链接程序开关是 `SUBSYSTEM:WINDOWS` 。

* Windows应用程序可以使用的进入点函数有四个：
  1. `int WINAPI WinMain`
  2. `int WINAPI wWinMain`
  3. `int __cdecl main`
  4. `int __cdecl wmain`  
操作系统实际上并不调用你编写的进入点函数。它调用的是C/C++运行期启动函数。该函数负责对C/C++运行期库进行初始化,这样,就可以调用malloc和free之类的函数。它还能够确保已经声明的任何全局对象和静态C++对象能够在代码执行以前正确地创建。

* 当进入点函数返回时,启动函数便调用C运行期的exit函数,将返回值(nMainRetVal)传递给它。Exit函数负责下面的操作:
  * 调用由_onexit函数的调用而注册的任何函数。
  * 为所有全局的和静态的C++类对象调用析构函数。
  * 调用操作系统的ExitProcess函数,将nMainRetVal传递给它。这使得该操作系统能够撤消进程并设置它的exit代码。
  
* 加载到进程地址空间的每个可执行文件或DLL文件均被赋予一个独一无二的实例句柄。可执行文件的实例作为(w)WinMain的第一个参数hinstExe来传递。对于加载资源的函数调用来说,通常都需要该句柄的值。

* (w)WinMain函数的格式：
```cpp
int WINAPI WinMain(
	HINSTANCE hinstExe,
	HINSTANCE,
	PSTR pszCmdline,
	int nCmdShow);
```

* 可以获得一个指向进程的完整命令行的指针,方法是调用GetCommandLine函数: `PTSTR GetCommandLine();` 该函数返回一个指向包含完整命令行的缓存的指针,该命令行包括执行文件的完整路径名。

* 每个进程都有一个与它相关的环境块。环境块是进程的地址空间中分配的一个内存块。每个环境块都包含一组字符串,其形式如下:  
```
VarName1=VarValue1\0
VarName2=VarValue2\0
```
等号不能是变量名的一部分，而且紧跟等号的空格也会作为变量或者值的一部分。

* 进程的错误模式:  
与每个进程相关联的是一组标志,用于告诉系统,进程对严重的错误应该如何作出反映,
这包括磁盘介质故障、未处理的异常情况、文件查找失败和数据没有对齐等。进程可以告诉系统如何处理每一种错误。方法是调用 `SetErrorMode` 函数

### 4.2
* 可以用CreateProcess函数创建一个进程:  
```cpp
BOOL CreateProcess(
	PCTSTR pszApplicationName,
	PTSTR pszCommandLine,
	PSECURITY_ATTRIBUTES psaProcess,
	PSECURITY_ATTRIBUTES psaThread,
	BOOL bInheritHandles,
	DWORD fdwCreate,
	PVOID pvEnvironment,
	PCTSTR pszCurDir,
	PSTARTUPINFO psiStartInfo,
	PPROCESS_INFORMATION ppiProcInfo);
```  
注意 在进程被完全初始化之前,CreateProcess返回TRUE。由于CreateProcess返回TRUE,因此父进程不知道出现的任何初始化问题。  
大多数情况下pszApplicationName都是NULL，如果不传递NULL,可以将地址传递给pszApplicationName参数中包含想运行的可执行文件的名字的字符串。  
pvEnvironment参数用于指向包含新进程将要使用的环境字符串的内存块。在大多数情况下,为该参数传递 NULL,使子进程能够继承它的父进程正在使用的一组环境字符串。  
psiStartInfo参数用于指向一个STARTUPINFO结构，大多数应用程序将要求生成的应用程序仅仅使用默认值。至少应该将该结构中的所有成员初始化为零,然后将cb成员设置为该结构的大小  。
p p i P r o c I n f o参数用于指向你必须指定的PROCESS_INFORMATION结构。CreateProcess在返回之前要对该结构的成员进行初始化。该结构的形式如下面所示:
```cpp
typedef struct _PROCESS_INFORMATION {
	HANDLE hProcess;
	HANDLE hThread;
	DWORD dwProcessId;
	DWORD dwThreadId;
} PROCESS_INFORMATION
```

### 4.3
* 若要终止进程的运行,可以使用下面四种方法:
  * 主线程的进入点函数返回(最好使用这个方法)。
  * 进程中的一个线程调用E x i t P r o c e s s函数(应该避免使用这种方法)。
  * 另一个进程中的线程调用Te r m i n a t e P r o c e s s函数(应该避免使用这种方法)。
  * 进程中的所有线程自行终止运行(这种情况几乎从未发生)。
  
### 4.4
* 创建新进程，让它进行一些操作，并且等待结果的代码例子：
```
PROCESS_INFROMATION pi;
DWORD dwExitCode;

BOOL fSuccess = CreateProcess(..., &pi);
if(fSuccess) {
	CloseHandle(pi.hThread);
	WaitForSingleObject(pi.hProcess, INFINITE);
	GetExitCodeProcess(pi.hProcess, &dwExitCode);
	CloseHandle(pi.hProcess);
}
```
若要放弃与子进程的所有联系, Explorer必须通过调用CloseHandle来关闭它与新进程及它的主线程之间的句柄。

### 4.5

## 第19章 DLL基础
* 动态链接库(DLL)一直是Windows操作系统的基础,Windows API 中的所有函数都包含在DLL中。 3个最重要的DLL是Kernel32.dll,它包含用于管理内存、进程和线程的各个函数; User32.dll,它包含用于执行用户界面任务(如窗口的创建和消息的传送)的各个函数; GDI32.dll,它包含用于画图和显示文本的各个函数。

### 19.1
* 在应用程序(或另一个DLL)能够调用DLL中的函数之前,DLL文件映像必须被映射到调用进程的地址空间中。若要进行这项操作,可以使用两种方法中的一种,即加载时的隐含链接或运行期的显式链接。一旦DLL的文件映像被映射到调用进程的地址空间中, DLL的函数就可以供进程中运行的所有线程使用。实际上, DLL几乎将失去它作为DLL的全部特征。

### 19.2
* DLL创建使用流程：
```
创造DLL:
1) 建立带有输出原型/结构/符号的头文件。
2) 建立实现输出函数/变量的C/C++源文件。
3) 编译器为每个C/C++源文件生成 .obj模块。
4) 链接程序将生成DLL的 .obj模块链接起来。
5) 如果至少输出一个函数/变量,那么链接程序也生成lib 文件。
创造EXE:
6) 建立带有输入原型/结构/符号的头文件。
7) 建立引用输入函数/变量的C/C++源文件。
8) 编译器为每个C/C++源文件生成 .obj源文件。
9) 链接程序将各个 .obj模块链接起来,产生一个 .exe文件(它包含了所需要DLL模块的名字和输入符号的列表)。
运行应用程序:
10) 加载程序为 .exe 创建地址空间。
11) 加载程序将需要的DLL加载到地址空间中进程的主线程开始执行;应用程序启动运行。
```

## DLL的高级操作技术
### 20.1
* 显式加载DLL的过程：
```
创造DLL :
1) 建立带有输出原型/结构/符号的头文件。
2) 建立实现输出函数/变量的 C/C++源文件。
3) 编译器为每个 C/C++源文件生成 .obj模块。
4) 链接程序将生成DLL的 .obj模块链接起来。
5) 如果至少输出一个函数/变量,那么链接程序也生成 .lib 文件。
创造EXE :
6) 建立带有输入原型/结构/符号的头文件(视情况而定)。
7) 建立不引用输入函数/变量的 C/C++源文件。
8) 编译器为每个 C/C++源文件生成 .obj源文件。
9) 链接程序将各个 .obj模块链接起来,生成 .exe文件。
注: DLL的lib文件是不需要的,因为并不直接引用输出符号。.exe 文件不包含输入表。
运行应用程序 :
10) 加载程序为 .exe 创建模块地址空进程的主线程开始执行;应用程序启动运行。
显式加载DLL:
11) 一个线程调用LoadLibrary (Ex)函数,将DLL加载到进程的地址空间 这时线程可以调用GetProcAddress 以便间接引用DLL的输出符号。
```

* 无论何时,进程中的线程都可以决定将一个 D L L映射到进程的地址空间,方法是调用下面两个函数中的一个:
```
HINSTANCE LoadLibrary(PCTSTR pszDLLPathName);
HINSTANCE LoadLibraryEx(
	PCTSTR pszDLLPathName,
	HANDLE hFile,
	DWORD dwFlags);
```
LoadLibraryEx函数配有两个辅助参数,即hFile和dwFlags。参数hFile保留供将来使用,现在必须是NULL 。对于参数dwFlags ,必须将它设置为0,或者设置为DONT\_RESOLVE\_DLL\_REFERENCES、LOAD\_LIBRARY\_AS\_DATAFILE和LOAD\_WITH\_ALTERED\_SEARCH\_PATH等标志的一个组合。

* 当进程中的线程不再需要 D L L中的引用符号时,可以从进程的地址空间中显式卸载DLL,方法是调用下面的函数:
```cpp
VOID FreeLibraryAndExitThread(
	HINSTANCE hinstDLL,
	DWORD dwExitCode);
```
必须传递HINSTANCE值,以便标识要卸载的DLL 。该值是较早的时候调用LoadLibrary(Ex)而返回的值。也可以通过调用FreeLibraryAndExitThread函数从进程的地址空间中卸载DLL.

## 第22章 插入DLL和挂接API
### 22.2
* 可以用修改注册表的方式插入DLL，只需要修改一个存在的注册表关键字的值，这是最简单的方法，但是有一些缺点。
