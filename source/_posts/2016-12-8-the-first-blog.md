---

layout: post

title: "第一篇博客"

keywords: 博客，github page

description: 使用github page搭建博客

date: 2016-12-18 20:58

author: "尹傲雄"

categories: [工程向]

tags: [博客,github,教程]
---

第一次就写一下，自己在github上利用github page这个功能搭建博客的过程也算是一个教程吧。第一次写有什么不对的地方请大家指正啦。

<h2>准备工作</h2>

首先想要使用github page搭建一个博客你需要一些准备，也就是一些储备知识。你先要了解git的基本用法，这里推荐一些廖雪峰的博客，感觉写的很好很适合入门，我也是从他那里入门的。<a href="http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000">廖雪峰的git教程</a> 然后去github的官网申请一个账号，然后我们就可以同时利用git强大的功能和github提供的服务来写博客了



<h2>克隆远程仓库</h2>

首先在github上新建一个仓库叫做yourusername.github.io记得吧yourusername替换成你的用户名。为什么要建这样一个仓库呢，因为github默认把这个当作github page。当然你也可以用其他的，其实每个项目都可以有一个静态页面，这里我用的就是默认的了。然后使用`git clone....`这个命令把项目克隆到本地，后面接的是你项目的地址。写博客就是在这个里面了。



<h2>搭建本地环境</h2>

官方使用的博客工具是jekyll，我用的也是这个。不过正如github上所写的，因为windows不是官方支持的平台所以比较麻烦。官方推荐用的是Chocolatey来安装jekyll，这是一个类似与apt-get这一类的软件包管理器。Chocolatey的安装方法在它的官网上有，十分的简单。<a href="https://chocolatey.org/install">chocolatey官网安装</a> 记得也管理员的身份运行命令提示符，不然的话会安装失败。然后安装官方的教程配置环境。之后按照官方的文件配置环境就可以了。<a href="https://jekyllrb.com/docs/windows/#installation">官方文档</a> 这应该是最简单的方法了，也官方推荐的方法，当然你也可以手动安装。当初我因为网络的一些原因安装不了chocolatey，就是手动安装的。



<h2>尝试写博客</h2>

环境搭建好了就可以写博客了，你可以通过在git bash也可以在命令提示符里输入`jekyll new loge`这个命令来生成一个博客文件夹在所在的目录（还是感觉用git bash好一点）。结构大致是这样的：

1. 文件夹_layouts：用于存放模板的文件夹，里面有两个模板，default.html和post.html

2. 文件夹_posts：用于存放博客文章的文件夹，里面有一篇markdown格式的文章--2014-01-27-welcome-to-jekyll.markdown

3. 文件夹css：存放博客所用css的文件夹

4. .gitignore：可以删掉，后面会将项目添加到git项目，所以这个不需要了

5. _coinfig.yml：jekyll的配置文件，里面可以定义相当多的配置参数，具体配置参数可以参照其官网

6. index.html：项目的首页

根据的你的实际需要创建你需要的文件夹。



使用`jekyll server -watch`这个命令就可以在本地的浏览器查看一下效果了，-watch命令可以让你的修改实时的显示出来。至于具体的过程我也在学习中就不误人子弟了，你可以去jekyll的官网上看教程，也可以看网上别人的教程。<a href="http://jekyll.com.cn/">中文官网</a>当然在学习了基本的一些方法之后想要有一个漂亮的博客就可以去使用一下别人写好的模版了。然后根据自己的需要去改造了，在这个过程中不断的学习进步。这些模板在github上有很多，官方的网站上也有。<a href="http://jekyllthemes.org/">主题官网链接</a>



<h2>配置自己的个性化博客域名</h2>

github的默认博客域名是yourusername.github.io。想要一个个性化的域名的话也很简单。首先你要买一个域名，如果你是高校的学生的话可以去抢一下腾讯云的学生优惠资格，在上学期间每年可以有一个.cn的域名，一个月1元就可以有一个云服务器了。<a href="https://www.qcloud.com/">腾讯云</a> 有了自己的域名之后先在解析里添加一个cname记录把你的域名向yourusername.github.io这个域名，然后在你的博客的根目录，也就是youusername.github.io这个文件夹里添加一个CANME文件在里面写上你的个性化域名。然后把它提交到远程仓库就可以了了，等解析完成了之后。在地址栏输入你的个性化域名就可以看到自己的博客了。



有兴趣的话可以去我的博客看一下，现在还是很简陋啦，也是直接借用别人的模板没做什么修改，附上链接<a href="http://blog.yinaoxiong.cn">尹傲雄的博客</a>
