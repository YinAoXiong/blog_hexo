---
layout: post

title: "vscode language server cannot resolve import with editable mode package 解决"

keywords: vscode language server, import resolution, editable mode package, troubleshooting, solution

description: "vscode language server cannot resolve import with editable mode package解决"

date: 2023-8-16 17:56

author: "尹傲雄"

categories: [工程向]

tags: [vscode,python]

---

#  起因

最近在用vscode写代码的时候用到了fairseq，它是通过'pip install -e .'的方式来安装的也就是editable mode。结果在vscode中编辑的时候的language server一直报错，提示找不到这个包，ctrl+左键也无法跳转这样就很难受了。但是我在命令行下面import fairseq是ok的，代码运行也没啥问题。（之前解决过一次没有记录，现在忘记了，特此记录一下）

#  解决过程

后来搜索到了 [language server cannot resolve import with editable mode package · Issue #4954 · microsoft/vscode-python (github.com)](https://github.com/Microsoft/vscode-python/issues/4954)，这个GitHub issue,找到了提示方法，就是在vscode的setting.json中添加如下配置：

```json
{
    "python.autoComplete.extraPaths": [
        "[path to your fairseq source code]"
    ],
    "python.analysis.extraPaths": [
         "[path to your fairseq source code]"
    ]
}

```

这样手动添加两个额外的搜索路径就可以了。

