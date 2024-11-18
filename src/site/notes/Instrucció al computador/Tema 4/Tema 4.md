---
{"tags":["ADD","SUB","Mx","IC","Num_Entero","SL","SRA","Comparador","组合电路","UPC"],"dg-publish":true,"permalink":"/Instrucció al computador/Tema 4/Tema 4/","dgPassFrontmatter":true,"created":"2024-10-09T07:18:26.772+02:00","updated":"2024-11-18T14:41:33.103+01:00"}
---


### 4.1 Introduccion

### 4.2 El sumador binario
设计开始！

##### 4.2.1 Algoritmo aritmético
==Aritmético / 计算的 / 算数的==
###### <b style="color: #5DD0C8;">问题与规划</b>

**题目：使得 Xn + Yn = Wn。


**辅助定义：**

- X 与 Y 代表它们各自的值，用于表达这个值的**字符串**由 X_n-1 ... X0 这些**字符**组成。其中，n 代表它们为第 n 位（4 位数 “2005” 中的 “2” 处于第3位；“5” 处于第0位）：
	 X = X_n-1 * X_n-2 * ... * X1 * X0；Y = Y_n-1 * Y_n-2 * ... * Y1 * Y0

- 字符串上的每个字符（第 i 位字符）都来自于 b 基（b 基可能是二进制、十进制、十六进制等等）：
	 X_i, Y_i ∈ {0, ... , b-1}

- X 与 Y 在十进制中被表达为 X_u 与 Y_u。我们的目标是使 X_u +Y_u = W_u。


**算出 W 最多是几位数**

- X 与 Y 在 b 基中最小的值可能是 0，最大可能是 (b^n) -1（四位数的 n 是 4，最大数是 (10^4) - 1 = 9999）。 
	 0 ≤ X_u, Y_u ≤ (b^n) - 1

- 所以 W_u = X_u + Y_u 处于 0 ≤ W_u ≤ 2(b^n) - 2 这个域中。
	 例：9999 + 9999 
		 = 19998 
		 = 2 * (10000) -2

- 在最极端的情况下，W_u 会有 n+1 位（比 X_u, Y_u 多一位），就是说它**一定小于 (b^n+1) - 1**。

- 也就是说，“n 再多一位” > “W_u 最大值” > "X_u, Y_u 最大值"
	(b^n+1) -1 > (2b^n) - 2 > b^n -1

==感觉这里最需要理清的是 “n 位数” 与 “第 n-1 位” 、“第 0 位” 之间的逻辑；
同样的，“n+1” 位数最开始的那位被称为 “第n位”。==

- 那么，Sumatorio：
![4 - 2 EQ1.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%202%20EQ1.png)

- 同时，组成 W 的每一个字符依然从属于 b 基。
![4 - 2 EQ2.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%202%20EQ2.png)


###### <b style="color: #5DD0C8;">解答</b>
我们将加法分成两个阶段来解决。

**Fase 1**
在这个阶段中，我们让 **X 与 Y 同一位上的值进行相加**，并且将得出的结果临时的赋到 W 的那一位上
	例：设 base 是十进制的。
		X_13 位为 “8”；Y_13 位为 “7”。它们相加得出 W_13 位是 “15”。
		虽然在十进制中一位数不可能是 15，但是我们不管。先把答案临时的挂着，**等到第二阶段的时候再进位。

相应的，因为 X_n 位 与 Y_n 位不存在，W_n 位就也不可能存在。不过没事，等到第二阶段的时候一进位可能就有了。

![4 - 4.2.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.2.11.png)

设 b = 10，n = 4，X = 6493 与 Y = 8119：

![4 - 4.2.12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.2.12.png)

我们看到，临时的 W 位数上不少都超过了十进制的上限，第 n 位也被假设为0。这些都是这个阶段的表现：**符合 EQ1 但或许不符合 EQ2** 。


**全新的公式、转型的方法**

下面的公式展现了进位的原理：

![4 - 4.2.13.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.2.13.png)

下面展现了由进位原理推理过去的船新公式：

![4 - 4.2.14.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.2.14.png)

大意就是，**如果 W_k 位减去 b（基数），W_k+1 位应在原有的数上 +1**。

