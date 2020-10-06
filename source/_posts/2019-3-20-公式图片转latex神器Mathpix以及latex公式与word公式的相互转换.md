---
layout: post

title: "公式图片转latex神器Mathpix以及latex公式与word公式的相互转换"

keywords: latex,word公式，Mathpix

description: 公式图片转latex神器Mathpix以及latex公式与word公式的相互转换

date: 2019-3-20 17:06

author: "尹傲雄"

categories: [杂类]
---

# 公式图片识别为latex

　　平时在写东西的时候时常有进行公式输入的需要，比如说看了一篇论文写点东西记录一下什么的。但是在写东西的时候手动抄着那些复杂的公式让人有一种在搬砖的错觉（我之前写文档抄公式的时候就有这种错觉:joy:）,这样就会很容易打消搬砖的积极性的。幸好之后杨同学告诉了我Mathpix这个神器瞬间将我解放了生产力max :xyx:。

官网地址：[https://mathpix.com/](https://mathpix.com/)  
官方测试PDF：[https://mathpix.com/examples.pdf](https://mathpix.com/examples.pdf)

　　调了其中比较复杂的第十四个公式来测试了一下，效果非常的喜人，非常的准确没有一点错误。公式图片和最后的结果对照如下所示。

![function](https://cdn.yinaoxiong.cn/image/posts/2019-3-20/function.jpg)


$$
\hat{H}=\int d r\left(\frac{\hbar^{2}}{2 M_{B}} \nabla \hat{\Psi}_{m}^{+} \nabla \hat{\Psi}_{m}+\frac{c_{0}}{2} \hat{\Psi}_{m}^{+} \hat{\Psi}_{m_{1}}^{+} \hat{\Psi}_{m_{1}} \hat{\Psi}_{m}+\frac{c_{2}}{2} \hat{\Psi}_{m_{1}}^{+} \hat{\Psi}_{m_{2}}^{+} F_{m_{1} m_{4}} F_{m_{2} m_{3}} \hat{\Psi}_{m_{3}} \hat{\Psi}_{m_{4}}\right)
$$


# latex转word

## 方法一：word原生latex支持

　　得到latex公式之后我就开心的把它复制到了word中去了，因为上次发现在word中开启latex后就可以直接写latex公式了。开启方式如下图所示在插入公式的时候选中latex就可以了。

![openLatexInWord](https://cdn.yinaoxiong.cn/image/posts/2019-3-20/openLatexInWord.png)

　　但是我发现还是有坑啊，简单一点的还好，复杂一点的长一点的有的时候会转换不了，有的时候会转换出错。例如上面那个公式在word里面转换成了这个样子：

![wordFunction](https://cdn.yinaoxiong.cn/image/posts/2019-3-20/wordFunction.png)

## 方法二：latex转MathML后粘贴（推荐）

　　后面在Google搜了一下之后发现可以先将latex公式转换为MathML后粘贴到word（记得选择为只保留文本，不然有样式信息会不成功的），之后word会自动将MathML显示为公式。尝试了一下发现真的可以而且效果非常好，上面的公式在使用MathML粘贴到word中的结果如下所示：

![rightWordFunction](https://cdn.yinaoxiong.cn/image/posts/2019-3-20/rightWordFunction.png)

至于latex公式如何转换为MathML，有挺多的方法的。在markdown神器typora中可以**右键选中公式然后依次选择公式-》复制到MSWord**，然后到word里面直接粘贴就可以了。或者可以到这个网站：[http://johnmacfarlane.net/texmath](http://johnmacfarlane.net/texmath)，它提供在latex转MathML的服务。

# word转latex

　　word公式转latex同样有两种，想必大家都可以猜出来了。和上面相对应的一种是word原生转换，一种是借助MathML进行中转。

## 方法一：word原生latex支持

　　这个方法非常简单，按照上文说的那样选择好latex后，直接对公式进行复制就然后在需要的地方粘贴就可以了。这个方法对与一下简单的公式来说还是很方便的，但是对与一些比较复杂的公式例如这个例子，之间复制得到的结果就有点问题了。这个例子复制得到的latex公式甚至不能被MathJax解析。

## 方法二：复制得到MathML后转latex（推荐）

　　由于方法一面对复杂公式的时候存在问题，我们可以考虑再次使用MathML进行中转。复制word公式默认得到的是它所说的线性格式的纯文本而不是MathML，我们需要进行一些设置上面的改变。具体步骤如下所示：

先点击箭头指向的小箭头打开公式选项：

![openFunctionSet](https://cdn.yinaoxiong.cn/image/posts/2019-3-20/openFunctionSet.png)

然后将复制公式时选项切换为MathML：

![changeFunctionSet](https://cdn.yinaoxiong.cn/image/posts/2019-3-20/changeFunctionSet.png)

在这之后之间复制公式就是得到MathML的纯文本了，然后再到这个网站：[http://johnmacfarlane.net/texmath](http://johnmacfarlane.net/texmath)，它可以将MathML转换为latex。

　　最后如果是大量的文件转换还是使用文件转换神器 [pandoc](https://pandoc.org/)，然后再修改一下吧，手动格式转换还是算了:joy:。

> 参考资料：

1. [如何将LaTeX公式拷贝到Word中](https://blog.csdn.net/bendanban/article/details/52823171)