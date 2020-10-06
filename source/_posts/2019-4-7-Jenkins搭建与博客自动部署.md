---

layout: post

title: "Jenkins搭建与博客自动部署"

keywords: Jenkins,ci,又拍云,对象存储

description: Jenkins搭建与博客自动部署

date: 2019-4-7 14:04

author: "尹傲雄"

categories: [杂类]

---

# 起因

最开始我是使用CircleCI来进行博客的自动部署的，但是在部署的时候使用又拍云的upx同步博客到对象存储的时候经常出现504（网关超时）错误最后导致失败。很容易就可以想应该是因为众所周知的网络原因，但是说好的全球cdn呢:sweat:，这不免让我对它的全球cdn的实力有所怀疑:joy:。于是就打算自己搭Jenkins服务器在国内构建部署解决问题，顺带学习一下。

# Jenkins安装

Jenkins的详细安装方法可以参考官方的中文文档：[https://jenkins.io/zh/doc/book/installing/](https://jenkins.io/zh/doc/book/installing/)。这里我使用docker进行安装，安装时使用的docker-compose.yml配置文件如下所示：

```yaml
version: "3"

services:
  kms:
    image: jenkinsci/blueocean
    user: root
    ports:
      - 8080:8080
    volumes:
      - jenkins-data:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
    restart: always
  
volumes:
  jenkins-data:
```

运行的时候先别急着`docker-compose up -d`，先`docker-compose up`一下，这时如果防火墙安全组什么的设置没有问题的话可以在8080端口访问到了。在看终端的输出信息的时候记录一下初始安装的随机密码，初始安装的时候需要用到，之后就是根据网站的提示一直下一步下一步了。安装配置结束后可以让docker转到后台运行。

# 使用前的配置

## 反向代理

直接使用8080端口当然也是可以的，但是明显看起来有点丑（主要是逼格不够高:smirk:），这个这个时候我们就需要用到反向代理来处理一下了。这里我使用的是caddy，caddy的配置十分简单，两行行实现https自动申请续期与透明转发，我的Caddyfile如下所示。

```
https://ci.yinaoxiong.cn {
    gzip
    log /var/log/caddy/ci.log
    tls aoxiongyin@qq.com
    proxy / localhost:8080 {
        transparent
    }
}

```
## 添加虚拟内存

安装的时候看到官方对于小团队的推荐硬件配置自己这个腾讯云的学生机也算是勉强达到了，然后一顿操作猛如虎，然后build的时候out of memory甩脸上。然后用`free -h`看了一下发现自己的交互内存配置的是0，赶紧加个1G的交换内存试一下看看，这里使用交换文件添加交换内存。具体操作如下所示：

```shell
#创建一个大小为1G的文件
sudo fallocate -l 1G /swapfile
#锁定root权限
sudo chmod 600 /swapfile
#标记文件为交换空间
sudo mkswap /swapfile
#启用交换空间
sudo swapon /swapfile
#查看设置是否生效
free -h
#配置写入fstab永久生效
#先备份文件
sudo cp /etc/fstab /etc/fstab.bak
#写入文件
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

# 配置构建与部署

Jenkins的使用这个问题过于庞大，大家可以去官网看它的文档学习一下，下面分享一下我的仅分享一下我自己的配置，这个博客的配置文件可以在这个链接中查看：[Jenkinsfile](https://raw.githubusercontent.com/YinAoXiong/blog/master/Jenkinsfile)。目前使用 的配置文件如下所示：

```
pipeline {
  agent {
    docker {
      image 'circleci/ruby:2.6.0'
    }

  }
  stages {
    stage('build') {
      steps {
        sh 'gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/'
        sh 'sudo gem install bundler -f'
        sh 'bundle config mirror.https://rubygems.org https://gems.ruby-china.com'
        sh 'bundle install --jobs=4 --retry=3 --path vendor/bundle'
        sh 'bundle exec jekyll build'
      }
    }
    stage('deploy') {
      steps {
        sh 'wget -c http://collection.b0.upaiyun.com/softwares/upx/upx-linux-amd64-v0.2.3'
        sh 'mv upx-linux-amd64-v0.2.3 upx'
        sh 'chmod +x upx'
        sh './upx login yax-blog ${upx_USR} ${upx_PSW}'
        sh './upx sync _site/ / --delete'
      }
    }
  }
  environment {
    upx = credentials('upyun-account')
  }
}
```

直接使用CircleCI家的docker镜像规范环境，因为本来就是从他们家迁移的就没什么好改的了。构建的时候为ruby设置中国源加速访问。最后使用又拍云的upx工具同步到对象存储中。

# 添加徽标

原来使用CircleCI直接提供了构建的徽标链接来满足用户的使用（装逼）需要，但是Jenkins本身没有提供这个功能。好在万能的网友早已明白人们对于徽标的使用（装逼）需求，并创建了一个生成各种徽标的开源项目。项目的地址是：[badges/shields](https://github.com/badges/shields)，项目的一个demo网址是：[https://shields.io/](https://shields.io/)。

在build中点击Jenkins按照要求填写好你就可以获得你想要的徽标了，我的填写方式如下所示。

![config](https://cdn.yinaoxiong.cn/image/posts/2019-4-7/config.png)

将my-blog更换成你自己的job名称应该就可以了，需要注意的是Jenkins默认是不允许匿名查看的，所以直接上去一顿操作是不行的。需要先用管理员账号在 系统管理》全局安全设置》访问控制 里面勾选允许匿名只读访问才行。如果你不想这么做也可以自己使用badges/shields搭建服务，然后按照这个文档：[Server Secrets](https://github.com/badges/shields/blob/master/doc/server-secrets.md)，设置好具有权限的Jenkins账号密码，这样就不用把构建信息公开了。





