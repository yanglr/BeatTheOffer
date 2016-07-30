# 剑指Offer2014 - 题解 {#offer2014}

**面试题：数的整数次方**

**温馨提示：**本技术博客的相关代码将会在github([https://github.com/yanglr](https://github.com/yanglr))中同步更新，敬请star和fork...

**题目：**实现函数double Power(double base, int exponent), 求base的exponent次方。不得使用库函数，同时**不需要考虑大数问题**。

其中base为浮点数，而exponent为整数(可正可负，可为0).

提交网址： [http://www.nowcoder.com/practice/1a834e5e3e1a4b7ba251417554e07c00?tpId=13&amp;tqId=11165](http://www.nowcoder.com/practice/1a834e5e3e1a4b7ba251417554e07c00?tpId=13&tqId=11165)

**分析：**

二分求幂(时间复杂度为log n)，使用二分法则问题可转化为：

![](http://img.blog.csdn.net/20160507003255004)

此公式并不能很好的反映情况，且用n=2k+1表示**奇数**时，**(**n<sub>奇</sub>**-1)/2=k=**⌊n<sub>奇</sub>/2⌋ (k∈Z)，故以上公式可改进成如下公式：

![](http://img.blog.csdn.net/20160514130818081)

其中 ![](http://img.blog.csdn.net/20160514132111631)，不论n是奇是偶，对其折半后下取整即可得到k，而在编程语言中如果变量类型为整型，k=n/2，如果用弱数据类型的语言(比如：PHP、[Python](http://lib.csdn.net/base/11)、JavaScript等)实现此方法，需自行ceiling或ceil进行下取整！

当选取某种强数据类型的编程语言，且上述公式中的变量n类型为整型时，可继续简化： ![](http://img.blog.csdn.net/20160507074543378?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这样，比较容易理解，也有利于写出清晰结构的代码...

非递归实现 AC代码：

<pre class="cpp">#include&lt;cstdio&gt;
using namespace std;
class Solution {
public:
    double Power(double base, int exponent) {
        if(exponent &lt; 0)  // 将指数exponent&lt;0的情形规约为exponent&gt;0的情形
        {
            exponent = -exponent;
            base = 1.0/base;
        }
        double res = 1.0;
        for(; exponent &gt; 0; exponent &gt;&gt;= 1)  // 指数每次折半，进行迭代
        {
            if((exponent &amp; 0x1) == 1)  res *= base;  // a &amp; 0x1 等价于 a%2，用来判断a的奇偶性
            base *= base;  // 底数base替换为base^2
        }
        return res;
    }
};

// 以下为测试
int main()
{
	Solution sol;
	double num1=sol.Power(20, 2);
	double num2=sol.Power(13, -2);
	double num3=sol.Power(-10, 3);
	double num4=sol.Power(0, 0);	
	printf(&quot;%f\n&quot;,num1); 
	printf(&quot;%f\n&quot;,num2);
	printf(&quot;%f\n&quot;,num3);		
	printf(&quot;%d\n&quot;,num4);		
	return 0;
}</pre>

剑指offer上不需要考虑大数问题，可是Leetcode上需要考虑大数问题、边界值问题...

50\. Pow(x, n)

Total Accepted: 90298 Total Submissions: 323977 Difficulty: Medium    ACrate: 27.9%

Implement pow(x, n).

提交网址： [https://leetcode.com/problems/powx-n/](https://leetcode.com/problems/powx-n/)

刚开始Leetcode上越界条件怎么都过不去，每次都是 **299 / 300** test cases passed....

**输入输出样例：**

Input: 2.00000 -2147483648 Output: 0.00000 ---------------- Input: 1.00000 -2147483648 Output: 1.00000 ---------------- Input: -1.00000 2147483647 Output: -1.00000 ---------------- Input: -1.00000 -2147483648 Output: 1.00000 ---------------- Input: 2.00000 -2147483648 Output: 0.00000 LeetCode AC代码:

<pre class="cpp">#include&lt;cstdio&gt;
#include&lt;climits&gt;     // INT_MAX和INT_MAX在C语言limits.h中定义，而float.h 定义了double、float类型数的最大最小值 
using namespace std;
class Solution {
public:
    double myPow(double x, int n)
    {
        if(x==0 &amp;&amp; n==0) return 1;    
        if((n&lt;=INT_MIN || n&gt;=INT_MAX) &amp;&amp; (x&gt;1 || x&lt;-1)) return 0;
        if(x==1 &amp;&amp; n==INT_MIN) return 1;  // 加的超强补丁   
        if(n&gt;=INT_MAX &amp;&amp; (x==1 || x==-1)) return x;
        if(n&lt;=INT_MIN &amp;&amp; (x==1 || x==-1)) return -x;

        if(n &lt; 0)
        {
            n = -n;
            x = 1.0/x;
        }
        double res = 1.0;
        for(; n &gt; 0; n &gt;&gt;= 1)
        {
            if((n &amp; 0x1) == 1)  res *= x;
            x *= x;  // 底数x替换为x^2
        }
        return res;
    }
};

// 以下为测试
int main()
{
	Solution sol;
	double res1=sol.myPow(0, 0);	
	double res2=sol.myPow(13, -2);
	double res3=sol.myPow(-10, 0);
	double res4=sol.myPow(20, 2);	
	double res5=sol.myPow(2.00000,-2147483648);
	double res6=sol.myPow(1.00000,-2147483648);
	double res7=sol.myPow(-1.00000,2147483647);
	double res8=sol.myPow(-1.00000,-2147483648);
	double res9=sol.myPow(2.00000,-2147483648);

	printf(&quot;%f\n&quot;, res1); 
	printf(&quot;%f\n&quot;, res2);
	printf(&quot;%f\n&quot;, res3);		
	printf(&quot;%f\n&quot;, res4);
	printf(&quot;%f\n&quot;, res5);
	printf(&quot;%f\n&quot;, res6);		
	printf(&quot;%f\n&quot;, res7);
	printf(&quot;%f\n&quot;, res8);
	printf(&quot;%f\n&quot;, res9);				
	return 0;
}</pre>

相关链接：

剑指offer-面试题11：数值的整数次方 [https://www.douban.com/note/355223813/](https://www.douban.com/note/355223813/)