这个 k+1 位被称为 ==**acarreo（*carry*）**==，我们使用 **“c_k+1”** 表达。


**Fase 2**
这阶段中我们需要**从末位到首位的**（从 k=0 到 k=n_-1）**，一个一个看是否有进位的必要**。使得每一位上的值同时遵循 EQ1 与 EQ2：

![4 - 4.2.15.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.2.15.png)

进位完成，k=3 那一位也有了自己的值。


**总结**
实际上这个原理**和小学学的竖式加法一毛一样**。看看有没有满 b，满 b 进一。

![4 - 4.2.16.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.2.16.png)


##### 4.2.2 Implementación

###### <b style="color: #5DD0C8;">Full-Adder</b>
在二进制时，上面关于 c_k+1 与 w 的两个阶段只需要三个变量就可以完美实现。所以，我们可以组成一个**三入口双出口**的电路，称为 **Full-adder (Fa)**，并想办法让它**实现下面语句的逻辑运算**。

![4 - 4.4.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.4.png)

运算规则就是 Xk + Yk + Ck（之前一位的进位）。上方的真值表已经展现了所有的八种排列组合。

![Pasted image 20241009184500.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%203/%E5%9B%BE%E7%89%87/Pasted%20image%2020241009184500.png)

出炉！是个有**三入口，双出口**的电路。出口名字中的 c 对应 Carry（进位）；s 对应 Sum（和）。


###### <b style="color: #5DD0C8;">Sumador binario </b>

Fa 是可以堆叠的，其**堆叠的次数 = n（位数）**。下图展示其语法与图示。

![4 - 4.5.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.5.png)

```c++
cin >> n; // n视情况决定（位数）

c0 = 0;
for (k=0; k<n; ++k) { // k代表所在的位数
	(c_k+1, w_k) = Fa (x_k, y_k, c_k);
}
w_4 = c_4;
```

- 第一次的入口 c **固定为 0**；
- 最后一次的输出 c **并不多余，而是代表第 n+1 位 w 的值**；
- 将上一位的输出 c 与下一位的输入 c 相连。

###### <b style="color: #5DD0C8;">不可表达的情况</b>
什么情况下，电路的结果无法用 n 位数表达？—— **当 w_n = 1 的时候** 。

实际上，计算机不是理想的 x_n, y_n, x_n+1 位数结构。实际上，它们的位数都一样（x_n, y_n, w_n）。所以，表达 w_n 时有否进位的 **c_n+1 就代表 w_n+1 的值**。

###### <b style="color: #5DD0C8;">Bloque ADD</b>
Bloque ADD 由**双入口与一出口**组成。侧边的 **c 出口**是为了代表 w_n+1 的值，就和上个小节说的一样（但若确定不需要 c 也可以不用）。

在作图方面，有一些需要注意的点：

- ==Buses / 总线==比 bit 线更**粗**。同时，总线应是**蓝色**的；
- bits 的命名方法是 **b0 - b15**；
- 默认情况下，ADD 包含 16 个 Fa 电路。

![4 - 4.3.2.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.3.2.png)


##### 4.2.3 Implementación del Full-adder 

###### <b style="color: #5DD0C8;">Half-Adder</b>
**Half-Adder (Ha)** 是一个有 x, y 双入口+ c, s 双出口的电路。其中，c 还是表示 Carry；s 也是表示 Sum。

它由一个 And 与一个 Xor 组成（或者一个 And  + （2Not 2And 1Or））。

它的作用是将输入的两个信号**相加**。且接下来我们会用它来组成一个 Full- Adder。Ha 的 TV 与结构见下图：

![4 - 4.7.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.7.png)


###### <b style="color: #5DD0C8;">Full-Adder</b>
还记得，Fa 用于将三个入口的值相加。我们可以通过一些方式来拼凑 Ha，使其构成 Fa。

整个 Fa 由三个 Ha 组成，分别为 Ha abc。其中，每个 Ha 的输入由 ==相同peso的比特== 组成。
	下图中，“2” 右上角，“小写字母” 右下角，以及 “xyz” 右方的数字都代表它的 **peso（量级）**。**从 c 口输出的比特量级 +1**。

**只有相同 peso 的比特才可以被输入到同一个 Ha 中相加。**

