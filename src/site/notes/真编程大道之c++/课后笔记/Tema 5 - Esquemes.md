---
{"tags":["Programació","UPC"],"dg-publish":true,"permalink":"/真编程大道之c++/课后笔记/Tema 5 - Esquemes/","dgPassFrontmatter":true,"created":"2024-10-19T20:47:00.082+02:00","updated":"2024-11-18T14:39:45.173+01:00"}
---


分为两个大章节：
- **Sliding window strategy - 滑动窗口策略**
	用于处理依赖邻居元素的序列，如：
	- 语法：处理全部
	- 语法：查找
- **Sequences of sequences - 序列的序列**

### 1.  Sliding Winow Strategy

###### <b style="color: #5DD0C8;">滑动窗口 - 初识</b> 
- **题目**
	计数，有多少 “递增对“ 的数量（现在这个数大于上一个数）。


- **思路**：想象我们将每个数都框选起来。
	
	我们要做的就是让电脑 ”一次性只看一对数“。
	- 若现在这个数大于上一个数，`++count`；
	- 不然，`count` 不变。
	
	一旦看完，”滑动窗户“ 去看下一对。
	
	![PRO - tema5 - 1.1.png](/img/user/%E7%9C%9F%E7%BC%96%E7%A8%8B%E5%A4%A7%E9%81%93%E4%B9%8Bc++/%E8%AF%BE%E5%90%8E%E7%AC%94%E8%AE%B0/%E7%9A%82%E7%89%87/PRO%20-%20tema5%20-%201.1.png)
	
	我们将 ”上一个数“ 定义为变量 `prev`，”现在这个数“ 定义为变量 `curr`。
	- 第一次：`prev=3, curr=12
	- 第二次：`prev=12, curr=8
	- 等等……

```c++
int count = 0;
int curr, prev;
cin >> prev;

while (cin>>curr){            // 若还有能读入的值
	if (curr>prev) ++count;   //   当“现在这个”比“上个”大，计数加一
	prev = curr;              //   （比较过后）将现在的“现在这个”定义为“上个”
}                             // 循环，迎接新的“现在这个”
cout << count << endl;
```


###### <b style="color: #5DD0C8;">滑动窗口 - 拓展</b> 
- 我们滑动的窗口不止可以双扇的，也可以三扇四扇甚至更多一起滑。
- 此外，treat-all 与 search 是可以同时进行的

**题目**：看序列中中是否存在字符串 ”hola“。该序列必定会以 "." 结尾。

**思路**：

- 现在，我们需要一个 ”四扇窗口“ 才能查找。
	- 这个四扇窗口可以由 1 * current 与 3 * previous 制成。
	- 它们需要一格一格地向前挪。

- 同时，我们的任务是查找，所以一旦找到就可以停了。

**注意**：如果当给的序列的字符数小于4呢？

```c++
char a, b, c;
char d;                                               // d 作为 current 用
bool found = false;

a='_'; b='_'; c='_';                                  // 随便给个默认值
cin >> d;                                             // 给“现在数”读入

while (not found && d!=".") {                         // 当读入的不是“.”时
	found = ( a=='h' && b=='o' && c=='l' & d=='a' );  // 四个条件同时满足，found=1
	a=b; b=c; c=d;                                    // 整体后挪
	cin >> d;                                         // 给d读入
}

if (found) cout << "yes" << endl;
else cout << "no" << endl;

```

- 代码解析
	为了应对字符数小于4的情况，我们将 abc 随便赋了值，并将 d 定义为序列中的第一个字符。
		此时，窗只有一扇。
	等到运行至 “整体后挪” 阶段，c 会取到当时 d 的值；再然后，d 会读入序列中的下一个数。
		此时，窗被扩大到两扇。
	再后挪一位，b=c，c=d，d 继续读入新值。
		窗被扩大到三扇。
	再挪，a=b，b=c，c=d，d继续读入新值。
		四扇窗齐了。
	再挪，就是四扇窗整体移动了。

类似的问题还有如：
- 找到序列中两数相差最大的一对（treat-all，两窗）；
- 计算同词连续最多重复次数（treat-all，两窗）；
- 判断该序列是否处于递增（search，两窗）；
- 找到最大的“波峰波谷”（treat-all，三窗）。


### 2. Sequences of sequences
在对待一个序列的序列时，需要理清两件事：
##### 2.1 Task structure - 任务结构
- 在序列的序列中：我们需要一个个的**处理全部序列**还是**一旦遇见特定序列就停止**？
	若是前者：treat-all sequences；若是后者，sequance search。
- 在单个序列中：我们需要一个个的**处理全部的elements**还是**一旦遇见特定element就停止**？
	若是前者：treat-all elements；若是后者：element search。


