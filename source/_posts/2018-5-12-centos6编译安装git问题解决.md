---

layout: post

title: "centos6编译安装git错误解决"

keywords: centos6,git,错误

description: centos6编译安装git错误解决

date: 2018-5-12 00:43

author: "尹傲雄"

categories: [linux]
---
　　今天想在centos6的服务器上安装使用gitea，但是报错git版本太低，而yum源自带的git比较古老需要自行编译安装。编译安装的过程中遇到了一些问题记录一下。
编译安装时报错如下：

imap-send.c: 在函数‘ssl_socket_connect’中: imap-send.c:291: 警告：不建议使用‘TLSv1_method’(声明于 /usr/local/include/openssl/ssl.h:1612) imap-send.c: 在函数‘cram’中: imap-send.c:865: 错误：‘hmac’的存储大小未知 imap-send.c:880....

原因是由于系统里安装了多版本的openssl ,删除/usr/local/include/openssl目录下的所有文件即可，这样之后编译就不会出错了。