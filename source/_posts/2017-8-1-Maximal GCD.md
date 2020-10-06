---

layout: post

title: "CodeForces - 803C Maximal GCD(贪心)"

keywords: acm，CodeForces - 803C Maximal GCD，贪心算法

description: CodeForces - 803C Maximal GCD的题目解释和题解

date: 2017-8-1 23:44

author: "尹傲雄"

categories: [算法]
---

　　刚开始一直没有看懂这个题目的意思，看了很久才明白。题目的意思就是给你两个数n和k，然后让你干下面这些事情。
1. 构造一个k位严格递增的序列。
2. 序列和为n
3. 同时使得这个序列的最大公约数最大、

　　这题毕竟关键的就是求这个最大公约数q，因为序列和为n，也就是q*(1+2+3+......k-1+k)<=n。显然q也是n的一个因数，通过这样的方法就可以求得最大共约数了，不过要注意如果直接遍历n的话是会超时的，因为有1e+10那么大，通过对n开方可以将循环次数减少到sqrt(n)最大也就是1e+5就不会超时了。如果q*sum_K-1小于n就将所有的都加到最后也该数里面去，这里注意不要类加q*(1+2+.....+k-1)这样会爆的，去掉q累加就可以了，我就犯了这个错误。

具体代码如下：

```c++

#include <iostream>
#include<cmath>
using namespace std;

typedef long long LL;
LL n, k;

void caculate()
{
	if (k>141420)
	{
		cout << "-1" << endl;
		return;
	}
	LL sum_k = (k + 1)*k / 2;
	LL q = 0;
	LL sqr=sqrt(n);
	for (long long i = 1; i <= sqr; i++)
	{
		if (n%i == 0)
		{
			if (i >= sum_k)
			{
				q = n / i; break;
			}
			else if (n / i >= sum_k)
				q = i;
		}
	}
	if (!q)
	{
		cout << "-1" << endl;
		return;
	}

	LL sum = 0;
	for (LL i = 1; i < k; ++i)
	{
		cout << i*q << ' ';
		sum += i;
	}

	cout << q*(n / q - sum) << endl;
	return;

}

int main()
{


	while (cin >> n >> k)
	{
		caculate();

	}
	return 0;
}

```