###### <b style="color: #5DD0C8;">1/4 例子：treat-all sequence + treat-all elements</b> 
- 给定一些含有值的序列，每个序列以0结尾。计算每个序列的最大值，并算出每个序列的最大值之和。

- 个人思路：
	```c++
	当读入时：
		当 next != 0 时：
			若 next>curr, max=next;
			窗口右滑
		// 当遇到 0 时，开始集合
		sum += max;
	
	cout << sum
	```
- 个人**错误**实例
	```c++
	int curr;
	cin >> curr;                               // 这行需要删掉，大忌！！！
	int next;
	int max = curr;
	int sum = curr;
	
	while (cin >> next){                       // 因为这行也会执行输入指令。
		while (next != 0 && cin >> next) {     // 如果不删的话会一次性输入两遍
			if (next > curr) max = next;
			curr = next;
		}
		sum += max;
		cout << max << endl;
	}
	
	cout << max << endl;		
	```
	
	- 错误总结：
			1. 不可以双层 cin。这样会导致重复读取，一口气跳过两个。
			2. 这个情况下不需要滑动窗口策略。
- 实例：
```c++
int sum = 0; 
int curr; 

while (cin >> curr) {                   // 读入，用于切换序列
	int max = curr;                     // 每次切换序列的初始化：将序列首位赋值给curr
	while (curr != 0) {                 //     进入序列后，重复执行，直到遇到0为止
		if (curr > max) max = curr;     //     检测是否大于           
		cin >> curr;                    //     下一位，继续循环
	}
	cout << max << endl;                // 此时必定已经遇到0，序列结束，输出当前max
	sum += max;                         // 累积sum
} 

cout << sum << endl;
```

这种序列中的序列需要嵌套两个循环：

- 大循环，用于**切换至下一个序列**。
- 小循环，用于在序列之一中**切换到下一个值**。


###### <b style="color: #5DD0C8;">2/4 例子：search sequence + treat-all elements</b> 
是否存在至少一个序列，序列内的值相加大于 50？

- 个人思路：
	```c++
	bool found = false;
	
	while (not found && cin>>n) {
		初始化
		小语句：
			让所有值相加，直到遇到0
		found = sum>50
	}
	```
- 个人实例：
	```c++
	int n;
	bool found = false;
	
	while (not found && cin >> n) {
		int sum = 0;
		while (n != 0) {
			sum += n;
			cin >> n;
		}
		found = sum>50;
	}
	
	cout << found << endl;
	```

对辣！

###### <b style="color: #5DD0C8;">3/4 例子：search sequence + search element</b> 
找到第一个存在以 "3" 结尾的数字的序列。

- 个人思路：
	```c++
	bool found = false;
	使用 char n 来一个字符一个字符的寻找。
	使用 int count 来记载序列号
	
	while ( not found && cin >> n ) {
		初始化
		while ( n!='0') {
			cin >> n;
			found = n!=3
		++count;
	
	输出
	```
- 个人实例：
	```c++
	int n;
	int count = 0;
	bool found = false;
	
	while (not found && cin >> n) {
		while (not found && n != 0) {
			int end = n % 10;
			found = end == 3;
			cin >> n;
		}
		++count;
	}
	cout << count << endl;
	```

对辣！

###### <b style="color: #5DD0C8;">4/4 例子：treat-all sequnce + search element</b> 
多少序列包含十的倍数？计数。

- 个人思路：
	应该和上题大差不差（错了！！大错特错！）
- 个人**错误**实例：
	```c++
	int n;
	int count = 0;
	
	while (cin >> n) {
		bool found = false;                 // 这次的初始化是 bool 函数
		while (not found && n != 0) {       // 在found=1时还！不！能！停！得读到0！
			int end = n % 10;               // 因为逻辑的改变，所以循环体也不对了 :(
			found = end == 0;
			cin >> n;
		}
	if (found) ++count;
	}
	
	cout << count << endl;
	```
- 实例：
```c++
int n;
int count = 0;

while (cin >> n) {
	bool found = false;                 // 每次循环初始化 found。
	while (n != 0) {                    // 在found=1时还！不！能！停！得读到0！
		int end = n % 10;               
		found = end == 0;
		if (n%10 == 0) found = true;    // 之前的bool判定随着n在序列内的读入而动态地变化
		cin >> n;                       // 比如在从10位移到76的时候，会从true变false 
	}                                   // 上面这种bool判定就不会。一旦触发，钉死
if (found) ++count;
}

cout << count << endl;
```

