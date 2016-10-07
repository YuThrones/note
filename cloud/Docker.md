#Docker
##简介
* Docker依赖于“写时复制”模型，修改应用程序非常迅速，拥有很高的性能
* Docker的核心组件：
  * Docker客户端和服务器
  * Docker镜像
  * Registry
  * Docker容器
* Docker是一个客户机-服务器（C/S）架构的程序，提供了一个命令行工具以及一整套RESTful API，可以从本地客户端连接另一台宿主机的Docker守护程序。
* Docker镜像是基于联合（Union）文件系统的一种层式的机构，由一系列指令一步一步构建出来。
* Registry用来保存用户的镜像，分为公共跟私有两种。
* 容器是基于镜像启动起来的，我们只需要把应用程序或者服务打包放进去。
* Docker的技术组件：
  * 一个原生的Linux容器格式libcontainer，或者很流行的容器平台lxc，前者是Docker容器默认格式。
  * Linux内核的命名空间（Namespace），用于隔离文件系统、进程和网络：
    * 文件系统隔离：每个容器都有自己的root文件系统
    * 进程隔离：每个容器都运行在自己的进程环境中
    * 网络隔离：容器间的虚拟网络接口跟IP地址都是分开的
    * 资源隔离和分组：使用cgroups（即control group，Linux的内核特性之一）将CPU和内存之类的资源独立分配给每个Docker容器
    * 写时复制：文件系统都是用写时复制创建，这意味着文件系统是分层的、快速的，而且占用磁盘空间更小
    * 日志：容器产生的STDOUT、STDERR和STDIN这些IO流都会被收集并计入日志，用来进行日志分析和故障排错
    * 交互式Shell：用户可以创建一个伪tty终端，将其连接到STDIN，为容器提供一个交互式的Shell

##Docker安装
* 前提条件：
  * 64位CPU的计算机
  * Linux3.8或者更高版本的内核
  * 内存必须支持一种适合的存储驱动（storage driver），例如:  
      *  Device Manager
      *  AUFS
      *  vfs
      *  btrfs
      *  默认存储通常是Device Mapper
  * 内核必须支持并开启cgroup和命名空间（namespace）功能
  * 检查前提条件：  
      * 用```uname```命令来检查内核版本  
      ```uname -a```
      * 检查Device Mapper    
      ``` ls -l /sys/class/misc/device-mapper ```  

* 安装Docker：  
添加Docker的APT仓库：  
```sudo sh -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"```  
检查是否有curl命令：  
```whereis curl```  
如果没有，需要安装：  
```sudo apt-get -y install curl```  
接下来添加Docker的GPG仓库：   
```curl -s https://get.docker.io/gpg | sudo apt-key add -```  
可能是```curl -fsSL https://get.docker.com/gpg | sudo apt-key add -```
之后需要更新软件源：  
```sudo apt-get update```  
然后就可以安装Docker软件包了：  
```sudo apt-get install lxc-docker```  
在Ubuntu中，如果使用UFW，即Uncomplicated Firewall，需要对其做一点修改，因为Docker使用一个网桥管理容器中的网络，默认情况下UFW会丢弃所有转发的包，需要在UFW启动数据包转发。将```/etc/default/ufw``` 中的 ```DEFAULT_FORWARD_POLICY="DROP"``` 改成 ```DEFAULT_FORWARD_POLICY="ACCEPT"``` ,然后 ```sudo ufw reload```

* 也可以简单的使用远程安装脚本安装docker：
```curl -fsSL https://get.docker.com/ | sh```

* 配置Docker守护进程：  
  运行Docker守护进程时，我们可以用 ```-H``` 标志调整守护进程绑定监听接口的方式，比如要想绑定到网络接口：  
``` sudo /usr/bin/docker -d -H tcp://0.0.0.0:2375```  
  如果不想每次都加上 ```-H``` ，可以通过设置环境变量来省略：
``` export DOCKER_HOST="tcp://0.0.0.0:2375" ```
  可以通过status检查守护进程是否在运行：  
``` sudo status docker ```
  可以使用stop和start来停止和启动Docker守护进程

