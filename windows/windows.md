#Windows编程  

##基础
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

##Unicode
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
* HelloWin程序：
```
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

