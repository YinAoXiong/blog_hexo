---
layout: post

title: "cargo-generate generate 在WSL2下报错Permission denied解决"

keywords: cargo-generate,WSL2,wrangler generate

description: "cargo-generate generate 在WSL2下报错Permission denied解决"

date: 2022-2-15 12:34

author: "尹傲雄"

categories: [工程向]

tags: [win10,WSL2]

---

#  起因

最近下一个国外的数据集有点慢，想用 https://github.com/xiaoyang-sde/reflare 这个项目用 cloudflare worker 来反向代理加速一下，但是在进行到用官方提供的模板生成项目的时候一直报错。执行的命令如下所示：

```shell
wrangler generate reflare-app https://github.com/xiaoyang-sde/reflare-template
cd reflare-app
npm install
```

报错信息如下所示：

```shell
Creating project called `reflare-app`...
warning: spurious network error (2 tries remaining): failed to mmap. Could not write data: Permission denied; class=Os (2)
warning: spurious network error (1 tries remaining): failed to mmap. Could not write data: Permission denied; class=Os (2)
Error:  Git Error: failed to clone into: /mnt/d/project/web/reflare-appdhwIOA
Error: tried running command:
/home/ubuntu/.cache/.wrangler/cargo-generate-0.5.0/cargo-generate generate --git https://github.com/xiaoyang-sde/reflare-template --name reflare-app --force
exited with exit status: 1
```

#  解决过程

我开始还以为是 GitHub 的网络问题，后面发现我开代理也没用，而且直接用 git 克隆这个项目也是没问题的。后面我想是不是 npm 安装的 wrangler 有问题，我又安装了一下 rust 然后编译安装 wrangler，但是问题还是没解决😢。之后去 GitHub issue 上看有人说按下面的方法修改一下配置文件解决了他的问题，

```yaml
#  config file: ~/.cargo/config.toml
[net]
git-fetch-with-cli = true 
```

但是这对我依然无效。后来我忽然注意到我是不是搞错重点了，报错的重点是不是 `Could not write data: Permission denied`，这应该个权限问题。**然后我尝试在 home 目录下执行命令，而不是在/mnt/d 这种挂载的 Windows 磁盘下执行命令果然项目就创建成功了😁。** 感觉这应该WSL2下面文件权限弄的还是有点问题，毕竟 NTFS 和Linux下的文件系统还是有点区别的，也难怪官方建议在对文件系统性能要求比较高的时候不要用挂载的磁盘。

