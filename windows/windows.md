# Windows编程  

## 基础
在每个用 C 编写的Windows 程式的开头都可看到:
```  
#include <windows.h>
```  

* WINDOWS.H 是主要的含入档案,它包含了其他Windows 表头档案,这些表头
档案的某些也包含了其他表头档案。这些表头档案中最重要的和最基本的是:
  * WINDEF.H 基本型态定义。
  * WINNT.H 支援 Unicode 的型态定义。
  * WINBASE.H Kernel 函式。
  * WINUSER.H 使用者介面函式。
  * WINGDI.H 图形装置介面函式。
  
  * 正如在 C 程式中的进入点是函数 main 一样,Windows 程式的进入点是
WinMain,总是像这样出现:  
```
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, PSTR szCmdLine, int iCmdShow)
```
该进入点在 / Platform SDK / User Interface Services / Windowing /Windows / Window Reference / Window Functions 中有说明。它在 WINBASE.H中宣告如下:  
```
int
WINAPI
WinMain(
    HINSTANCE hInstance,
    HINSTANCE hPrevInstance,
    LPSTR lpCmdLine,
    int nShowCmd
);
```
第三个参数在 WINBASE.H中定义为 LPSTR,我将它改为 PSTR。这两种资料型态都定义在 WINNT.H 中,作为指向字串的指标。LP 字首代表「长指标」,这是 16 位元 Windows 下的产物。字首 i 表示 int、sz 表示「以零结束的字串」。  
WinMain 函式宣告为返回一个 int 值。WINAPI 识别字在 WINDEF.H 定义,语
句如下:
```
#define WINAPI __stdcall
```
该语句指定了一个呼叫约定,包括如何生产机器码以在堆栈中放置函数调用的参数。许多 Windows 函数调用声明为 WINAPI。  

* WinMain的第一个参数唯一的标志该程序，需要它在其他Windows函数调用中作为参数。同一应用程序的可以通过检查hPrevInstance确定自身的其他执行实体是否正在运行。在Win32版本中已被废弃，总是为NULL。第三个参数用于执行程序的命令参数，第四个用于支出程序最初显示的方式。

* MessageBox的第一个参数是消息窗拥有的窗口的ID，第二个是要显示的信息，第三个是窗体上方标题列的信息，第四个是用于确定在对话框中出现的按钮等。

## Unicode
* ASCII使用８位，Unicode使用16位。C 中的宽字元基於 wchar_t 资料型态, 它在几个表头档案包括 WCHAR.H 中都有定义,像这样:
```
typedef unsigned short wchar_t ;
```
因此,wchar_t 资料型态与无符号短整数型态相同,都是 16 位元宽。  
可定义指向宽字串的指标:
```
wchar_t * p = L"Hello!" ;
```
注意紧接在第一个引号前面的大写字母 L(代表「long」)。这将告诉编译器该字串按宽字元保存——即每个字元占用 2 个位元组。

* 如果定义了_UNICODE 识别字,那么一个称作__T 的巨集就定义如下:
```
#define __T(x) L##x
```
那一对井字号称为「粘贴符号(token paste)」,它将字母 L 添加到巨集引数上。因此,如果巨集引数是"Hello!",则 L##x 就是 L"Hello!"。基本地,必须按下述方法在_T 或_TEXT 巨集内定义字串文字:
```
_TEXT ("Hello!")
```

* INNT.H 的前面包含 C 的表头档案 CTYPE.H,这是 C 的众多表头档案之一,
包括 wchar_t 的定义。WINNT.H 定义了新的资料型态,称作 CHAR 和 WCHAR:
```
typedef char CHAR ;
typedef wchar_t WCHAR ;
// wc
```
这里精选了表头档案中一些实用的说明资料型态语句:
```
typedef CHAR * PCHAR, * LPCH, * PCH, * NPSTR, * LPSTR, * PSTR ;
typedef CONST CHAR * LPCCH, * PCCH, * LPCSTR, * PCSTR ;
```
WINNT.H 将 TCHAR定义为一般的字元类型。如果定义了识别字 UNICODE(没有底线),则 TCHAR 和指向 TCHAR 的指标就分别定义为 WCHAR 和指向 WCHAR 的指标;如果没有定义识别字 UNICODE, 则 TCHAR 和指向 TCHAR 的指标就分别定义为 char 和指向 char 的
指标:
```
#ifdef UNICODE
typedef WCHAR TCHAR, * PTCHAR ;
typedef LPWSTR LPTCH, PTCH, PTSTR, LPTSTR ;
typedef LPCWSTR LPCTSTR ;
#else
typedef char TCHAR, * PTCHAR ;
typedef LPSTR LPTCH, PTCH, PTSTR, LPTSTR ;
typedef LPCSTR LPCTSTR ;
#endif
```

