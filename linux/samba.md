# samba 
## samba配置参数详解
[samba配置详解](http://blog.csdn.net/splenday/article/details/47116969) 
## samba常见问题
* 新版的samba可能会存在问题，访问时链接断开，具体参见链接  
[samba问题解决](http://askubuntu.com/questions/772730/samba-software-caused-connection-abort)   
通过安装旧版可以避免这个bug：  
```
sudo apt-get install samba=2:4.1.6+dfsg-1ubuntu2 samba-common=2:4.1.6+dfsg-1ubuntu2 samba-libs=2:4.1.6+dfsg-1ubuntu2 samba-common-bin=2:4.1.6+dfsg-1ubuntu2 samba-dsdb-modules=2:4.1.6+dfsg-1ubuntu2 python-samba=2:4.1.6+dfsg-1ubuntu2 libldb1=1:1.1.16-1 python-ldb=1:1.1.16-1
```