#Ansible：Up and running  
##第一章 概述  
* Ansible中将脚本称作playbook，它描述了哪些主机需要哪些配置，执行命令如下：
```ansible-playbook webservers.yml```  
Playbook是基于yaml开发的  
* 使用Ansible安装的服务器需要安装**ssh和python2.5**（或者更新版本），除此之外，不需要任何agent或其他软件。控制主机需要安装Python2.6或更高版本。  
* Ansible是基于推送模式进行更新。  
* Ansible模块是声明式的，具有幂等性。  
* Ansible使用Jinja2作为模板引擎  
* ansible命令并不常用，一般用于点对点的、一次性的事情上  
  
## 第二章 playbook
* 示例web-notls.yml   
  
```
- name: Configure webserver with nginx
  hosts: webservers
  sudo: True
  tasks:
    - name: install nginx
      apt:name=nginx update_cache-yes
	
	- name: copy nginx config file
	  copy: src=files/nginx.conf dest=/etc/nginx/sites-available/default

	  - name: enable configuration
	  file: >
		dest=/etc/nginx/sites-enabled/default
		src=/etc/nginx/sites-available/default
		state=link

	- name: copy index.html
	  template: src=templates/index.html.j2 dest=/usr/share/nginx/html/index.html
	  mode=0644

	- name: restart nginx
	  service: name=nginx state=restarted
```  
Ansible官方文档示例中，向模块传递参数时候使用yes和no，其他地方使用True和False
按照惯例，Ansible会将一般文件放在名为files的子目录中，将Jinja2模板文件放在templates的子目录中。  

* inventory文件使用.ini文件格式，下例表示testserver属于webservers群组  
```  
[webservers]
testserver ansible_ssh_host=127.0.0.1 ansible_ssh_port=2222
```  

* 类Unix操作系统中的文本文件第一行前两个字符为#！时叫做shebang，操作系统的程序载入器会分析#！后的内容，将这些内容作为解释器指令，并将含有这个shebang的文件及其路径作为该解释器的参数。  

### YAML知识：  
* 文件的起始  
YAML文件以三个减号`---`开头以标记文档的开始，不过如果你忘记在playbook文件开头敲三个减号，不会影响Ansible的运行。  
* 注释  
与Shell和Python相同，用井号#  
* 字符串  
一般来说YAML字符串不必用引号括起来，不过使用也没有问题  
* 布尔型  
YAML使用多种释义，包括true，yes等  
* 列表  
列表使用减号`-`作为分隔符，例如：  
```
- My Fail Lade
- Oklahoma
- The Pair
```
也可以使用内联格式，就像：  
```
[My Fail, Lade, Oklahoma, The Pair]
```
* 字典,在YAML中叫映射：  
```
address: 742 Evergreen Terrace
cite: Springfield
state: North Takoma
```
也可以使用内联格式：  
```
{address: 742 Evergreen Terrace，cite: Springfield，state: North Takoma}
```
* 折行  
YAML中使用大于号（>）来标记折行，例如： 
```
address: >
	Department of COmputer
```  
YAML解释器会把换行符转换成空格

* playbook就是一组play组成的列表，每个play必须包含下面两项：  
  * host：需要配置的一组主机  
  * task：需要在这些主机上执行的任务  
除此之外，还包含一些可选配置，最常见的三个为：  
  * name 一段注释，用来描述这个play做什么
  * sudo 如果为真，Ansible会在运行每个task时都是用sudo命令切换为root用户
  * vars 变量与其值组成的列表

*　模块　　
模块是由Ansible包装后在主机上执行一系列操作的脚本，例如：  
  * apt
  使用apt包管理工具安装或删除软件包
  * copy
  将一个文件从本地复制到主机上
  * file
  设置文件、符号链接或者目录的属性
  * service
  启动、停止或者重启一个服务
  * template
  从模板生成一个文件并复制到主机上
`ansible-doc`命令可用于查看模块的文档

* 当运行ansible-playbook时，Ansible变回输出它在play中执行的每一个task的状态信息。有些任务状态显示changed，而其他状态都是ok。
 
* 生成TLS证书：
```
mkdir files
openssl req -x509 -nodes -days 3650 -newkey rsa:2048 \
  -subj /CN=localhost \
  -keyout files/nginx.key -out files/nginx.crt
```
该命令会在files目录下生成文件nginx.key和nginx.crt。

