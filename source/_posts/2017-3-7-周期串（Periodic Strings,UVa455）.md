---

layout: post

title: "周期串（Periodic Strings,UVa455）"

keywords: 周期串，Periodic Strings,UVa455

description: 周期串（Periodic Strings,UVa455）题解

date: 2017-3-7 23:50

author: "尹傲雄"

categories: [算法]

---
　　最近在学习开始学习算法，现在看的是刘汝佳的《算法竞赛入门》，感觉还是有必要把一些有问题题目记录下来。另外其他的代码都同步到了gitbub上的仓库了，可能不是最好的，不过都被在uva上ac了。欢迎感兴趣的朋友一起交流，小白一只。贴一下网址：[https://github.com/YinAoXiong/Algorithmic-exercises](https://github.com/YinAoXiong/Algorithmic-exercises)，（全部都是用vs2015写的工程文件，源文件在dbug目录中），还在不断更新中。
　　这道题其实是比较简单的一道题，但是对于题目的理解上可能会出现一些问题，比如我就一直认为abcd这种字符的最小周期是0，而不是4.另外最后的输出格式也可能会出现一些问题，如果注意一些最后一次的输出的话很有可能会多打一个换行。好了不多说了，贴一下代码。
　　

```
#include<iostream>
#include<string>
using namespace std;
int issame(string &s, int a, int b)                             //比较是否相同的函数
{
	for (int i = a; i < b; i += a)
	{
		if (s.substr(0, a) != s.substr(i, a))
			return 0;
	}
	return 1;
}
int main()
{
	//freopen("data.in", "r", stdin);
	//freopen("data.out", "w", stdout);
	int t = 0;
	cin >> t;
	while (t--)
	{
		string a;
		cin >> a;
		int n = a.size();
		int i = 1,k=0;
		for (i; i <= n; ++i)
		{
			if (n%i == 0)
			{
				if (issame(a, i, n))            //如果相同就使用当前的i
				{
					k = i;
					break;
				}
			}
		}
		if (t == 0)
			cout << k << endl;                                 //输出ik，要注意题目要求的输出格式问题
		else
			cout << k << endl << endl;
	}

}
```