为了正确的计数，我们在发现了那个 “序列的序列之一包含10的倍数” 之后，还不能跳出内循环。我们**必须等到读到 “0”，等到读完那个序列才可以跳出内循环**。
- 如果同个序列中有两个十的倍数，会计数两次
	1. 读到十的倍数，跳出内循环
	2. ++count
	3. 从之前跳出的地方继续读入，遇到同个 sequence 里第二个十的倍数，跳出内循环
	4. ++count
	5. 从之前跳出的地方继续读入……

**if 语句的 bool 判定**：用于一旦触发就不再更改的情况。


##### 2.2 Imput structure - 输入结构
- 每个序列都有自己的 “**结束标记**”，还是每个序列中的物品只有 ”**固定数量**“ 呢？
- 我们要读入全部的序列 **直到不可读为止**，还是只需要读入**固定数量**的序列即可呢？

###### <b style="color: #5DD0C8;">1/6 例子：Known number of sequences, known number of elements in each</b> 
固定序列，固定物品

```输入
4                           
5  12 10 8 7 5
1  22
0
3  0 -4 1
```

- 首先，会用一个值表达**共会输入多少个序列**。
	这个例子中是 4，它之下一共也就四个序列。

- 接下来的值代表 ”每个序列包含几个 elements“ 。

- 经过这些数量的 elements 后，**如果还有更多序列**（取决于全部输入的第一个值），下一个值表达第二个序列包含多少 elements 。

话说回来，实际上，上面整整齐齐的格式可能会以下面这种方式呈现。它们是等价的，我们必须学会辨别每个值代表什么。
```
4 5 12 10 8 7 5 1 22 0 3 0 -4 1
```


**代码演示**
```c++
int N_sequence;                            // 定义总序列数
cin >> N_sequence;                         // 输入。因为只可能有一个序列数，不必循环

for (int i =0; i<N_sequence; ++i){         // 临时变量i，每次读完一个序列时递增
	int N_element;                         // 定义总Element数，
	cin >> N_element;                      // 输入。因为可能不止一个总Elem.所以需要循环
	
	for (int j=0; j<ne; ++j){              // 临时变量j，每次读完一个Elem.时递增
		int Element;                       // 定义单个Element
		cin >> Element;                    // 读入单个Element
		// 处理每个 elements              
```


###### <b style="color: #5DD0C8;">2/6 例子：Known number of sequences, final mark in each</b> 
固定序列，结束标记

```输入
4                           
12 10 8 7 5 0
22 0
0
4 1 0
```

- 这里依然存在开头的值表达总序列数。

- 使用 ”0“ 代表序列的结束。
	我们需要在碰到 ”0“ 时结束内循环。
	为了避免第一位就是 0，我们**需要提前读入**，如下：

```c++
int ns;                                // 同上
cin >> ns;

for (int i=0; i<ns; ++i) {             // 同上
	int x;                             // 定义单个Element，同上
	cin >> x;                          // 提前录入序列第一位。若第一位是0直接跳过序列
	
	while (x != 0) {                   // 检测条件
		// 处理elements
		cin >> x;                      // 处理完后，读入下一位
	} 
}
```


###### <b style="color: #5DD0C8;">3/6 例子：Unknown number of sequences, known number of elements in each</b> 
读完序列，固定物品

```输入                         
5  12 10 8 7 5
1  22
0
3  0 -4 1
```

几乎是上面去掉 ns 后的版本。

```c++
int ne; 

while (cin >> ne) {
	for (int j=0; j<ne; ++j) { 
		int x; 
		cin >> x;
		// 处理
```


###### <b style="color: #5DD0C8;">4/6 例子：Unknown number of sequences, final mark in each</b> 
读完序列，结束标记

```                         
12 10 8 7 5 0
22 0
0
4 1 0
```

```c++
int x; 

while (cin >> x) {
	while (x != 0) {
		cin >> x;
	} 
}
```


###### <b style="color: #5DD0C8;">5/6 例子：Mark indicating no more sequences, known number of elements in each.</b> 
序列结束标记，固定物品

```
5  12 10 8 7 5
1  22
0
3  0 -4 1
-1              
```

这里，**读完上个序列后，如果第一个值是 -1**，代表序列结束

```c++
int ne;
cin >> ne; 
while (ne != -1) {                // 当读完上个序列之后，第一位不是 -1 时，继续读入
	for (int i=0; i<ne; ++i) { 
		int x; 
		cin >> x;
	}
	cin >> ne;
}
```


###### <b style="color: #5DD0C8;">6/6 例子：Mark indicating no more sequences, final mark in each..</b> 
序列结束标记，物品结束标记

```
12 10 8 7 5 0
22 0
0
4 1 0
-1
```

```c++
int x;
cin >> x; 
while (x != -1) {
	while (x != 0) {
		cin >> x;
		}
	cin >> x;
}
```




