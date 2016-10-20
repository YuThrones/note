# cobbler
## cobbler安装
### 文档参考
[https://www.ibm.com/developerworks/cn/linux/l-cobbler/](https://www.ibm.com/developerworks/cn/linux/l-cobbler/ "文档参考")  

1. 安装cobbler和cobbler-web
  其中cobbler-web是cobbler的web接口，可以通过它来使cobbler操作形象。
```
sudo apt-get install cobbler cobbler-web
```
建议把`debmirror` 和 `createrepo` 也一并安装好，其中debmirror是用来建立Debian系统镜像源的工具，而createrepo是用来简历RedHat系列镜像源的工具。  
安装完成后，cobbler和apache的服务都会启动。

2. 检查安装cobbler  
  `curl -I localhost/cobbler/`  
  看到 `HTTP 200 OK` 说明工作正常了。  

3. 初步配置cobbler
  运行 `cobbler check`检查配置，根据提示修复错误  
  同时配置debmirror.conf配置文件  
```cp /usr/share/doc/debmirror/examples/debmirror.conf /etc/```
  注释掉@dists和@arches两行  
  然后使配置生效， `sudo cobbler sync`

4. 配置cobbler web
安装好cobbler后，默认访问信息如下：  
用户名：cobbler  
密码：cobbler  
出于安全考虑，可以通过htdigest命令来修改用户cobbler的密码。
```
htdigest /etc/cobbler/user.digest "Cobbler" cobbler
Changing password for user cobbler in realm Cobbler 
New password: 
Re-type new password:
```
我们随时可以通过cobbler sync来重新加载配置文件

5. 安装dnsmasq和tftpd-hpa
```
apt-get install dnsmasq
apt-get install tftpd-hpa
```
配置cobbler接管DHCP、DNS和tFTP服务  
修改配置文件 `/etc/cobbler/settings`  
需要修改和修改后的值如下：  
```
manage_dhcp: 1
manage_dns: 1
manage_tftpd: 1
restart_dhcp: 1
restart_dns: 1
pxe_just_once: 1
next_server: <server's IP address>
server: <server's IP address>
```
使用 `$ openssl passwd -1` 生成密码，修改 `default_password_crypted: ""`

6. 配置dhcp，dns和tFTP  
修改 `/etc/cobbler/modules.conf`
```
[authentication]
module = authn_configfile
[authorization]
module = authz_allowall
[dns]
module = manage_dnsmasq # uses dnsmasq
[dhcp]
module = manage_dnsmasq # uses dnsmasq 
[tftpd]
module = manage_in_tftpd  # defaut, uses the system's tftp server, in this example, use tftpd-hpa
```  
修改dhcp模板：`vi /etc/cobbler/dhcp.template`  
```
subnet 192.168.1.0 netmask 255.255.255.0 {
     option routers             192.168.1.1;
     option domain-name-servers 192.168.1.210,192.168.1.211;
     option subnet-mask         255.255.255.0;
     filename                   "/pxelinux.0";
     default-lease-time         21600;
     max-lease-time             43200;
     next-server                $next_server;
}
```
修改dnsmasq模板：`vi /etc/cobbler/dnsmasq.template `
```
read-ethers
addn-hosts = /var/lib/cobbler/cobbler_hosts
#domain=
dhcp-range=192.168.88.100,192.168.88.254
dhcp-option=3,$next_server
dhcp-lease-max=1000
dhcp-authoritative
dhcp-boot=pxelinux.0
dhcp-boot=net:normalarch,pxelinux.0
dhcp-boot=net:ia64,$elilo
$insert_cobbler_system_definitions
```
修改tFTP模板：`vi /etc/cobbler/tftpd.template`
```
{
		disable                = no
        socket_type            = dgram
        protocol                = udp
        wait                    = yes
        user                    = $user
        server                  = $binary
        server_args            = -B 1380 $args
        per_source              = 11
        cps                    = 100 2
        flags                  = IPv4
}
```

7. 导入操作系统
先挂载到/mnt下
```
mkdir /mnt/iso
mount -o loop /home/vinzor/ubuntu-12.04.5-server-amd64.iso /mnt/iso
```
然后import
```
cobbler import --path=/media/Ubuntu-Server\ 12.04\ LTS\ amd64/ --name=ubuntu12 --arch=x86_64    
```
**注：经尝试，在这一步就可以启动并且成功安装系统**
可以通过列出profile和distros
```
cobbler distro list
cobbler profile list
```

8. 创建系统
```
cobbler system add --name=test --profile=ubuntu12-x86_64
cobbler system list
cobbler system report --name=test
```
设置网络
```
cobbler system edit --name=test --interface=eth0 --mac=00:11:22:AA:BB:CC --ip-address=192.168.100.100 --netmask=255.255.255.0 --static=1 --dns-name=test.mydomain.com
```
网关需要单独添加
```
cobbler system edit --name=test --gateway=192.168.100.1 --hostname=test.mydomain.com
```

9. 使用cobbler启动tinycore linux：  
重新制作tinycore的镜像，在boot目录下增加一个`version`文件，内容为`tinycore`，以作标志文件使用。  
制作镜像参考：[http://wiki.tinycorelinux.net/wiki:remastering](http://wiki.tinycorelinux.net/wiki:remastering)
然后，需要修改cobbler的文件`/var/lib/cobbler/distro_signatures.json`，在文件的末尾部分修改如下：  
```
  "windows": {
  },
############被框起来的这一段表示是我们加入的内容，两行井号不需要输入
  "tinycore": {
   "test": {
    "signatures":["boot"],
    "version_file":"version",
    "version_file_regex":"tinycore",
    "kernel_arch":"",
    "kernel_arch_regex":"null",
    "supported_arches":["x86_64"],
    "supported_repo_breeds":["tcz"],
    "kernel_file":"vmlinuz",
    "initrd_file":"core.gz",
    "isolinux_ok":false,
    "default_kickstart":"",
    "kernel_options":"ipaddress=192.168.100.2",
    "kernel_options_post":"",
    "boot_files":[]
   }
  },
###########
  "generic": {
   "generic26": {
    "signatures":[],
    "version_file":"",
    "version_file_regex":"",
    "kernel_arch":"",
    "kernel_arch_regex":"",
    "supported_arches":["i386","x86_64"],
    "supported_repo_breeds":[],
    "kernel_file":"",
    "initrd_file":"",
    "isolinux_ok":false,
    "default_kickstart":"",
    "kernel_options":"",
    "kernel_options_post":"",
    "boot_files":[]
   }

  }
 }
}

```
修改完成后重启机器，然后就可以按上面cobbler启动的步骤启动tinycore linux了  
```
mount -o loop,ro TC-remastered.iso /mnt
 cobbler import --arch=x86_64 --path=/mnt/tiny --name=tiny
```
成功之后就可以用cobbler在另一台机器上面启动tinycore linux了。