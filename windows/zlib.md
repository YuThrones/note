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

* 示例代码：
```cpp
//code by MulinB
//2011-04-28

#define UNICODE
#define _UNICODE

#include <atlconv.h>  //for W2CA
#include "zlib/contrib/minizip/zip.h"


//最终接口：从某个目录创建zip文件
void CreateZipFromDir(const CString& dirName, const CString& zipFileName);


//将文件添加到zip文件中，注意如果源文件srcFile为空则添加空目录
//fileNameInZip: 在zip文件中的文件名，包含相对路径
void AddFileToZip(zipFile zf, const char* fileNameInZip, const char* srcFile)
{
  FILE* srcfp = NULL;

  //初始化写入zip的文件信息
  zip_fileinfo zi;
  zi.tmz_date.tm_sec = zi.tmz_date.tm_min = zi.tmz_date.tm_hour =
  zi.tmz_date.tm_mday = zi.tmz_date.tm_mon = zi.tmz_date.tm_year = 0;
  zi.dosDate = 0;
  zi.internal_fa = 0;
  zi.external_fa = 0;

  //如果srcFile为空，加入空目录
  char new_file_name[MAX_PATH];
  memset(new_file_name, 0, sizeof(new_file_name));
  strcat(new_file_name, fileNameInZip);
  if (srcFile == NULL)
  {
      strcat(new_file_name, "/");
  }
  
  //在zip文件中创建新文件
  zipOpenNewFileInZip(zf, new_file_name, &zi, NULL, 0, NULL, 0, NULL, Z_DEFLATED, Z_DEFAULT_COMPRESSION);

  if (srcFile != NULL)
  {
      //打开源文件
      srcfp = fopen(srcFile, "rb");
      if (srcfp == NULL)
      {
          MessageBox(_T("无法添加文件") + CString(srcFile) + _T("!"));
          zipCloseFileInZip(zf); //关闭zip文件
          return;
      }

      //读入源文件并写入zip文件
      char buf[100*1024]; //buffer
      int numBytes = 0;
      while( !feof(srcfp) )
      {
          numBytes = fread(buf, 1, sizeof(buf), srcfp);
          zipWriteInFileInZip(zf, buf, numBytes);
          if( ferror(srcfp) )
              break;
      }

      //关闭源文件
      fclose(srcfp);
  }

  //关闭zip文件
  zipCloseFileInZip(zf);
}


//递归添加子目录到zip文件
void CollectFilesInDirToZip(zipFile zf, const CString& strPath, const CString& parentDir)
{
  USES_CONVERSION; //for W2CA
  
  CString strRelativePath;
  CFileFind finder; 
  BOOL bWorking = finder.FindFile(strPath + _T("//*.*"));
  while(bWorking) 
  { 
      bWorking = finder.FindNextFile(); 
      if(finder.IsDots())
          continue; 
      
      if (parentDir == _T(""))
          strRelativePath = finder.GetFileName();
      else
          strRelativePath = parentDir + _T("//") + finder.GetFileName(); //生成在zip文件中的相对路径
  
      if(finder.IsDirectory())
      {
          AddFileToZip(zf, W2CA(strRelativePath), NULL); //在zip文件中生成目录结构
          CollectFilesInDirToZip(zf, finder.GetFilePath(), strRelativePath); //递归收集子目录文件
          continue;
      }
      
      AddFileToZip(zf, W2CA(strRelativePath), W2CA(finder.GetFilePath())); //将文件添加到zip文件中
  }
}


//最终接口：从某个目录创建zip文件
void CreateZipFromDir(const CString& dirName, const CString& zipFileName)
{
  USES_CONVERSION; //使用W2CA转换unicode字符集
  zipFile newZipFile = zipOpen(W2CA(zipFileName), APPEND_STATUS_CREATE); //创建zip文件
  if (newZipFile == NULL)
  {
    MessageBox(_T("无法创建zip文件!"));
    return;
  }
  
  CollectFilesInDirToZip(newZipFile, dirName, _T(""));
  zipClose(newZipFile, NULL); //关闭zip文件
}

```