![4 - 4.8.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.8.png)

此外，观察 TV 后我们发现，出口 w2 的比特**永远是 0**（因为三个 1 相加等于 3，在二进制中为 11，是两位数）。这种情况下，最后一个出口就不需要是 Ha 了。**一个 Or-2 门即可平替**。

这样的话，可能会有更少的 Tp 、空间消耗与制作成本。

**节省与优化**
如果一个电路中的 Fa 存在一个空闲的相加口（不是 carry 口），它一般可以被 Ha 平替。


### 4.3 El restador binario

##### 4.3.1 Algoritmo aritmético

###### <b style="color: #5DD0C8;">问题与规划</b>

**题目：使得 W_u = X_u - Y_u

**辅助定义**

- 设 Xu 和 Yu 是 n 位数，那么它们的数值一定小于 2^n -1（二进制）
	0 ≤ Xu, Yu ≤ 2^n – 1

- 两数的差 Wu 就一定在正负 2^n -1 之间
	-(2^n - 1) ≤ Wu ≤ 2^n -1

###### <b style="color: #5DD0C8;">不可表达的情况</b>
但是，**在计算机的二进制中，负数是无法被表达的**。
	这也是**第二种计算机无法表达的值** - Nº no naturales（还记得第一种是 n+1 bits）。

符号代表的值与加法器一样：
	- 二进制
	- n-1 ... 0 代表字符所在的位数
	- i 代表任意的位数

![4 - 3 EQ3.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%203%20EQ3.png)

![4 - 3 EQ4.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%203%20EQ4.png)


###### <b style="color: #5DD0C8;">解答</b>
依然是将解题思路分为两个阶段

**Fase 1**
建立一个临时变量 Wi，等于 Xi - Yi。这是和加法器一样的同位相减，临时变量 Wi 的结果也可能超过二进制规则的限制（负数），会违背 EQ 4，但我们先撇下不管。

设 X=1001，Y=0011。这一步要做的事如下。我们看见 bit 1 的结果就超过了二进制的规则：
![4 - 4.3.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.3.11.png)

**退位的方法**
如果 k 位数的结果在第一阶段中为 -1，那么我们可以向 k+1 位借一位（ k+1 位的值在原有的基础上 -1），**同时 k 位的值 +1，变为 1（符合 EQ4）。**
	如果 k+1 位在被借位后的结果也是 -1，那么就向 k+2 位借。以此类推……
	该原理的证明过程在 pdf 中，可以去慢慢推理。

**Fase 2**
第二阶段的目标就是从 k=0 开始，一个一个的朝 k=n-1 找不符合 EQ4 的值。有的话就向上位借1，直到整个 W 都符合 EQ4 为止。

需要额外注意的是，==如果 n-1 位（首位）的结果也是 -1，代表 W 是负数。

新概念：==borrow（借位）==，是和加法器中的 carry 相似但相反的概念，代表上位借一。

一位上的 borrow 最小**不超过 -2**（自己本身欠一 + 上一位的借一）。**欠二再借位的话自己就不会变成 1，而是变成 0**。

![4 - 4.3.12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.3.12.png)

###### <b style="color: #5DD0C8;">Algoritmo</b>

```c++
b0 = 0;                        // 第0位不可能欠位
for ( k=0; k<n; k=k+1){
	if( Yk+bk ≤ Xk ){          // 如果答案大于等于零（被减数大于等于减数）
		int Wk = Xk-(Yk+bk);   // 处于 k 位的 W 正常运算
		int bk+1 = 0;          // 进位的 borrow = 0
	} else {                   // 如果答案小于零（被减数小于减数）
		Wk = 2 + Xk-(Yk+bk);   // 上位借一，k 位 W 加二
		bk+1 = 1;              // 进位的 borrow = 1
	}
}
```


##### 4.3.2 Implementación del restador binario
缀后，是想办法用电路实现 ==bucle / 循环体== 的时候了（就是 for 语句包含的那两块）。

该电路被称为 ==Full-Subractor / Fs==。与 Fa 一样是**三入口双出口**。
	- 出口的 s 代表 subtract（差）；
	- 出口的 b 代表 borrow（欠位）。

