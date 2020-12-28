---

layout: post

title: "在linux上使用git+github传文件到云服务器"

keywords: Linux，git，github

description: 在linux上使用git+github传文件到云服务器

date: 2016-12-22 22:28

author: "尹傲雄"

categories: [工程向]

tags: [Linux,git,GitHub]

---
因为网站的代码都是在本地写的而写完之后要放到云服务器上去，这个就比较麻烦了。网上有人说可以自己搭一个fpt来传文件，但是这个也不是一个很大的项目，于是就想通过git和github来实现一个简单的传输，也方便管理。具体步骤如下：

 1. 安装Git，使用命令 `sudo apt-get install git`
 2. 生成ssh key，使用命令 “ssh-keygen -t rsa -C "your_email@youremail.com"”，your_email是你的email .公匙一般默认在用户的/home/.ssh/id_rsa.pub文件里面。我的比较奇怪是在/root/.ssh/rsa.pub里所以需要root权限。
 3. 进入到github，进入Account Settings，左边选择SSH Keys，Add SSH Key,title随便填，粘贴key。
 4. 测试ssh key是否成功，使用命令“ssh -T git@github.com”，如果出现You’ve successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。
 5. 配置Git的配置文件，username和email

                 git config --global user.name "your name"   //配置用户名

                 git config --global user.email "your email"    //配置email


这样git的配置和github的链接就配好了，使用`git clone`命令就可以克隆自己的项目了。感觉这样子做还是很方便，也很快捷。其他的有关git和github的操作可以去看廖雪峰的博客，百度一下就有了。