* 在文字模式程式设计中,
```
printf ("The sum of %i and %i is %i", 5, 3, 5+3) ;
```
的功能相同於
```
char szBuffer [100] ;
sprintf (szBuffer, "The sum of %i and %i is %i", 5, 3, 5+3) ;
puts (szBuffer) ;
```
## 窗口
* HelloWin程序：
```c
HELLOWIN.C
/*------------------------------------------------------------------------
	HELLOWIN.C -- Displays "Hello, Windows 98!" in client area
		(c) Charles Petzold, 1998
-----------------------------------------------------------------------*/
#include <windows.h>

LRESULT CALLBACK WndProc (HWND, UINT, WPARAM, LPARAM) ;

int WINAPI WinMain (HINSTANCE hInstance, HINSTANCE hPrevInstance,
	PSTR szCmdLine, int iCmdShow)
{
	static TCHAR szAppName[] = TEXT ("HelloWin") ;
	HWND hwnd ;
	MSG msg ;
	WNDCLAS wndclass ;
	wndclass.style = CS_HREDRAW | CS_VREDRAW ;
	wndclass.lpfnWndProc = WndProc ;
	wndclass.cbClsExtra = 0 ;
	wndclass.cbWndExtra = 0 ;
	wndclass.hInstance = hInstance ;
	wndclass.hIcon = LoadIcon (NULL, IDI_APPLICATION) ;
	wndclass.hCursor = LoadCursor (NULL, IDC_ARROW) ;
	wndclass.hbrBackground = (HBRUSH) GetStockObject (WHITE_BRUSH) ;
	wndclass.lpszMenuNam = NULL ;
	wndclass.lpszClassName = szAppName ;
	
	if (!RegisterClass (&wndclass))
	{
	     MessageBox (	NULL, TEXT ("This program requires Windows NT!"),
			   szAppName, MB_ICONERROR) ;
	     return 0 ;
	}
	hwnd = CreateWindow( szAppName,	// window class name
	    TEXT ("The Hello Program"), // window caption
	    WS_OVERLAPPEDWINDOW, // window style
	    CW_USEDEFAULT,  // initial x position
	    CW_USEDEFAULT, // initial y position
	    CW_USEDEFAULT, // initial x size
	    CW_USEDEFAULT, // initial y size
	    NULL, // parent window handle
	    NULL, // window menu handle
	    hInstance, // program instance handle
	NULL) ; // creation parameters
	
	ShowWindow (hwnd, iCmdShow) ;
	UpdateWindow (hwnd) ;
	
	while (GetMessage (&msg, NULL, 0, 0))
	{
		  TranslateMessage (&msg) ;
		  DispatchMessage (&msg) ;
	}
	return msg.wParam ;
}

LRESULT CALLBACK WndProc (HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam)
{
	HDC hdc ;
	PAINTSTRUCT ps ;
	RECT rect ;
	switch (message)
	{
		 case WM_CREATE:
		 	PlaySound (TEXT ("hellowin.wav"), NULL, SND_FILENAME | SND_ASYNC) ;
		 	return 0 ;
		 case WM_PAINT:
			 hdc = BeginPaint (hwnd, &ps) ;
			 
			 GetClientRect (hwnd, &rect) ;
			 
			 DrawText (hdc, TEXT ("Hello, Windows 98!"), -1, &rect,
			 	DT_SINGLELINE | DT_CENTER | DT_VCENTER) ;
			 EndPaint (hwnd, &ps) ;
			 return 0 ;
		 case WM_DESTROY:
			 PostQuitMessage (0) ;
			 return 0 ;
	}
	return DefWindowProc (hwnd, message, wParam, lParam) ;
}
```
WndProc 函式传回一个型态为 LRESULT 的值,该值简单地被定义为一个LONGWinMain 函式被指定了一个 WINAPI 型态,而 WndProc 函式被指定一个 CALLBACK 型态。这两个识别字都被定义为_stdcall,表示在 Windows 本身和使用者的应用程式之间发生的函式呼叫的呼叫参数传递方式。

* 视窗讯息处理程式在处理讯息时,必须传回 0。视窗讯息处理程式不予处理的所有讯息应该被传给名为DefWindowProc 的 Windows 函式。 从 DefWindowProc 传回的值必须由视窗讯息处理程式传回。