电路入口的**顺序是不可更换的**。第一个入口固定为 **加数**，第二与第三个入口为 **减数**。所以我们会**把加减符号标记在电路上**。

具体画法与 TV 如下：

![4 - 4.3.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.3.21.png)


###### <b style="color: #5DD0C8;">Restador binario con Full-subtractors en propagación del débito</b>
减法模块被称为 **SUB**。它与 ADD 模块非常相似：
	- 都是 XY 双入口，W 出口加一个小分支；
	- 每一位数字都需要一个 Fs 电路来计算；
	- 如果 b4 的结果为 1，代表计算结果是负数。

额外注意，SUB 模块入口的**顺序也是固定的**。X 入口固定加数，Y 入口固定减数。加减符号在模块上**被标注**。

![4 - 4.3.22.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.3.22.png)


### 4.4 Desplazador de k bits a la izquierda (multiplicador por 2k)
让俺们开始研究用于乘二的模块吧

##### 4.4.1 Algoritmo aritmético de multiplicación por 2 en binario

###### <b style="color: #5DD0C8;">问题与规划</b>

**题目：使得 W_u = 2 * X_u

**辅助定义**

- 设 Xu 是 n 位数，那么它的数值一定小于 2^n -1（二进制）
	0 ≤ Xu ≤ 2^n – 1

- Wu 就一定在 2^(n+1) -2 之间
	Wu ≤ 2^(n+1) -2

- 所以，Xu 的最大位数与 Wu 的最大位数是不同的。**W 的最大位数可能比 X 多一位**。
	X = X_n-1 ... X_0
	W = W_n W_n-1 ... W0

符号代表的值与加法减法器一样：
	- 二进制
	- n-1 ... 0 代表字符所在的位数
	- i 代表任意的位数

![4 - 4.4 EQ5.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.4%20EQ5.png)

![4 - 4.4 EQ6.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.4%20EQ6.png)

**核心概念**

因为 2 * (2^i) == 2^(i+1)。我们一旦把这个公式套到 EQ5 上，我们就能得到乘二的拓展算式：

![4 - 4.4.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.4.11.png)

这个公式中，**W 一侧的不变，但在 X 一侧，所有 2 的次方都被 +1**。
	这是因为我们套用上方的公式，**把 EQ5 中的 2 挪到括号里面**，以 2^i+1 的方式呈现了。

所以，得出结论：

![4 - 4.4.12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.4.12.png)

- W_0 = 0
	这是因为二进制乘二后，最末位的数**必定为零**（0\*2=0, 1\*2=10)。

- W_i = X_(i-1)  para  1 ≤ i ≤ n
	这是拓展算式的二次拓展。

可以说，==二进制乘以二就相当于将 Xn-1 ... X0 整体往左移一位，并往末尾加上一个零。==
	更简单一点说，就是往**数右边末尾（menor peso）插入一个零**。
	例：10010  → 乘二→  100100

![4 - 4.4.13.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.4.13.png)

###### <b style="color: #5DD0C8;">不可表达的情况</b>
还记得，计算机中，乘数与被乘数的 bits 一致。所以，**当 X_n-1 位为 1 时**就会发生 ==overflow / 溢出==。


##### 4.4.2 Algoritmo aritmético de multiplicación por potencias de 2
在了解了乘二之后，我们自然也能理解了乘以二的 k 次方的方法了：就是**连续乘以 2，k 次**；或者往数的右边**添加 k 个零，同时往左移 k 位**。

公式如下：

- 目标：
	W_u = 2^k * X_u

- 设 X_u 有 n-1 位
	X = X_n-1 ... X0
 
- 此时，W_u 需要有 n + k-1 位才可以承载。
	W_(n+k-1) W_(n+k) ... W_0
		k-1 是因为 2^1 时并不需要进位，因为 2^1 = 2。直到到 2^2 时才需要。

但这也是理论上的计算。**在实际的模块中，输出与输入的位数是一致的。一旦进位，无论多少，都无法表示**。

![4 - 4.4.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.4.21.png)


##### 4.4.3 Implementación del multiplicador por 2^k (SL-k)
用于乘以 2 的 k 次方的模块被称为 **Shift Left-k (SL-k)**。它的设计与逻辑计算一点关系都没有，**只是用于移位输出**。

