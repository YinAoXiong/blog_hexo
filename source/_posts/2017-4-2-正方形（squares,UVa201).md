---

layout: post

title: "正方形（squares,UVa201)"

keywords: squares,UVa201

description: squares,UVa201题解

date: 2017-4-6 11:45

author: "尹傲雄"

categories: [算法]

---
　　这个题目思路还是比较简单的，两个数组储存行和列的情况。然后再遍历各个顶点的情况就可以了。其他的见源码中的注释。

```
#include<iostream>
#include<fstream>
using namespace std;
struct zuobao              //创建结构体用来储存横边和竖边的坐标
{
	int row;
	int column;
};
bool ifexist(int i, int j, int size, const zuobao* rc)
{
	for (int m = 0; m < size; ++m)
	{
		if (rc[m].row == i && rc[m].column == j)
			return true;
	}
	return false;
}
void panduan(int i, int j, int square_size, int lenth, int *number, const zuobao *r, const zuobao*c)    // i为横坐标，j为纵坐标，该函数用来计算以该点为右上角的起点，可以构成什么正方形
{

	for (int m = 1; m < square_size; ++m)           //循环判断规格为1--n-1的正方形
	{
		int key = 1;                                    //用来标记边是否存在
		for (int n = 0; n < m; ++n)                          //左边竖向的边
		{
			if (!ifexist(i + n, j, lenth, c))
			{
				key = 0;
				break;                             //缺少边跳出循环
			}
		}
		if (key == 0)                             //如果缺少跳过下面的判断
			continue;
		for (int n = 0; n < m; ++n)
		{
			if (!ifexist(i + m, j + n, lenth, r))
			{
				key = 0;
				break;
			}
		}
		if (key == 0)
			continue;
		for (int n = 0; n < m; ++n)
		{
			if (!ifexist(i + n, j + m, lenth, c))
			{
				key = 0;
				break;
			}
		}
		if (key == 0)
			continue;
		for (int n = 0; n < m; ++n)
		{
			if (!ifexist(i, j + n, lenth, r))
			{
				key = 0;
				break;
			}
		}
		if (key == 0)
			continue;
		++number[m];
	}
}
int main()
{
	///*文件重定向，输出测试*/
	//ifstream fin;
	//fin.open("data.in");
	//cin.rdbuf(fin.rdbuf());
	//ofstream out;
	//out.open("data.out");
	//cout.rdbuf(out.rdbuf());
	int n = 0, m = 0, times = 1;       //n为正方体边上点的个数，m为边的个数
	int t = 0;
	while (cin >> n >> m)
	{
		if (0 != t)
			cout <<endl<< "**********************************" << endl << endl;
		t = 1;
		char kind;
		zuobao * r = new zuobao[m + 2];  //r为横边，c为竖边
		zuobao * c = new zuobao[m + 2];
		int number[12] = { 0 };                //统计不同规格的正方形的个数
		for (int i = 0; i < m; ++i)
		{
			cin >> kind;
			if(kind=='H')
				cin >> r[i].row >> r[i].column;
			else
				cin >> c[i].column >> c[i].row;
		}                                             //注意这里题目里的i变成了列，而j变成了行，注意调换顺序
		cout << "Problem #" << times << endl << endl;
		++times;
		for (int i = 1; i < n; ++i)
			for (int j = 1; j < n; ++j)
			{
				panduan(i, j, n, m, number, r, c);
			}
		int temp = 0;
		for (int i = 1; i < n; ++i)
		{
			if (number[i] != 0)
			{
				cout << number[i] << " square (s) of size " << i << endl;
				temp = 1;
			}
		}
		if (0 == temp)
			cout << "No completed squares can be found." << endl;


	}
}
```
前面的题目的一些代码在gihub的仓库里[https://github.com/YinAoXiong/Algorithmic-exercises](https://github.com/YinAoXiong/Algorithmic-exercises)，（全部都是用vs写的工程文件，源文件在同名的子目录目录中）.
