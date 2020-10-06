---

layout: post

title: "纵横字谜的答案(Crossword Answers UVa232)"

keywords: 纵横字谜的答案，Crossword Answers UVa232

description: 纵横字谜的答案，Crossword Answers UVa232题解

date: 2017-3-11 20:28

author: "尹傲雄"

categories: [算法]

---
　　做到这个题目的时候发现和前面的那个题目非常的像，都是字符都是表格。依然是用一个二维字符数组，储存表格的状况，然后进行搜索判断。
　　先写一下我犯的错误吧，读者看到我犯过的错误之后说不定就找到自己的方法了呢。毕竟题目还是要自己想出来的比较好。
　　
 1. 错误的认为字符字串前的数字是第几个字母
 2. 错误的使用对行处理的方法处理列，字串乱序输出
 　　遇到的问题：
 1. 找不到字符子串前的数字的规律。(解决方法：在书上发现这个数字是字符字串的起始字母对应的起始格的编号。）
 2. 竖向选择的字符字串输出乱序（解决方法：使用结构体数组储存结果，然后根据起始数组对数组进行排序，最后输出）
 　　好了大概就这么多了，贴一下代码， 前面的题目的一些代码在gihub的仓库里[https://github.com/YinAoXiong/Algorithmic-exercises](https://github.com/YinAoXiong/Algorithmic-exercises)，（全部都是用vs2015写的工程文件，源文件在同名的子目录目录中）
 　　

```
#include<iostream>
#include<string>
#include<cctype>
#include<fstream>
using namespace std;
int is_the_begin(const char a[][11],int i,int j)                  //判断是否为起始字符
{
	if ((i - 1) < 0 || a[i - 1][j] == '*' || (j - 1) < 0 || a[i][j - 1] == '*')
		return 1;
	else
		return 0;
}

int main()
{    /*文件重定向，输出测试*/
	ifstream fin;
	fin.open("data.in");
	cin.rdbuf(fin.rdbuf());
	ofstream out;
	out.open("data.out");
	cout.rdbuf(out.rdbuf());
	int r = 0, c = 0,key=1,t=0;       //r,c储存行和列,key标记次数，
	while (cin>>r>>c&&r!=0)
	{
		if (++t > 1)                                 //用来控制输出中的空行，首行前和最后一行后无空行，输出用空行隔开
			cout << endl;
		cin.get();                                     //清除回车
		char a[11][11] = { 0 };    //储存字符
		for (int i = 0; i < r; ++i)    //输入字符
		{
			for (int j = 0; j < c; ++j)
			{
				cin.get(a[i][j]);
			}
			cin.get();
		}
		cout << "puzzle #" << key << ':' << endl;
		++key;
		cout << "Across" << endl;
		int begin = 0;                            //储存当前是几个起始格
		for (int i = 0; i < r; ++i)               //输出行字串
		{
			for (int j = 0; j < c; ++j)
			{
				if (isupper(a[i][j]))                                 //判断是不是大写字母
				{

					if (is_the_begin(a, i, j))
					{
						if (begin+1<10)
							cout << "  " << begin+1 << '.';
						else
							cout << ' ' << begin+1 << '.';
						for (j; j < c; ++j)
						{
							if (a[i][j] != '*')
							{
								if (is_the_begin(a, i, j))
									++begin;
								cout << a[i][j];
							}

							else
								break;
						}
						cout << endl;
					}
				}
			}
		}
		int x[11][11] = { 0 } ;                                          //用来储存字母对应是第几个起始符
		begin = 0;
		for (int i = 0; i < r; ++i)                
		{
			for (int j = 0; j < c; ++j)
			{
				if (isupper(a[i][j]))
				{
					if (is_the_begin(a, i, j))
					{
						begin += 1;
						x[i][j] = begin;
					}
				}
			}
		}
		struct answer                                       //定义一个结构体用来储存结果
		{
			int id;
			string z;
		};
		answer b[100];
		int m = 0;                                       //用来表示用了多少个结构体
		for (int j = 0; j < c; ++j)                       //将结果付给结构体
		{
			for (int i = 0; i < r; ++i)
			{

				if (isupper(a[i][j]))
				{
					if (is_the_begin(a, i, j))
					{
						b[m].id = x[i][j];                   //将是第几个起始格付给结构体
						//int q = i;
						for (i; i < r; ++i)
						{
							if (a[i][j] != '*')
							{
								b[m].z += a[i][j];              //将字符串赋值
							}

							else
								break;
						}
						++m;                                         //m加一进入下一个结构体
					}
				}
			}
		}
		for(int i=0;i<m-1;++i)                                  //用冒泡排序法对结构体根据id进行排序
			for (int j = 0; j < m - i - 1; ++j)
			{
				if (b[j].id > b[j + 1].id)
				{
					answer temp;
					temp = b[j];
					b[j] = b[j + 1];
					b[j+1] = temp;
				}
			}
		cout << "Down" << endl;
		for (int i = 0; i < m; ++i)                                 //输出结果
		{
			if(b[i].id<10)
				cout << "  " << b[i].id << '.' << b[i].z << endl;
			else
				cout << ' ' << b[i].id << '.' << b[i].z << endl;

		}


	}
}
```