SL - k 由**一出口一输出**组成。下图是一个 16 bits 的 SL-4，用于输入一个 16 位的二进制数，并将其乘以 2^4 。

**我感觉**它可以分为三个部分来理解（G老师觉得我说得没错）：

- 1：<u>输入</u>的前 k 位
	我们会将这 k 位连接至一个 Or-k 门。这是为了检测这几位中**有没有 1**。一旦有，overflow，该数无法表达。

- 2：<u>输出</u>的 后 k 位
	这部分**固定为零**。

- 3：中间的几位
	这部分就是简洁明了的**移位**，X_i → W_(i+k)。

![4 - 4.4.31.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.4.31.png)


### 4.5 Desplazador de k bits a la derecha (divisor por 2^k)

##### 4.5.1 Algoritmo aritmético de división por 2 en binario

###### <b style="color: #5DD0C8;">问题与规划</b>

**题目：使得 W_u = X_u / 2

**辅助定义**

- 设 Xu 是 n 位数，那么它的数值一定小于 2^n -1（二进制）
	0 ≤ Xu ≤ 2^n – 1

- Wu 就一定在 **2^(n-2) -1** 之间
	**Wu ≤ 2^(n-2) -1**

- 所以，Xu 的最大位数与 Wu 的**最大位数是相同的**。**结果一定能被表达**。
	X = X_n-1 ... X_0
	W = W_n-1 ... W0

符号代表的值与加法减法器一样：
	- 二进制
	- n-1 ... 0 代表字符所在的位数
	- i 代表任意的位数

![4 - 4.5 EQ7.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.5%20EQ7.png)

![4 - 4.3 EQ8.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.3%20EQ8.png)

因为 2^i / 2 = 2^(i-1)，由此延伸出拓展公式（Wi = Xi+1 位）：
![4 - 4.5.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.5.11.png)

所以，二进制中的除以二就相当于==将 X 中的 bits 往右移一位，并在最高位（bit de mayor peso）插入一个零==，形成 W。
	右移后，==X 的末尾直接被抛掉==，不管是 1 是 0。

下面是 1001 除以二。我们看到 1001 → 100，末尾的 1 直接被抛弃了。

![4 - 4.5.12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.5.12.png)

###### <b style="color: #5DD0C8;">不可表达的情况</b>
无，详见 “问题与规划” 中的第三条。


##### 4.5.2 Algoritmo aritmético de división por potencias de 2
略，一例以蔽之：

![4 - 4.5.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.5.21.png)


##### 4.5.3 Implementación del divisor por 2^k (SRL-k)

用于进行除法的模块被称为 **Shift Right Logically - k (SRL-k)**。

原理与 SL-k 几乎一样，唯一的区别在于，这里**连 Or-k 门也不需要了**，结果**永远是可表达的**。

- 1：<u>输入</u>的最右边 k 位
	智多星。

- 2：<u>输出</u>的 后 k 位
	固定为零。

- 3：中间的几位
	这部分就是简洁明了的**移位**，**X_i → W_(i-k)**。
	注意，是 i-k，减法！

![4 - 4.5.31.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.5.31.png)


### 4.6 Comparadores de números naturales

我们将组建三个电路，分别是 EQ，LTU 与 LEU。**所有比较器的原理都是 X 减去 Y**：
*“Unsigned” 代表 XY 的脚缀 “u”。*

- w = **EQ**(X,Y)，相等，*Equal*：
	`if (X == Y) w=1; `
	`else w=0;`
	
	**EQ 判断由 SUB + z 组成：**
		- SUB 代表减法模块；
		- z 模块是个全新的模块。它由**五个 Or-4 嵌套成两个层级，并在最终加上一个 NOT 构成**。
	
	**逻辑：
	1. SUB 模块运算，结果为 W；
	2. z 模块检查。当检查到**结果为 n 个 0** 时（0丁洋本洋），第二层级的 Or-4 输出 0，并在最后被 NOT 反转变成 1。

![4 - 4.6.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.6.01.png)