### 解压缩
参考[minizip解压文件详解](minizip解压文件详解 "http://www.cnblogs.com/menlsh/p/4480577.html")  
* 示例代码：
```cpp
/*
 * 函数功能 :  解压zip文件
 * 备    注 : 参数strFilePath表示zip压缩文件的路径
 *           参数strTempPath表示要解压到的文件目录
 * 作    者 : 博客园 依旧淡然（http://www.cnblogs.com/menlsh/）
 */
void CZlibDemoDlg::UnzipFile(CString strFilePath, CString strTempPath)
{
    int nReturnValue;

    //打开zip文件
    unzFile unzfile = unzOpen(strFilePath);                    
    if(unzfile == NULL)
    {
        MessageBox("打开zip文件失败!", "提示", MB_OK|MB_ICONWARNING);
        return;
    }

    //获取zip文件的信息
    unz_global_info* pGlobalInfo = new unz_global_info;
    nReturnValue = unzGetGlobalInfo(unzfile, pGlobalInfo);
    if(nReturnValue != UNZ_OK)
    {
        MessageBox("获取zip文件信息失败!", "提示", MB_OK|MB_ICONWARNING);
        return;
    }

    //解析zip文件
    unz_file_info* pFileInfo = new unz_file_info;
    char szZipFName[MAX_PATH];                            //存放从zip中解析出来的内部文件名
    for(int i=0; i<pGlobalInfo->number_entry; i++)
    {
        //解析得到zip中的文件信息
        nReturnValue = unzGetCurrentFileInfo(unzfile, pFileInfo, szZipFName, MAX_PATH, 
            NULL, 0, NULL, 0);
        if(nReturnValue != UNZ_OK)
        {
            MessageBox("解析zip文件信息失败!", "提示", MB_OK|MB_ICONWARNING);
            return;
        }

        //判断是文件夹还是文件
        switch(pFileInfo->external_fa)
        {
        case FILE_ATTRIBUTE_DIRECTORY:                    //文件夹
            {
                CString strDiskPath = strTempPath + _T("//") + szZipFName;
                CreateDirectory(strDiskPath, NULL);
            }
            break;
        default:                                        //文件
            {
                //创建文件
                CString strDiskFile = strTempPath + _T("//") + szZipFName;
                HANDLE hFile = CreateFile(strDiskFile, GENERIC_WRITE,
                    0, NULL, OPEN_ALWAYS, FILE_FLAG_WRITE_THROUGH, NULL);
                if(hFile == INVALID_HANDLE_VALUE)
                {
                    MessageBox("创建文件失败!", "提示", MB_OK|MB_ICONWARNING);
                    return;
                }

                //打开文件
                nReturnValue = unzOpenCurrentFile(unzfile);
                if(nReturnValue != UNZ_OK)
                {
                    MessageBox("打开文件失败!", "提示", MB_OK|MB_ICONWARNING);
                    CloseHandle(hFile);
                    return;
                }

                //读取文件
                const int BUFFER_SIZE = 4096;
                char szReadBuffer[BUFFER_SIZE];
                while(TRUE)
                {
                    memset(szReadBuffer, 0, BUFFER_SIZE);
                    int nReadFileSize = unzReadCurrentFile(unzfile, szReadBuffer, BUFFER_SIZE);
                    if(nReadFileSize < 0)                //读取文件失败
                    {
                        MessageBox("读取文件失败!", "提示", MB_OK|MB_ICONWARNING);
                        unzCloseCurrentFile(unzfile);
                        CloseHandle(hFile);
                        return;
                    }
                    else if(nReadFileSize == 0)            //读取文件完毕
                    {
                        unzCloseCurrentFile(unzfile);
                        CloseHandle(hFile);
                        break;
                    }
                    else                                //写入读取的内容
                    {
                        DWORD dWrite = 0;
                        BOOL bWriteSuccessed = WriteFile(hFile, szReadBuffer, BUFFER_SIZE, &dWrite, NULL);
                        if(!bWriteSuccessed)
                        {
                            MessageBox("读取文件失败!", "提示", MB_OK|MB_ICONWARNING);
                            unzCloseCurrentFile(unzfile);
                            CloseHandle(hFile);
                            return;
                        }
                    }
                }
            }
            break;
        }
        unzGoToNextFile(unzfile);
    }

    //关闭
    if(unzfile)
    {
        unzClose(unzfile);
    }
}
```