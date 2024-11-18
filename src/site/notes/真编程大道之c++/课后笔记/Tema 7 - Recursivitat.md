---
{"dg-publish":true,"tags":["Programació","UPC"],"permalink":"/真编程大道之c++/课后笔记/Tema 7 - Recursivitat/","dgPassFrontmatter":true,"created":"2024-11-12T11:55:33.466+01:00","updated":"2024-11-18T14:40:49.718+01:00"}
---



当一个函数需要调用它自己时，它被称为 “递归”。

下文来源于视频 https://www.bilibili.com/video/BV1Bu4y1Y7ow?spm_id_from=333.788.videopod.sections&vd_source=3fbdb8991e5e64123e4ff53eceff4a72
## 7.1 递归 - 现象解释

假设存在一个数列： `1 4 7 10 13 ···` 请找出该数列中的第六个数。

- 通过观察，我们发现这是一个等差数列。
- 我们可以推理出它的**公式**：A(n) = A(n-1)+3。也就是右边 = 左边一项+3的和。

 所以：
- 想要知道数列中的第六个数，我们必须知道第五个数；
-  想要知道第五个数，我们必须知道第四个数；
- ...
- 第一个数为 `1`。这是递归的**起始项**。

```c++
// Pre: n >= 1
// Post: 第n位数列
int f (n) {
	if (n == 1) return 1;              // 起始项：避免无限递归
	else return ( f(n-1) + 3 );            // 递归公式
}

int main {
	cin >> n;
	cout >> f(n) >> endl;
}
```

假如 n = 3，会发生以下情况：
#####  <b style="color: #5DD0C8;">阶段1：递归，直到抵达起始项</b>
↓ 被主函数调用
```c++
int f (3) {
	if (3 == 1) return 1;    // 错误，前往 else
	else return ( f(3-1) + 3 );      // 调用 f(2)
}
```
↓ 暂停该 instance，==递归调用/Recursive call==，==创造/create== ==函数实例/instance==
```c++
int f (2) {
	if (2 == 1) return 1;    // 错误，前往 else
	else return ( f(2-1) + 3 );      // 调用 f(1)
}
```
↓ 暂停该 instance，递归调用，创造
```c++
int f (1) {
	if (1 == 1) return 1;    // 正确，开始返回
	else ..
}
```

#####  <b style="color: #5DD0C8;">阶段2：返回，直到回到递归的开端</b>

```c++
int f (1) {
	if (1 == 1) return 1;    // 正确，返回起始项的值（1）
	else ..
}
```
↓ 返回，==销毁/destroy== 函数实例
```c++
int f (2) {
	if ..
	else return  ( f(2-1) + 3 );      // 返回 f(2)，也就是f(1)+3 的值：4
}
```
↓ 返回
```c++
int f (3) {
	if ..
	else return  ( f(3-1) + 3 );      // 返回 f(3)，也就是f(2)+3 的值：7
}
```
↓ 返回至主函数


#####  <b style="color: #5DD0C8;">现象总结</b>
我们将这种现象描述为：
- 每次调用函数时，都会**创建/create**一个新的**函数实例/instance**。每次函数 “返回 ”时，都会**销毁/destroy** 实例。

代码的执行顺序如下：
```c++
int f (3) {
	if (3 == 1) return 1;
	else return ( f(3-1)..               // 暂停该inctance，创建新instance
		
		int f (2) {
			if (2 == 1) return 1;
			else return ( f(2-1)..       // 暂停该instance，创建新instance
			
			int f (1) {
				if (1 == 1) return 1;       // 返回
			
			.. + 3);                        // 返回并加三
		
	.. + 3 );                               // 返回并加三
}
```
- 图例：
	![真编程大道之c++/课后笔记/皂片/7 - 7.1.01.png](/img/user/%E7%9C%9F%E7%BC%96%E7%A8%8B%E5%A4%A7%E9%81%93%E4%B9%8Bc++/%E8%AF%BE%E5%90%8E%E7%AC%94%E8%AE%B0/%E7%9A%82%E7%89%87/7%20-%207.1.01.png)


## 7.2 弊端与优化
假如我们不再计算等差数列，而是斐波那契数列。
![真编程大道之c++/课后笔记/皂片/7 - 7.2.01.png](/img/user/%E7%9C%9F%E7%BC%96%E7%A8%8B%E5%A4%A7%E9%81%93%E4%B9%8Bc++/%E8%AF%BE%E5%90%8E%E7%AC%94%E8%AE%B0/%E7%9A%82%E7%89%87/7%20-%207.2.01.png)

我们算出公式：
- 除了 序列0 与 序列1 之外，序列n = 序列n-2 + 序列n-1
- A(n) = A(n-2) + A(n-1)

如果我们按照之前的语法来写代码的话：
```c++
int fib (n) {
	if (n<=0) return 1;
	else return ( fib(n-1) + fib(n-2) );
}
```