- w = **LTU**(X,Y)，小于，*Less Than Unsigned*
	`if (Xu < Yu) w=1;`
	`else w=0;`
	
	它只由 SUB 模块组成，原理来自于用于判断结果是否为负数的小出口 ”b“。若它的结果为 1，代表 LTU 的成立。


- w = **LEU**(X,Y)，小于或等于，*Less or Equal Unsigned*
	`if (Xu <= Yu) w=1;`
	`else w=0;`
	
	它由 SUB + z 模块组成，是 EQ 与 LTU 的叠加。只要小出口 ”b“ **或者** z 模块的其中一个（Or-2）通过，LEU 成立。

![4 - 4.6.02.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.6.02.png)

共性：如果**成立，w=1**；如果不成立，w=0。


### 4.7 Operadores lógicos bit a bit

##### 4.7.1 AND bit a bit
它将 Xi 与 Yi（**同位的 bit**）进行 AND 比较。此时，W, X, Y 的**最大位数相等**，都是 n-1。

![4 - 4.7 EQ9.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.7%20EQ9.png)

在模块中，需要对比的**字符的数量 = AND-2 门的数量**。如下：

![4 - 4.7.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.7.11.png)

![4 - 4.7.12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.7.12.png)

##### 4.7.2 OR y XOR y NOT bit a bit

OR 与 XOR 都是 -2 门，其它略过。
NOT 只有一个入口（X），其它略过。

![4 - 4.7.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.7.21.png)

![4 - 4.7.22.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.7.22.png)


### 4.8 Diseño de multiplexores
浪我们用新学的知识重新设计一个 Multiplexor！

##### 4.8.0 什么是 Multiplexor
但在此之前，先让我们来复习一下什么是 Multiplexor。之前还没系统性的介绍过这个模块的作用

Multiplexor（Mx-a-b）的中文称呼是**数据选择器**。顾名思义，它的用处就是**从多个输入中选择其中一个作为输出**。a 代表 ”备选 x 口的数量“，b 代表 ”目标 x 口的数量“。
	如果**共有 a 个**备选 x 口，而我们只需要**选择一个**作为输出，那这就是个 Mx-a-1.

每个 Mx 都由三种口子组成：x、s 与 w：

- **输入：x**
	x 代表备选的输入选项。

- **输入：s**
	- s 代表 select，一般它会有较多位，但少于 x 的数量。
	
	- x-s **数量上**的对照关系如下（设有 n 个 x 与 m 个 s）：
		x 的数量 = 2^m
	
	- 一般 x-s **逻辑上**的对照关系如下（设有 s0 s1 与 x3 x2 x1 x0（2^2 = 4））：
		- 当 select 为零 (00)，那么 X0 信号需要为真（00 0001, 00 1001, 00 1101, 00 1111 都行，但是 00 0010, 00 1010 之类的就不行）。
		- 当 select 为三 (11)，输出 X3 信号需要为真（11 1000, 11 1101...） 。

- **输出：w**
	w 代表输出，就是 s 口对应的 x 口的值。

##### 4.8.1 Multiplexor Mx-2-1

举个例子，在一个 Mx-2-1 电路中：

第一步，我需要理清自己的目标，并画出它的 TV（右边那个小的）。如下：
![4 - 4.8.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.8.01.png)

第二步，画出相应的电路图，如下：
![4 - 4.8.02.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.8.02.png)


##### 4.8.2 Multiplexor Mx-8-1
现在让我们造一个 Mx-8-1 （八选一）：

###### <b style="color: #5DD0C8;">概念图、概念 TV</b>

![4 - 4.8.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.8.21.png)

- **模块概念图**
	1. **计算 s 口的数量**
		知道 x 口的数量后，经过简单的计算，我们得出 2^3 = 8。所以三个 s 口足矣。
	2. **画图**
		一般来说，s 口处于左侧，选择口处于上方。同时，注意**八选一实际上的编号是 0-7！！**

- **真值表**
	确保 s 口表达的二进制数字与 x 口的号码分别对应即可。

###### <b style="color: #5DD0C8;">电路绘制</b>

我们选择通过树状分布 Mx-2-1 来组成一个 Mx-8-1。

看着概念 TV 上的思维指引，我们发现它可以被有逻辑的一开二，二开四，形成一个初步的树形结构。此外，电路的结构或许也可以按照这个来？

