---
layout: archive
title: 算法 递归
---

递归算法的构成：基本项和归纳项

例题一 求n的阶乘n!(n>0)
终止状态：当n=1时，n!=1
归纳项：当n>1时，n!=n*(n-1)!
[cpp] view plain copy


	1	int fact(int n)  
	2	{  
	3	  if(n==1) return 1;  
	4	  else return n*fact(n-1);  
	5	}  
例题二 Fibonacci序列：1,1,2,3,5,8,13…
定义为：F0=F1=1   Fi=Fi-1+Fi-2(i>1)

终止状态：当i=1或i=0时，Fi=1
归纳项：当i>1时，Fi=Fi-1+Fi-2

[cpp] view plain copy


	1	int Fib(int i)  
	2	{  
	3	   if(i==1||i==0) return 1;  
	4	   else return Fib(i-1)+Fib(i-2);  
	5	}  

 1.求单链表的表长——递归算法
方法一

[cpp] view plain copy


	1	int length(struct node *h)  
	2	{  
	3	   if(h==0) return 0;  
	4	   else return  1+length(h->next);  
	5	}  
方法二


[cpp] view plain copy


	1	int counter=0;  
	2	void length(struct node*h)  
	3	{  
	4	   if(h==0) return;  
	5	   else  
	6	   {  
	7	     counter++;length(h->next);  
	8	   }  
	9	}  
方法三


[cpp] view plain copy


	1	int counter=0;  
	2	void length(struct node*h)  
	3	{  
	4	   if(h==0) return;  
	5	   else  
	6	   {  
	7	     length(h->next);counter++;  
	8	   }  
	9	}  
方法四


[cpp] view plain copy


	1	void length(struct node*h,int &x)  
	2	{  
	3	   if(h==0) return;  
	4	   else  
	5	   {  
	6	     x++;length(h->next,x);  
	7	   }  
	8	}  

2.输出单链表各结点的元素值
方法一

[cpp] view plain copy


	1	void print{struct node*head)//正向输出  
	2	{  
	3	  if(head==0) return;  
	4	  else   
	5	  {  
	6	    cout<<head->data<<" ";  
	7	   print(head->next);  
	8	  }  
	9	}  


方法二
[cpp] view plain copy


	1	void print{struct node*head)//反向输出  
	2	{  
	3	  if(head==0) return;  
	4	  else   
	5	  {  
	6	    print(head->next);  
	7	    cout<<head->data<<" ";  
	8	  }  
	9	}  

问题：出栈序列
算法设计思路 穷举法
思想：就是用穷举的方法列出所有的情况
算法：用递归算法进行所有情况的搜索

[cpp] view plain copy


	1	void counter1(int sout,int sin,int &sum)  
	2	//sin表示在栈中的元素个数  
	3	//sout表示未进栈的元素个数，sum为统计数目  
	4	{  
	5	  if(sout==1) sum++;  
	6	  else counter1(sout-1,sin+1,sum);  
	7	  if(sin>0) counter1(sout,sin-1,sum);  
	8	}  

[cpp] view plain copy


	1	void counter2(int sout,int sin,int &sum)  
	2	//sin表示在栈中的元素个数  
	3	//sout表示未进栈的元素个数，sum为统计数目  
	4	{  
	5	  if(sout==0) {sum++;return;}  
	6	  else counter2(sout-1,sin+1,sum);  
	7	  if(sin>0) counter2(sout,sin-1,sum);  
	8	}  

问题:
张明与朋友们一同去上海世博会参观，世博会的门票为50元，张明发现一个奇怪的现象：在排队购票的2n个人中，总有n个人拿着面值为100元的钞票，而另外n个人拿的是面值为50元的钞票。张明想知道在这种情况下这2n个人共有多少中排队方式，使售票处不至于出现找不开钱的局面（假设售票处原来没有零钱的情况）？
算法设计思路
思路：根据影响因素进行函数关系的构建
由于题目中影响序列个数的因素主要是拿50元和100元的当前人数，因此，我们可以设定：
x为拿50元的当前人数，y为拿100元的当前人数。
于是有我们就可以构建出函数f(x,y)。
分情况讨论函数关系
y=0，则只有拿50元的人，于是f(x,0)=1; 
X<y，因为拿100元的人多于50元的人，最后会出现无法找零，于是f(x,y)=0; 
其余情况：
–考虑x+y个人排队的情况，第x+y个人站在第x+y-1个人的后面，那么第x+y个人的排队方案可以按以下两种情况来分析：
（1）第x+y个人拿100元，那么他之前的x+y-1个人中有x个人50
元，有y-1个人拿100元，此时为f(x,y-1); 
（2）第x+y个人拿50元，那么他之前的x+y-1个人中有x-1个人拿50元，有y个人拿100元，此时为f(x-1,y);
[cpp] view plain copy


	1	int f(int x, int y)   
	2	{   
	3	     if (y==0) return 1;   
	4	     else if (x<y) return 0;   
	5	     else return f(x-1,y)+f(x,y-1);   
	6	}  