* 变量  
我们定义了变量之后可以使用{{}}来引用他们  
如果刚好在模块声明之后引用变量，必须使用引号，否则会发生解析错误：  
```
command:"{{ myapp }}"
```
参数中含有冒号也会有类似问题

* handlers与task想死，但是只有被task通知的时候才会运行。如果Ansiblel识别到task改变了系统的状态，task就会触发通知机制。使用notify来通知。handler指挥在所有任务执行后执行，而且即使被通知了多次，也只会执行一次。它会按照play中定义的顺序执行，而不是被通知的顺序。

## 第三章 inventory：描述你的服务器
* Ansible可管理的主机集合叫做inventory。在Ansible中，描述主机的默认方法是列在inventory文件中。
* 群组：Ansible自动定义了一个群组叫all，包含所有inventory中的主机  
可以定义群组的嵌套，比如定义一个包括web和task两个群组的“django”群组  
```
[django:children]
web
task
```  
我们可以用编号范围指代服务器:  
```
[web]
web[01:20].example.com
web-[a-t].example.com
```

* 可以针对主机和群组设置变量，然后在inventory内部和playbook中使用
```
quebec.example.com color=purple
```
群组变量设定：使用[<group name>:vars]组织
```
[all:vars]
ntp_server=ntp.ubuntu.com
```
在inventory文件中只能为变量指定布尔型或字符串。  

* 可以为每个主机和群组创建独立的变量文件，Ansible会使用YAML格式来解析这些变量文件。  
Ansible会在`host_vars`的目录中寻找主机变量文件，在名为`group_vars`的目录中寻找群组变量文件。Ansible假设这些目录在包含playbook的目录下或者与inventory文件相邻的目录下。
* 如果inventory文件标记为可执行，那么Ansible会假设这是一个动态inventory脚本，并且会执行它而不是读取它的内容。  
想要将一个文件标记为可执行，使用chmod+x命令。  
一个动态inventory脚本必须支持如下两个命令行参数：
  * --host=<hostname> 用来列出主机的详细信息
  * --list 用来列出群组

* 展示主机详细信息  
  Ansible会按照如下形式调用inventory脚本来获取单台主机的详细信息：  
```
./dynamic.py --host-vagrant2
```
输出应该包含所有主机特定的变量，也包括行为参数，类似：  
```
{ "ansible_ssh_host": "127.0.0.1", "ansible_ssh_port": 2200,
  "ansible_ssh_user": "vagrant"}
```

* 列出群组  
动态inventory脚本需要能够列出所有的群组和每台主机的详细信息。例如我们的脚本叫做dynami.py，Ansible将按照如下方式调用它来列出所有的群组：  
```shell
$./dynamic.py --list
```
输出是一个JSON对象，该JSON对象的名字为群组名，值为由主机的名字组成的数组。

* debug模块可以用来打印任意信息
```
-debug: var=myvarname
```

* 确认模块返回形式最简单的方法就是设置一个register变量，然后使用debug模块输出这个变量：
```
- name: show return value of command module
  hosts: server1
  tasks:
    - name: capture output of id command
      command: id -un
      register: login
    - debug: var=login
```

* 当Ansible采集fact的时候，它会连接到主机收集各种详细信息：CPU架构、操作系统等，这些信息保存在被称为fact的变量中，fact与其他变量的行为一模一样。Ansible使用一个名为setup的特殊模块来实现fact的手机，不需要再playbook中去调用，因为他会在采集fact时自动调用。我们也可以用filter参数对fact名进行过滤。

* 如果模块返回一个ansible_facts的key，那么Ansible将会根据对应的value创建相应的变量，并分配给相对应的主机。

* 可以将一个或多个文件放置在目标主机的/etc/ansible/fact.d目录下，如果该目录中的文件是一下形式的，Ansible会自动识别：
  * .ini格式
  * JSON格式
  * 可以不加参数形式执行，并在标准输出中输出JSON的可执行文件
    
* 用于安装系统软件包的task：
```
- name: install apt packages
  apt: pkg={{ item }} update_cache=yes cache_valid_time=3600
  sudo: True
  with_items:
    - git
    - libjpeg-dev
    - libpq-dev
```
使用with_items语句来安装多个软件包效率更高。

* 可以用字典或者字符串形式传递参数

* 使用命令行参数传递密码会造成使用ps命令的时候密码出现在进程列表当中