- 当 **s2 为 0 时**，它掌控 x0 - x3 的输出：
	
	- 当 s2 为 0 且 **s1 为 0 时**，x0, x1 被掌控：
		- 当 s2 为 0, s1 为 0, **s0 为 0 时**，x0 被输出； 
		- 当 s2 为 0, s1 为 0, **s0 为 1 时**，x1 被输出。
	
	- 当 s2 为 0 且 **s1 为 1 时**，x2, x3 被掌控：
		- 当 s2 为 0, s1 为 1, **s0 为 0 时**，x2 被输出； 
		- 当 s2 为 0, s1 为 1, **s0 为 1 时**，x3 被输出； 

- 当 **s2 为 1 时**，它掌控 x4 - x7 的输出：
	
	- 当 s2 为 1 且 **s1 为 0 时**，x0, x1 被掌控：
		- 当 s2 为 1, s1 为 0, **s0 为 0 时**，x4 被输出； 
		- 当 s2 为 1, s1 为 0, **s0 为 1 时**，x5 被输出。
	
	- 当 s2 为 1 且 **s1 为 1 时**，x2, x3 被掌控：
		- 当 s2 为 1, s1 为 1, **s0 为 0 时**，x6 被输出； 
		- 当 s2 为 1, s1 为 1, **s0 为 1 时**，x7 被输出； 

![4 - 4.8.22.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.8.22.png)


### 4.9 Análisis de circuitos con bloques

那些计算模块的输入一般有很多的 bits（因为输入的都是字符串）。就和下图一样，三条 8 bits 的buses 可以整出 2^24 行的 TV。

面对这种情况，我们只需要**计算指定的一个或几个组合**即可，列出整个 TV 就不是人干的。

###### <b style="color: #5DD0C8;">电路计算与验证它的方法</b>

**题目与解答**

X = 11011001, Y = 10110111, Z = 11000101。三者被输入进下图电路，求 W 输出。
条件：不可能发生不可表达的情况（p=0, q=0)。

![4 - 4.9.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.9.01.png)

深思熟虑，得出 W=01010011。

**验证**

我们可以把 xyz **翻译成十进制**自然数，并在次基础上运算。最后，把十进制运算结果翻译成二进制，看看两者是否匹配。
	- Xu=217, Yu=183, Zu=197
	- Wu = (Xu-Yu) + (Zu/4)) = 83
	- 83 = 01010011
	- 答案正确！


### 4.10 Diseño de nuevos bloques aritméticos
俺们已经掌握辽一些基础了，可以开始整奇奇怪怪的活了！
##### 4.10.1 Incrementador
Incrementador 类似于 ADD 模块。它执行两件事情：
1. 输入数字串
2. 对数字串 +1

它和 ADD 模块的区别有三：
1. 它只有**单输入口**。
2. 将 Fa **用 Ha 代替**。
3. 将第一个 Ha 电路的入口由 0 改成 1。

![4 - 4.10.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.10.11.png)

##### 4.10.2 Multiplicador por 5

**题目：使得 W_u = 5 * Xu

**辅助定义**

- 设 X 有 n 位；W 有 m 位。

- 因为 5 = (1 * 2^2) + (0 * 2^1) + (1 * 2^0) = 2^2 + 1，我们得到拓展公式：
	W_u = X_u*(2^2) + Xu
		- 二次方让我们需要 n+2 bits。此外，我们再给 + Xu 时留出 1 bit 的空间。
		**- 总共，我们需要 n+2+1 = n+3 bits**。

这下就好办，我们既知道计算 X * 2^ 的方法，也知道两数相加的电路结构。

**解题**

- **X_u * 2^2**
	让数字向左平移两位，再往末尾塞上两个 0。

- **(X_u * 2^2) + Xu**
	让平移之后数字的后四位与 Xu 相加。

因为电路中有相加的性质，所以我们必须使用 Fa 来构建。Fa 的数量取决于 X 的位数。设 n=4，电路如下：

**表达 n 位数时只需要 n-1 个 Fa。因为最高位的 Fa 可以靠着超载的特性身兼两职。**

![4 - 4.10.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%204/%E5%9B%BE%E7%89%87/4%20-%204.10.21.png)