设有n条封闭曲线画在平面上，而任何两条封闭曲线恰好相交于两点，且任何三条封闭曲线不相交于同一点，问这些封闭曲线把平面分割成的区域个数。设fn为n条封闭曲线把平面分割成的区域个数，则有：
f2-f1=2   
f3-f2=4 
f4-f3=6 
……
fn-fn-1=2*(n-1)
[cpp] view plain copy


	1	int f(int n)   
	2	{   
	3	     if (n==1) return 1;   
	4	     else if (n>1) return f(n-1)+2*(n-1);   
	5	}  


问题描述:
将整数n分成k份,且每份不能为空,任意两种分法不能相同(不考虑顺序),问有多少种不同的分法。(6<n<=200,1<k<7)

说明：n=7，k=3，则下面三种分法被认为是相同的。
 1,1,5    1,5,1   5,1,1
样例：输入:7 3   输出：4   
1 1 5，1 2 4，1 3 3，2 2 3   
算法分析
1递归算法：分析处理过程，找到递推公式，用递归算法来解决问题。
首先分析处理过程，当n=15,k=6时的分法
1 1 1 1 1 10
1 1 1 1 2 9
1 1 1 1 3 8
1 1 1 1 4 7
……
1 2 3 3 3 3
2 2 2 2 2 5
2 2 2 2 3 4
2 2 2 3 3 3
共26种。n=12,k=3时
1  1  10          2  2  8         3  3  6       4  4  4
1  2  9             2  3  7         3  4  5
1  3  8             2  4  6          
1  4  7             2  5  5
1  5  6
思路
逐步将范围缩小。
每个位置都进行逐个数值的处理分析。
即第一个数的选择方法，接着考虑第二数的选择方法，如此类推。

构造递归函数
于是设函数f(n,k)表示总的分法,它可按第一个分解数的不同,分为i类（0<i<=n /k）：
i=1时，因为第一个数已经固定为1，那么剩下就为n-1，要划分为k-1分，因此，f1=f(n-1,k-1)
i=2时，因为第一个数固定为2，因此，后面的每个数均不少于2,因此后面的分解就从2开始，再把剩余的数n-2划分为k-1份。因此，f2=f(n-2,k-1)从2开始处理
i=3时，因为第一个数固定为3，因此，后面的每个数均不少于3，所以后面的分解就从3开始，再把剩余的数n-3划分为k-1份。因此，f3=f(n-3,k-1)从3开始处理

我们考虑一般情况，即第一个数为i时，因为，第一数已经固定为i，即其后面的每个数均不少于i，所以后面数的分解就要从i开始处理，再把剩余的数n-i划为k-1份。因此，fi=f(n-i,k-1)从i开始处理因此，我们得出如下的递归函数：F(n,k)=f1+f2+…+f[n/k],又因为，每次均要从上一次所选数开始进行处理，因此我们将函数改写为
[cpp] view plain copy

	1	F(n,k,now) =F(n-1,k-1,1) + F(n-2,k-1,2) + F(n-n/k,k-1,n/k)    
[cpp] view plain copy

	1	int f(int n,int k,int now)     
	2	{      
	3	   int i,s=0;     
	4	   if (k==1)  return 1;     
	5	   else      
	6	   {for(i=now;i<=n/k;i++)     
	7	    s=s+ f(n-i,k-1,i);//用同样的分法处理规模降1的剩余数    
	8	    return s;      
	9	   }     
	10	}     
	11	void main()     
	12	{   int n=15,k=6;     
	13	    cout<<f(n,k,1);     
	14	}     
算法分析2
递归算法：分析处理过程，找到递推公式，用递归算法来解决问题。首先分析处理过程，当n=15,k=6时的分法
1 1 1 1 1 10
1 1 1 1 2 9
1 1 1 1 3 8
1 1 1 1 4 7
……
1 2 3 3 3 3
2 2 2 2 2 5
2 2 2 2 3 4
2 2 2 3 3 3
共26种。