## 文本输出
* 目前使用最为普遍的文字输出函式是TextOut。该函式的格式如下:
```
TextOut (hdc, x, y, psText, iLength) ;
```
TextOut 向视窗的显示区域写入字串。psText 参数是指向字串的指标,iLength 是字串的长度。x 和 y 参数定义了字串在显示区域的开始位置(不久会讲述关於它们的详细情况)。hdc 参数是「装置内容代号」.程式必须在处理单个讯息处理期间取得和释放代号。除了呼叫 CreateDC(函式,在本章暂不讲述)建立的装置内容之外,程式不能在两个讯息之间保存其他装置内容代号。  

* 在处理 WM_PAINT 讯息时,使用这种方法。它涉及 BeginPaint 和 EndPaint两个函式,这两个函式需要视窗代号(作为参数传给视窗讯息处理程式)和PAINTSTRUCT 结构的变数 (在 WINUSER.H 表头档案中定义)的地址为参数。Windows程式写作者通常把这一结构变数命名为 ps 并且在视窗讯息处理程式中定义它:
```
PAINTSTRUCT ps ;
```

* 在处理 WM_PAINT 讯息时,视窗讯息处理程式首先呼叫 BeginPaint。BeginPaint 函式一般在准备绘制时导致无效区域的背景被擦除。该函式也填入ps 结构的栏位。BeginPaint 传回的值是装置内容代号,这一传回值通常被保存在叫做 hdc 的变数中。
```
HDC hdc ;
```

* 一般地,处理 WM_PAINT 讯息的形式如下:
```
case WM_PAINT:
	hdc = BeginPaint (hwnd, &ps) ;
		使用 GDI 函式
	EndPaint (hwnd, &ps) ;
	return 0 ;
```
在处理 WM_PAINT 讯息时,必须成对地呼叫 BeginPaint 和 EndPaint。如果视窗讯息处理程式不处理 WM_PAINT 讯息,则它必须将 WM_PAINT 讯息传递给Windows中DefWindowProc(内定视窗讯息处理程式)。

* 在处理 WM_PAINT 讯息时,为了在更新的矩形外绘图,可以使用如下呼叫:
```
InvalidateRect (hwnd, NULL, TRUE) ;
```
该呼叫在 BeginPaint 呼叫之前进行,它使整个显示区域变为无效,并擦除背景。但是,如果最後一个参数等於 FALSE,则不擦除背景,原有的东西将保留在原处。

* 要得到视窗显示区域的装置内容代号,可以呼叫 GetDC 来取得代号,在使用完後呼叫 ReleaseDC:
```
hdc = GetDC (hwnd) ;
使用 GDI 函式
ReleaseDC (hwnd, hdc) ;
```

## 进程  
* 进程通常被定义为一个正在运行的程序的实例,它由两个部分组成:  
  * 一个是操作系统用来管理进程的内核对象。内核对象也是系统用来存放关于进程的统计
信息的地方。
  * 另一个是地址空间,它包含所有可执行模块或 D L L模块的代码和数据。它还包含动态内
存分配的空间。如线程堆栈和堆分配空间。

* 每个进程都有一个与它相关的环境块。环境块是进程的地址空间中分配的一个内存块。每
个环境块都包含一组字符串,其形式如下:  
```
VarName1=VarValue1\0
VarName2=VarValue2\0
```
由于等号用于将变量名与变量值隔开，因为等号不能是变量名一部分，紧靠等号的空格也会作为一部分。

* 要终止进程的运行,可以使用下面四种方法:
  * 主线程的进入点函数返回(最好使用这个方法)。
  * 进程中的一个线程调用E x i t P r o c e s s函数(应该避免使用这种方法)。
  * 另一个进程中的线程调用Te r m i n a t e P r o c e s s函数(应该避免使用这种方法)。
  * 进程中的所有线程自行终止运行(这种情况几乎从未发生)。

* CreateProcess例子：
```
STARTUPINFO si;
PROCESS_INFORMATION pi;
memset(&si, 0, sizeof(STARTUPINFO));
memset(&pi, 0, sizeof(PROCESS_INFORMATION));
si.cb = sizeof(STARTUPINFO);

CreateProcess(TEXT("D:\\weixin\\WeChat\\WeChat.exe"),
	TEXT(""),
	NULL,
	NULL,
	FALSE,
	0,
	NULL,
	NULL,
	&si,
	&pi);
```
参考[http://www.cnblogs.com/wangliang651/archive/2007/05/25/760057.html](http://www.cnblogs.com/wangliang651/archive/2007/05/25/760057.html "CreateProcess详解")