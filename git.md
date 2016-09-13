#git
* git是分布式版本控制系统，不需要中央电脑
* **安装**
**ubuntu**安装git：
```
sudo apt-get install git
```
老版的ubuntu可能要运行：
```
sudo apt-get install git-core
```
**windows**安装git：
[https://git-for-windows.github.io](https://git-for-windows.github.io "git for windows")
安装成功后需要最后一步设置：
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
* 创建版本库：
```
git init
```
把当前目录变成git可以管理的仓库
**注：**git只能追踪纯文本格式文件的改动，图片视频等二进制文件只能追踪大小变化，word文件也是二进制文件，无法追踪。

* 把文件添加到git库流程：
**1.**添加文件到暂存区(stage)
```
git add <file>
```
git add \* 可以添加**所有**文件
**2.**把暂存区的所有改动提交到当前分支
```
git commit -m 'update message'
```
如果修改没有add到暂存区，就不会被commit

* 查看信息
```
git status
```
掌握版本库当前状态
```
git diff <file>
```
查看文件修改

* 版本回退
```
git log
```
查看历史提交记录
```
git reset --hard HEAD^
```
回退一个版本
```
git reset --hard HEAD~100
```
回退100个版本
```
git reset --hard <commit-id>
```
回退到指定版本
```
git reflog
```
记录每一次的命令，可以看到相应的commit-id
**注：**HEAD指向的版本就是当前版本
* 撤销修改
```
git checkout -- <file>
```
撤销修改到最后一次git commit或者git add时的状态
```
git reset HEAD <file>
```
可以把暂存区的修改撤销掉，重新放回工作区
* 删除文件
```
rm <file>
git rm <file>
```
在手动删除文件后也要在git里面删除

* 使用github
**1.**创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
```
ssh-keygen -t rsa -C "youremail@example.com"
```
如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
**2.**登陆GitHub，打开“Account settings”，“SSH Keys”页面，然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容。

* 关联远程仓库
```
git remote add origin git@github.com:michaelliao/learngit.git
```
添加后，远程库的名字就是```origin```，也可以改成别的。
```
git push -u origin master
```
第一次推送```master```分支时，加上了```-u```参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

* 克隆远程仓库
```
git clone git@github.com:michaelliao/gitskills.git
```

* 创建与合并分支
```
git checkout -b dev
```
创建dev分支并切换到dev分支
```git checkout```命令加上```-b```参数表示创建并切换，相当于以下两条命令：
```
git branch dev
git checkout dev
```
查看当前分支：
```
git branch
```
当前分支前面会有一个\*号
```
git merge dev
```
把dev分支的工作成果合并到master分支上
```
git branch -d dev
```
删除dev分支
```
git log --graph --pretty=oneline --abbrev-commit
```
用带参数的```git log```也可以看到分支的合并情况
```
git merge --no-ff -m "merge with no-ff" dev
```
```--no-ff```参数，表示禁用```Fast forward```,Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
```
git stash
```
Git提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续push工作
```
git stash list
```
```stash list``` 能够查看储存的工作现场,有两种方法可以恢复：一是用```git stash apply```恢复，但是恢复后，```stash```内容并不删除，你需要用```git stash drop```来删除；另一种方式是用```git stash pop```，恢复的同时把```stash```内容也删了。
```
git stash apply stash@{0}
```
可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash

* 如果要丢弃一个没有被合并过的分支，可以通过```git branch -D <name>```强行删除。

* 要查看远程库的信息，用```git remote```
  或者，用```git remote -v```显示更详细的信息

* 创建远程origin的dev分支到本地
```git checkout -b dev origin/dev```

* 用```git pull```把最新的提交从origin/dev抓下来

* 设置```dev```和```origin/dev```的链接
```git branch --set-upstream dev origin/dev```

* 在Git中打标签非常简单，首先，切换到需要打标签的分支上,然后，敲命令```git tag <name>```就可以打一个新标签。可以用命令```git tag```查看所有标签。
  也可以用```git tag <name> <commit-id>```打标签
  可以用```git show <tagname>```查看标签信息
  还可以创建带有说明的标签，用```-a```指定标签名，```-m```指定说明文字
  还可以通过```-s```用私钥签名一个标签
  如果标签打错了，也可以删除：
```
git tag -d v0.1
```
  如果要推送某个标签到远程，使用命令```git push origin <tagname>```
  一次性推送全部尚未推送到远程的本地标签：
```
git push origin --tags
```
  如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除:
```
git tag -d <tagname>
```
  然后，从远程删除。删除命令也是push，但是格式如下：
```
git push origin :refs/tags/<tagname>
```

* 如何参与一个开源项目呢？比如人气极高的bootstrap项目，这是一个非常强大的CSS框架，你可以访问它的项目主页https://github.com/twbs/bootstrap ，点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone。
  一定要从自己的账号下clone仓库，这样你才能推送修改。如果从bootstrap的作者的仓库地址git@github.com:twbs/bootstrap.git克隆，因为没有权限，你将不能推送修改。
Bootstrap的官方仓库twbs/bootstrap、你在GitHub上克隆的仓库my/bootstrap，以及你自己克隆到本地电脑的仓库，他们的关系就像下图显示的那样：  

![](http://www.liaoxuefeng.com/files/attachments/001384926554932eb5e65df912341c1a48045bc274ba4bf000/0)
  如果你想修复bootstrap的一个bug，或者新增一个功能，立刻就可以开始干活，干完后，往自己的仓库推送。
  如果你希望bootstrap的官方库能接受你的修改，你就可以在GitHub上发起一个pull request。当然，对方是否接受你的pull request就不一定了。

* Git还有很多可配置项。比如，让Git显示颜色，会让命令输出看起来更醒目：
```
git config --global color.ui true
```
* 在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore
忽略文件的原则是：
**1.**忽略操作系统自动生成的文件，比如缩略图等；
**2.**忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
**3.**忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。
  可以用```git check-ignore```命令检查

* 我们只需要敲一行命令，告诉Git，以后st就表示status：
```
git config --global alias.st status
```

* 配置Git的时候，加上```--global```是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。每个仓库的Git配置文件都放在**.git/config**文件中

##搭建Git服务器
第一步，安装git:
```
sudo apt-get install git
```
第二步，创建一个git用户，用来运行git服务：
```
sudo adduser git
```
第三步，创建证书登录：

收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。

第四步，初始化Git仓库：
先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：
```
sudo git init --bare sample.git
```
Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：
```
sudo chown -R git:git sample.git
```
第五步，禁用shell登录：

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：
```
git:x:1001:1001:,,,:/home/git:/bin/bash
```
改为：
```
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```
这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。

第六步，克隆远程仓库：

现在，可以通过git clone命令克隆远程仓库了，在各自的电脑上运行：
```
git clone git@server:/srv/sample.git
```

摘抄自[http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000 "廖雪峰git教程")