构造递归函数
于是设函数f(n,k)表示总的分法,它可按第一个分解数的不同,分为i类（0<i<=n /k）：
i=1时，因为第一个数已经固定为1，那么剩下就为n-1，要划分为k-1分，因此，f1=f(n-1,k-1)
i=2时，因为第一个数固定为2，因此，后面的每个数均不少于2，所以就可以先在后面的k-1份中的每一份里放1，再把剩余的数n-2-(k-1)划分为k-1份。因此，f2=f((n-2)-(k-1),k-1)=f(n-k-1,k-1)
i=3时，因为第一个数固定为3，因此，后面的每个数均不少于3，所以就可以先在后面的k-1份中的每一份里放2，再把剩余的数n-2-(k-1)*2分为k-1份。因此，f3=f((n-3)-(k-1)*2,k-1)=f(n-2k-1,k-1)
我们考虑一般情况，即第一个数为i时，因为，第一数已经固定为i，即其后面的每个数均不少于i，所以就可以先在后面的k-1份中的每一份里放i-1，再把剩余的数n-i-(k-1)*(i-1)划分为k-1份。因此，

[cpp] view plain copy

	1	fi=f((n-i)-(k-1)*(i-1),k-1) = f(n-(i-1)*k-1,k-1  
因此，我们得出如下的递归函数：

F(n,k)=f1+f2+…+f[n/k]// 特别地f(n,1)=1,f(n,2)=n/2
[cpp] view plain copy

	1	F(n,k)=f1+f2+…+f[n/k]// 特别地f(n,1)=1,f(n,2)=n/2  
[cpp] view plain copy

	1	int f(int n,int k)     
	2	{   int i, s=0;     
	3	    if (k==2) return n/2; //if (k==1) return 1;     
	4	    else {  s=0;     
	5	                for(i=1;i<=n/k;i++)     
	6	                     s=s+ f(n-(i-1)*k-1,k-1);     
	7	                return s;     
	8	            }     
	9	}     
	10	void main()     
	11	{    cout<<f(15,6);    }     
[cpp] view plain copy

	1	#include <iostream>    
	2	using namespace std;    
	3	int a[60], s = 0;    
	4	void f(int n, int now, int k, int loop)    
	5	{    
	6	    int i, j;    
	7	    if (k == 1)    
	8	    {    
	9	        a[now] = (loop - 1)*k + n; s++;    
	10	        for (j = 1; j <= now; j++) cout << a[j];    
	11	        cout << endl;    
	12	    }    
	13	    else    
	14	    {    
	15	        for (i = 1; i < n / k; i++)    
	16	        {    
	17	            a[now] = a[now - 1];    
	18	            f(n - (i - 1)*k - 1, now + 1, k - 1, i);    
	19	        }    
	20	    }    
	21	}    
	22	    
	23	int main()    
	24	{    
	25	    a[0] = 0; f(7, 1, 3, 0); cout << s;    
	26	    return 0;    
	27	}    


如何输出每种分法的结果
如何保存每种分法的各个取值。
fi=f(n-i,k-1) 从i开始处理，将这个i值保存到数组中

问题升级
任何一个大于1的自然数n，总可以分拆成若干个小于n的自然数之和。
例如：
N=2可以分拆为1种：2=1+1 
N=3可以分拆为2种： 3=1+2   3=1+1+1 
N=4可以分拆为4种：4=1+3   4=1+1+2   4=1+1+1+1  4=2+2 
注意：分拆N=5时，5=2+3    5=3+2 是同一种拆分。
思路分析
拆分方法（以N=7为例）
第一步：将N拆分为2个数
7=1+6 7=2+5 7=3+4       
拆分特点：第2加数总大于第一个加数，因此第一个加数从1变化到n/2。
第二步：只要第2个加数仍然大于1，就可按第一步重复拆分。
7=1+6    可以继续拆分为 7=1+1+5 7=1+2+4 7=1+3+3 
其中7=1+1+5又可以继续拆分获得7=1+1+1+4 7=1+1+2+3…

[cpp] view plain copy

	1	void  f(int n,int now)     
	2	{   int i , j;     
	3	if (n<now) return     
	4	else      
	5	{ for(i=now;i<=n/2;i++)     
	6	  { s++;//全局变量，统计分法的总数    
	7	    f(n-i,i);       
	8	   }     
	9	}     
	10	}    