##Docker入门
* 查看Docker程序是否存在：  
``` sudo docker info ```

* 创建容器:  
```sudo docker run -i -t ubuntu /bin/bash```  
```-i``` 保证STDIN是开启的， ```-t``` 告诉Docker为容器创建一个伪tty终端。这是创建一个交互式的容器最基本的参数
Docker会先检查本地是否存在ubuntu镜像，如果没有，就会链接官网维护的Docker Hub Registry，查看并下载
在创建完成后，Docker就会执行容器中的 ```/bin/bash``` 命令，这时我们就可以看到容器中的shell了。

* 容器使用：
  容器的hostname就是容器的ID，我们可以在容器中进行我们平时在主机中做的操作，在做完之后，输入 ```exit``` 退出容器，在退出之后容器停止运行，但是我们可以使用 ```docker ps -a``` 查看当前系统中容器的列表
  有三种方式可以指代唯一容器，短ID，长ID跟名称

* 容器命名：
  可以使用 ```--name```标志给容器命名：
``` sudo docker run --name bob_the_contariner -i -t ubuntu /bin/bash```
  容器名称必须是唯一的，如果我们试图创建同名的容器将会失败，我们可以先用 ```docker rm``` 删除已有同名容器再来创建新的。

* 重新启动新的容器：
```sudo docker start bob_the_container```

* 附着到容器上：
```sudo docker attach bob_the_container```

* 创建守护式容器：
``` sudo docker run --name daemon_dave -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done" ```  
  创建时使用了 ```-d``` 参数，Docker会将容器放到后台运行

* 查看容器行为：
``` sudo docker logs daemon_dave```  
  我们可以用 ```docker logs```命令获取容器的日志，也可以加 ```-f```参数进行监控，类似 ```tail -f```，也可以使用 ```-t``` 加上时间戳

* 查看容器内进程：
``` sudo docker top daemon_dave```

* 在容器内运行进程：
  在容器内运行进程有两种，后台任务和交互式任务
  后台任务运行例子如下：  
```sudo docker exec -d daemon_dave touch /etc/new_config_file```
  交互式任务运行例子如下：
```sudo docker -i -t daemon_dave /bin/bash```
  和运行交互式容器一样，这里的 ```-t``` 和 ```-i```创建了TTY并捕捉STDIN

* 停止守护式容器：
```sudo docker stop daemon_dave``` 
  如果想快速停止某个容器，也可以使用 ```docker kill``` 命令

* 自动重启容器：
  如果由于某种原因导致容器停止运行退出，我们可以通过设置 ```--restart```标志，让Docker自动重启该容器。 ```--restart```会检查容器的退出代码，据此决定是否重启容器，默认行为是Docker不会重启容器。
``` 
sudo docker run --restart=always --name daemon_dave -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
```
  我们还可以将这个标志设为 ```on-failure```，这样只有容器退出代码非0值时才会重新启动,另外， ```on-failure``` 还可以接受一个可选的重启次数参数  ```--restart=on-failure:5```  

* 深入容器：
  我们可以通过 ```docker inspect```  获得更多容器的信息：
```sudo docker inspect daemon_dave```
  也可以使用 ```-f``` 或者```--format```参数来选定查看结果：
```sudo docker inspect --format '{{ .State.Running }}' daemon_dave```
  还可以指定多个容器，并显示每个容器的输出结果

* 删除容器
  可以使用小技巧删除所有容器：
```docker rm `docker ps -a -q` ```
  ```-q``` 标志表示只需要返回ID不需要其他信息，通过ID删除所有容器

##使用镜像和仓库
* Docker镜像是由文件系统叠加而成，最底层是一个引导文件系统，即**bootfs**，当一个容器启动后，引导文件系统会被卸载，流出更多空间供initrd磁盘镜像使用。  
  Docker镜像的第二层是root文件系统**rootfs**，它可以是一种或多种操作系统（如Debian或者Ubuntu），在Docker里，它永远是只读状态，Docker利用联合加载技术在它上面加载更多只读文件系统，联合加载指一次同时加载一个或多个文件系统，但是在外面看起来只能看到一个文件系统。  
  Docker底层是基础镜像，最顶层是读写层，采用**写时复制**。

