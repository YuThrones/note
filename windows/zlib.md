# zlib
## 编译源码
* 参考[http://www.cnblogs.com/MrOuqs/p/5751485.html](http://www.cnblogs.com/MrOuqs/p/5751485.html "编译zlib") 

1. 用Visual Studio打开 `zlib128\zlib-1.2.8\contrib\vstudio\vc14` 
2. 右键点击zlibvc，在弹出菜单中选择Properties(属性)，在弹出的属性对话框选择Build Event(生成事件)，点击Pre-Build Event(预先生成事件)，会看到Command Line(命令行)中有错误提示所示的命令行，在这里我们对其进行修改，点击命令旁边的下拉按钮，然后点击edit(编辑)，把内容删掉，替换如下：(路径替换为具体所在的盘和路径，例如zlib解压在F盘就E改为F，然后第二行相应改为对应路径)
```
E:
cd E:\zlib128\zlib-1.2.8\contrib\masmx64
bld_ml64.bat
```
最后生成解决方案即可，在x64\ZlibDllDebug下面可以找到对应的输出文件

