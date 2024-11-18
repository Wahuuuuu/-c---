---
{"tags":["Programació","UPC"],"dg-publish":true,"permalink":"/真编程大道之c++/课后笔记/Tema 4 - Sequencias - 序列/","dgPassFrontmatter":true,"created":"2024-10-18T10:30:23.422+02:00","updated":"2024-11-18T14:39:42.000+01:00"}
---


**写出一个程序，用于在一串变量中找出最大的那个。**

- 方法1：

```c++
int m, elem;                   // elem - 输入的数列表；m - 最大的数
cin >> m;                      // 先读入第一个数。因为一共只导入了一个，它就是最大的数。

while (cin >> elem) {          // 之后读入的每个数都会被记作 elem
	if (elem > m) m=elem;      // 每读入一次做一次检测，如果 elem>m，m会被现在的值顶替
}

cout << m << endl;             // 直到程序结束为止，cout 最大的那个数
```

### 1. 深度认识 cin

cin 可以被视为 **Boolean 函数：
- 如果读入成功，会返回 true；
- 如果读入失败，会返回 false。

上面的代码块就用到了这个特性。while 只有在括号内的结果为 true 时才会循环。

###### <b style="color: #5DD0C8;">cin >> n 的新用法</b> 
该语句可以用于侦测一个 sequence 的结尾。
- 如果 sequence 还有内容，则将读入的内容赋值给 n；
- 如果 sequence 没有内容了，n 的内容不会改变。


**寻找是否存在比 n 更大的数：**

```c++
int n, num;
cin >> n;
bool found = false;                 // 添加 bool 函数 “found”，默认为 false。

while (not found and cin>>num) {    // found取反 and 如果还可以读入
	found = num>n;                  // 将 “num是否大于n” 赋值给 found
}

cout << found << endl;              // 如果 found=1，代表存在比n大的数。
```


### 2 - 语法：处理全部 / treat-all 
处理 - Treat

```c++
while (not 到达序列末尾）{
	逮一个新的东西 a;
	处理 a;
```

然后就会循环着处理序列里全部的 a 了。


### 3 - 语法：查找 / search

```c++
bool found = false;
开始查找;

while (not found and not 到达序列末尾) {
	带一个新的东西 a;
	if found = true;
}
```

- 当 found=1，代表找到了 / 序列中存在目标；
- 当 found=0，代表不存在。


###### <b style="color: #5DD0C8;">练习：longest repeated subsequence</b> 

- 题目：
	- 目标：找到一个序列中第一个字符串的最长连续重复子序列（longest repeated subsequence）的**重复次数**。
	
	- 例子：**cat** dog bird cat bird bird **cat cat cat** dog mouse horse
		- 该序列中的第一个字符串是 “cat”，之后存在一个 “cat cat cat” 的三次连续重复。我们需要看看这个序列中最大的 cat 连续重复（只有三次或者多于三次）。
		- 当然，在这之前我们还需要让电脑认识第一个字符串，并让它找到那个 cat cat cat。
	
	- 思路：我们需要找到

```c++
string First;   // 定义string变量，用于记录第一个字符串
cin >> First;   // 输入第一个字符串
string Next;    // 定义string变量，用于一个一个的记录之后的字符串
int Length = 1; // 定义int变量，用于记录目前长度
int Longest = 1;// 定义int变量，用于记录最长长度

while (cin >> Next) {                    // 当“存在可以输入给Next变量的字符串”时
	if ( First != Next ) Length = 0;     // 如果“第一个字符串”!=“现在这个”，忽略
	else {                               // 但如果等于“现在这个”的话
		Length = ++Length;               // 长度+1，结束，去看下一个（循环）
		if ( Length > Longest ) Longest = Length;  //检查，如果"++后">"最长"，顶替。
	}
```


###### <b style="color: #5DD0C8;">练习：search in dictionary</b> 

目标，在序列中查找是否存在和第一个字符串同样的字符串。字典以首字母顺序排列。

```c++
string word;            // 定义string变量
cin >> word;            // 输入string变量

bool found = false;     // 定义bool变量，初始值为 0。若 found=1，存在；若 =0，不存在。

string next = "";       // 定义string变量，初始值为空白字符串（因为字典可能是空的（？？））

while (not found and cin>>next) found = next >= word; 
// 当“非 不存在”与 “存在输入” 时，将“next是否大于等于word”的结果赋值给 found。
// 一旦 found=1，while循环条件被破坏，不再查找。
```


###### <b style="color: #5DD0C8;">练习：Increasing number</b> 
检测数字 n 的位数是否是从小到大逐渐提升的关系（前后两位大小不变也行，只要不降都行）。

