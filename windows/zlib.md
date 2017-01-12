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

## 导入库
* 将 `zlib128` 目录下的 `zlib.h` 和 `zconf.h` 复制到自己的工程目录下，并且将在编译后生成在 `ZlibDllDebug` 文件夹下面的 `zlibwapi.lib` 复制到自己工程目录下。并且在项目属性->C/C++->常规->附加包含目录下加入 `./`。然后如下即可使用：
```
#include <zlib.h>
#pragma comment(lib, "zlibwapi.lib")
```

## 使用minizip
### 压缩
总体流程是，创建一个zip文件，然后每个要压缩进去的文件都在zip文件里面创建对应的新文件，然后将源文件的数据写进入之后关闭zip文件流。  
参考[minizip压缩文件详解](http://blog.csdn.net/shineorrain/article/details/26138129)   
用到的函数如下：
1.  zipOpen  打开、创建zip文件
2.  zipOpenNewFileInZip  在zip文件中创建新文件
3.  zipWriteInFileInZip  将数据写入zip文件中的文件里
4.  zipCloseFileInZip    关闭zip文件中的文件
5.  zipClose  关闭zip文件