* 列出镜像：
```sudo docker images```  
  本地镜像都保存在 ```/var/lib/docker```目录下，可以在 ```/var/lib/docker/containers``` 目录下看到所有容器。
  镜像从仓库下载下来，仓库存在于Registry中，默认的Registry是由Docker公司运营的公共Registry服务，即**Docker Hub**
  可以使用 ```docker pull``` 拉取仓库中所有内容，例如
``` sudo docker pull ubuntu``` 
  为了区分同一个仓库中的不同镜像，Docker提供了标签（tag）功能，我们可以通过在仓库名后面加上**冒号和标签名**来指定该仓库中的某一个镜像。
  Docker有两种仓库，**用户仓库**和**顶层仓库**，用户仓库由Docker用户创建，顶层仓库由Docker内部的人管理。用户仓库由用户名加仓库名组成，如 ```jamtur01/puppet```,而顶层仓库只包含仓库名部分。

* 查找镜像：  
```sudo docker search puppet```
  查找Docker Hub上公共的可用镜像

* 构建镜像：  
  构建镜像有两种方法：
  1. 使用docker commit命令
  2. 使用docker build 和 Dockerfile文件（推荐）
  可以使用docker login命令登录到Docker Hub
  在对镜像进行操作之后执行docker commit命令，会提交修改
```
sudo docker commit -m="A new custom image" --author="James Turnbull" \
4aab3asdfas jamtur02/apache2:webserver
```
  `-m`选项指定提交信息，`--author`列出作者信息，接着指定了要提交的容器ID，并为该镜像指定了一个webserver标签。
  `Dockerfile`示例：
```
# Version:0.0.1
FROM ubuntu:14.04
MAINTAINER James Turnbull "james@example.com"
RUN apt-get update
RUN apt-get install -y nginx
RUN echo 'Hi, I am in your container' \
	>/usr/share/nginx/html/index.html
EXPOSE 80
```
  该Dockerfile由一系列指令和参数组成，每条指令都**必须为大写字母**，且后面要跟随一个参数，指令会按顺序从上到下执行
  RUN命令会在当前镜像中执行，默认会用/bin/sh -c执行
  EXPOSE指令向外部公开端口
  ENV指令设置环境变量
  ADD复制文件到docker镜像

* docker自动使用缓存，如果不想用，可以使用 `--no-cache` 标识
  可以使用docker history命令查看构造过程

* docker rmi命令可以删除一个镜像

* 从容器安装一个registry
```
sudo docker run -p 5000:5000 registry
```

* 创建一个容器
```
sudo docker run -d -p 80 --name website \ 
-v $PWD/website:/var/www/html/website：ro \
jamtur01/nginx nginx
```
-v是一个新选项，允许我们将宿主机的目录作为卷挂在到容器里。
卷是一个在一个或者多个容器内被选定的目录，可以绕过分层的联合文件系统（Union File System），为Docker提供持久数据或者共享数据。这意味着对卷的修改会直接生效，并绕过镜像。当提觉或者创建镜像时，卷不被包含在镜像里。卷可以在容器中共享，即便容器停止，卷里的内容依旧存在。
参数-v指定了卷的源目录和容器里的目的目录，这两个目录通过：来分隔，如果目的目录不存在，Docker会创造一个。
也可以在目的目录后面加上rw或者ro来指定目的目录的读写状态。

* 在安装Docker时，会创建一个新的网络接口，名字是Docker0。每个Docker容器都会在这个接口上分配一个IP地址，docker0接口有符合RFC1918的私有IP地址，范围是172.16~172.30。接口本身地址172.17.42.1是这个Docker网络的网关地址，也是所有Docker容器的网关地址。

* 接口docker0是一个虚拟的以太网桥，用于连接容器和本地宿主网络。如果进一步查看Docker宿主机的其他网络接口，会发现一系列以veth开头的接口。Docker每创建一个容器就会创建一组互联的网络接口。这组接口其中一端作为容器里的eth0接口，而另一端统一命名为类似vethec6a这种名字，作为宿主机的一个接口。

* 