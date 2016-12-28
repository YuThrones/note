# VS使用问题及解决方案
## error LNK2005: _DllMain@12 already defined in 
* 解决方案：在自己定义DllMain的地方前面加上`extern "C" { int _afxForceUSRDLL; } `即可  
参考链接[http://stackoverflow.com/questions/343368/error-lnk2005-dllmain12-already-defined-in-msvcrt-lib/19930430#19930430](http://stackoverflow.com/questions/343368/error-lnk2005-dllmain12-already-defined-in-msvcrt-lib/19930430#19930430)