```c++
int n;                            // 定义 n
cin >> n;                         // 输入 n
bool incr = true;                 // bool函数，while破坏条件
int previous = 9;                 // 定义int变量“上一个”，默认为 9，因为从末尾往首位看。

while (incr and n>0) {            // 当incr=1 与 n>0 时
	int next = n%10;              // 定义int变量，用于记录位数。（从末尾往首位数）
	incr = next<=previous;        /* 如果“下一个小于上一个”，incr继续等于1
	                                一旦“下一个大于上一个”，while 条件被破坏，结束循环*/
	previous = next;              // 将“下一个”的值定义为“上一个”，然后往后一位走
	n /= 10;                      // n弃掉一位，作为第一行的呼应 
}

if (incr) cout << "YES" << endl;  // 如果一切按计划进行，循环从尾到头没中断，输出 YES
else cout << "NO" << endl;        // 如果循环被中断了，说明不对劲，输出 NO
```
```
56688 - YES
134729 - NO
```
###### <b style="color: #5DD0C8;">重点 get</b> 
感觉这一课的重点也是
- 提前设置 bool 函数，作为 while 循环的结束条件
- 将结束条件添加到循环体中：
	- 可以将结束条件作为 “**条件达成的信号**”。一旦条件达成，破坏循环，节省算力；
	- 也可以将结束条件作为 ”**意外**“。一旦条件达成，代表没有按照预期发生，后续处理。


###### <b style="color: #5DD0C8;">练习：Insert a number in an ordered sequence</b> 
让首位数字按它大小插入到序列中它合适的位置。

```c++
int first;
cin >> first;
bool found = false;                      // bool函数，while破坏条件
int next;

while (not found and cin>>next) {        // 当 存在
	if (next >= first) found = true;     // 当
	else cout << next << " ";
}

cout << first;

if (found) {
	cout << " " << next;
	while (cin>>next) cout << " " << next;
}

cout << endl;
```

###### <b style="color: #5DD0C8;">重点 get</b> 
另外一个重点在于 ”在一次输入序列中同时赋值多个变量” 。结构如下

```c++
int a;
cin >> a;              // （如果有的话）将第一个数赋值给 a
int b;

while (cin >> b) {     // 将之后的值陆续赋值给 b 。如果之后没有值的话，会卡在这一步。
	
}
```

此时，当在终端输入一个序列 “1 2 3 4 5 6 7 8 9” 时，它会**将第一个值赋值给 a**（以空格作分隔）。之后的值会被陆续赋值给 b。

但是，一旦没有可读入的值，**while 循环不会被破坏**，**程序会被卡死在 while** 里面无法自拔。
	除非输入 ctrl + D 之类的指令。

为了解决这类问题，我们一般会**定义一个 bool 函数，用 `and` 将其与 `cin` 绑定**，来创立一个可以破坏 while 的条件（之前说过 cin 也会被视为 bool 判定）。
	但是**如果整个循环顺风顺水地结束，还是会被卡死在循环中**。


## REASONING ABOUT LOOPS: INVARIANTS
Invariant - 不变量

不变量的用处：
- 在进入循环之前，定义变量的值；
- 定义进入 post-condition  的必要条件；
- 定义循环体；
- 检测循环在何时结束。

不变量很重要，但选择一个合适的不变量并不简单。

建议：
- 将不变量作为所有循环的基础
- Use formal invariant-­‐based reasoning for non-­‐trivial loops（听不懂）


###### <b style="color: #5DD0C8;">Example with invariants</b> 
给一个大于等于 0 的数，计算 n! 的结果（ 1 * 2 * 3 * ... * (n-1) * n ）（0! = 1）。

我们发现：
- 当乘以一个序列中的数 i 之后，得到的结果是 i! 。同时，我们将此时的结果定义为 f（i! = f）。
- 这个序列中的数 i 一定**小于等于** n。

找到不变式 ：`f=i! && i≤n`。

```c++
int n;
cin >> n;
int i = 0;                  // i 的初始值为 0
int f = 1;                  // f 的初始值为 1，因为 0!=1

// 不变式：f=i! && i≤n
while (i<n) {               // 当 i 小于 n 时，执行循环。一旦 i 等于 n，跳出循环。
	// 此时 f=i! && i≤n
	++i;                    // 递增 i
	f = f*i;                // 计算 i! / n!
	// 此时 f=i! && i≤n
}
                            // 此时 f=i! && i≥n(因为跳出循环了，所以与while条件相反)
                            // 所以 f=n!

cout << f << endl;            // 输出 n!
```

选择一个好的不变式并不总是容易的，但它非常重要。推荐的做法是：

- 对于所有的循环，尽可能使用基于不变式的推理（即便是非正式的）。
- 对于复杂的循环，应该使用正式的不变式推理。

在阶乘例子中，不变式 `f = i! 且 i ≤ n` 是一个非常清晰的选择，它可以确保每次迭代都正确地更新 `f` 的值，直到循环结束时 `f` 成功计算出 `n!`。


###### <b style="color: #5DD0C8;">Calculating π</b> 
将所给数字首尾倒置（12345 → 54321）。

```c++
int n;                  // 变量 n 用于记录所给数
cin >> n;
int r = 0;              // 变量 r 用于倒置数（默认值是 0，为了让所给数=0时有话可说。

while (n>0) {
	r = 10*r + n%10;    
	n = n/10;
}

cout << r << endl;
```
