![7 - 7.2.02.png](/img/user/%E7%9C%9F%E7%BC%96%E7%A8%8B%E5%A4%A7%E9%81%93%E4%B9%8Bc++/%E8%AF%BE%E5%90%8E%E7%AC%94%E8%AE%B0/%E7%9A%82%E7%89%87/7%20-%207.2.02.png)
此时，为了计算 fib(5)，程序需要调用 fib(4) 与 fib(3)；fib(4) 需要调用 fib(3) 与 fib(2)；fib(3) 需要调用 fib (1) 与 fib(2)，还需要被调用两次；更别提 fib(2) fib(1) fib(0)。**这个程序将需要成吨的计算量**：
- fib(5) is called once 
- fib(4) is called once
- fib(3) is called twice
- fib(2) is called 3 times
- fib(1) is called 5 times
- fib(0) is called 3 times

所以，我们**最好不要用递归，而是采用类似滑动窗口策略的方法**，用于储存 fib(n-1) 与 fib(n-2)：
```c++
int fib (n) {
// Pre: n >= 0 
// Post: returns the Fibonacci number of order n

	int fi = 1;                    
	int fprevi = 1;
	// Inv: fi 是第 i 位的值
	//      fprevi 是 i-1 位的值
	for (int i = 1; i<n; ++i) {
		int f = fi + fprevi;
		fprevi = fi;
		fi = f;
	}
	return fi;
}
```

这样，fib(5) 就只需要六次运算了（012345 各一次）。


## 7.3 递归 - 思路分析
来自于视频：https://www.bilibili.com/video/BV1C14y1V77j/?spm_id_from=333.337.search-card.all.click&vd_source=3fbdb8991e5e64123e4ff53eceff4a72


一般来说，只需要从四个方面分析一个递归：

0. **Pre/post 分析**
1. **基础情况处理 - Basic case**
	“在什么情况下，递归终止”？
2. **递归调用**
	用于将数据规模减小，目的是达到 “基础情况”
3. **返回与处理**
	在抵达基础情况并返回时，对变量的处理。很多情况下就是**表达规律的公式**。


比如：
- 在第一个数列问题中：
	1. 基础情况：当 n=0，return 1；
	2. 递归调用：将 n-1，直到达到 “基础情况 n=0”；
	3. 返回与处理：将返回值 +3（ A(n) = A(n-1) +3 )。

- 在 “排列数组顺序”问题中（来自于视频）：
	0. 给出数列、起始位、终止位，将其排序（白色）。
	1. 基础情况：当起始位-终止位为 2 时，终止并返回（绿色）；
	2. 递归调用：新定义一个变量 m，调用两次自己，将当前数组截成两部分（橙色）；
	3. 返回与处理：调用函数 Merge ，按照大小排列两个 MergeSort 数组（蓝色）。
	- 图例
		![真编程大道之c++/课后笔记/皂片/7 - 7.3.01.png](/img/user/%E7%9C%9F%E7%BC%96%E7%A8%8B%E5%A4%A7%E9%81%93%E4%B9%8Bc++/%E8%AF%BE%E5%90%8E%E7%AC%94%E8%AE%B0/%E7%9A%82%E7%89%87/7%20-%207.3.01.png)


#####  <b style="color: #5DD0C8;">来 顺顺思路</b>
递推的关键是 “**基础情况处理**” 与 “**返回至当前层（微操作）**”。
看起来最大一块的 “**递归调用（超级操作）**” 反倒不需要太多注意。我们越把精神放在脑内对它的推演只会让我们越乱。因为，**递归调用 实际上就是对 基础情况处理 与 返回至当前层 的重复实现**。

就好比多米诺骨牌，只要我们设计好骨牌 AB 间的距离，并确定推到 A 之后，B 也一定倒。那么，就算我们调用再多次距离，插在 AB 中间，只要推倒 A，B 依然会倒。有点像数学中的数学归纳法。

所以，只要情况处理正确，返回处理正确，只要处理好递推逻辑，那么就一定正确。

![真编程大道之c++/课后笔记/皂片/7 - 7.3.02.png](/img/user/%E7%9C%9F%E7%BC%96%E7%A8%8B%E5%A4%A7%E9%81%93%E4%B9%8Bc++/%E8%AF%BE%E5%90%8E%E7%AC%94%E8%AE%B0/%E7%9A%82%E7%89%87/7%20-%207.3.02.png)


将问题映射到汉诺塔，我们可以得出下面结论：
0. 给定 圆盘数n、源头柱子source、目标柱子target、辅助柱子auxiliar；返回移动完毕的汉诺塔。
1. 基础情况：当只需要挪动一个圆盘时，相应的移动它，并且返回：
	数据情况最小时，递归终止，开始返回；
2. 超级操作：除最大圆盘以外的，n-1 个圆盘移动至某根柱子上：
	此时圆盘数变成 n-1，更加靠近 1；
3. 微操作：将第 n 个圆盘移动至 柱子target 。

需要执行的动作如下：
1. 超级操作：将所有 n-1 个圆盘移动至 柱子auxiliary 上；
2. 微操作：将大圆盘移动至 柱子target 上；
3. 超级操作：将所有 n-1 个圆盘移动至 柱子target 上。


![Pasted image 20241112180208.png](/img/user/%E7%9C%9F%E7%BC%96%E7%A8%8B%E5%A4%A7%E9%81%93%E4%B9%8Bc++/%E8%AF%BE%E5%90%8E%E7%AC%94%E8%AE%B0/%E7%9A%82%E7%89%87/Pasted%20image%2020241112180208.png)

































