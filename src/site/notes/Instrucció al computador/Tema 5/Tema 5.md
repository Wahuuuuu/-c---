---
{"tags":["IC","Num_Narutal","ADD","SUB","Comparador","SL","SRA","组合电路","UPC"],"dg-publish":true,"permalink":"/Instrucció al computador/Tema 5/Tema 5/","dgPassFrontmatter":true,"created":"2024-10-12T15:09:15.971+02:00","updated":"2024-11-18T14:41:38.411+01:00"}
---



### 5.1 Un repaso a la representación y suma de números naturales

**Sistema de numeración binario**
在计算机中，**自然数** 被以 **二进制** 的方式表达。

![5 - EQ1.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ1.png)

![5 - EQ2.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ2.png)

其中，脚缀的意义如下：

- **n-1 ... 0**
	代表 bit 所在的那一位的编号。一个四位数的最大 n 是 3，因为最小的编号是 0。
- **i** 
	代表 {n-1 n-2 ... 1 0} 的集合。EQ1 中意为所有的 bits 要么是0要么是1。
- **u**
	代表该数是，一个以**自然数**（unsigned integer) 方式表达的二进制向量。
	计算方式是 “X_i * (2^编号)” ——或者—— (那一位的值) \* (2^(编号))。

==自然数 / unsigned integers==：包括零的正整数。

**Rango**
自然数的 rango 代表包含它所有可能的值的范围。
一个 “能被使用二进制表达的**自然数**” 的范围如下：

![5 - EQ3.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ3.png)

**Sumador binario**
如果 Xu+Yu+C0 的和依然只有 n 位：cn=0；
如果前者的和需要进位，有 n+1 位：cn=1。代表该数字**无法在只有 n bits 的 ADD 模块中被表达**。

![5 - EQ4 & EQ5.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ4%20&%20EQ5.png)
![5 - EQ6.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ6.png)

EQ6 同时表达 EQ5 和 EQ6 的公式，俺看不懂。EQ8 和 EQ9 俺也看不懂。看看重不重要吧，看情况回来补。

![5 - EQ7.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ7.png)
![5 - EQ8 & EQ9.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ8%20&%20EQ9.png)

**La suma como recorrido circular de la tabla de representación**

![5 - 5.1.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.1.01.png)

当我们往 111 加上 001 时：
- c3 会变成 1，表示 “结果无法表达”；
- 同时 X2X1X0 会重回 000。

这个加上 001 的过程就像是一个循环一样，从 TV 的最终兜圈循环回到 TV 的最初。这是本 Tema 的核心之一，我们将其称为 ==la suma como recorrido circular==。

### 5.2 En busca de una representación eficiente para los números enteros
我们开始学习电脑储存 ==Entero / signed integers / 整数== 的方式了，它们代表 **包含零的正整数与负整数**。与 Números naturales / unsigned integers / 自然数 作区分。

它们依然是被以 **二进制向量** 的方式表达（遵循 EQ1）。

##### 5.2.1 Signo y magnitud en base 2

在我们熟悉的十进制中，一个数字被分为两个部分：==Signo y magnitud / sign and magnitude / 正负与大小==。

在二进制中，设 Xn 表达了整数 Xsm（signo y magnitud）。前者的**首位**（n-1 位）**表达它的 signo / 正负**；**首位以外的位表达它的 magnitud / 大小**。：

- 如果其首位是 0，代表 Xn 是正数（011 = +3)；
- 如果其首位是 1，代表 Xn 是负数（111 = -3）。

此外，由此推理，Xsm 的 **rango 就是大于等于正负 2^(n-1) - 1**。

![5 - 5.2.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.2.11.png)

二进制**加法 / 减法的规矩**和十进制差不多：

- 若两个加数的 signo 一致，二者之和的 signo 也一致。
	(+34) + (+12) = +46 y también (-34) + (-12) = -46

- 若两个加数的 signo 不同，加法中 **较大 magnitud 的数应在前，较小的在后**。二者之和的 signo 与较大 magnitud 的数一致。
	(+34) + (-12) = +24 y también (-34) + (+12) = -24

所以：

- 当两个 signo 一致时，计算它们需要一个 n-1 位的 ADD 模块（**与 natural 运算的代价一样**）；
- 当两个 signo 不同时，计算它们需要一个 n-1 位的 Comparador + 一个 SUB 模块（**比 natural**
- **运算的代价高很多很多**）。

得出结论： 包含 signo y magnitud 的运算有很高的代价，自然数运算会简单很多。所以一般来说计算机不使用 sm 方式表达数字。

##### 5.2.2 Otras representaciones más efectivas
硕以，节俭的人们为了节省代价，选择换一种表达方式，以让 enteros 可以使用与 naturales 一样的计算器。

还记得我们在本文初提到过的 “加法循环论” 吗？往 111 加上 001 时，结果会变成 000。如果……比如……我们**把 111 定义成 -1** 的话，事情就会有所改变？！
	由此，加上两个 001 得出 000 的 110 被定义为 -2，101 被定义为 -3，100 = -4……

需要注意，上面那些只是我们个人对特定二进制向量的定义，并非公理。所以，定义的方式是灵活的。比如下面的三个方式，我们可以选择最适合应用场景的那个来定义：

![5 - 5.2.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.2.21.png)

这三者的区别在于它们的 rango：
a) –3 ≤ Xa ≤ 4 ,
b) –5 ≤ Xb ≤ 2 , 
c) –4 ≤ Xc ≤ 3 .

需要注意的是，**cn 口的意义也变化了**。它不再代表 “进位” 或 “不可表达，而是其它意义。

**新的脚缀**：Xs 代表对 signed integers / 整数 的表达。


##### 5.2.3 Complemento a dos
现金的计算机一般使用和选项 c 相似的设定。它被称为 **Complemento a dos - Ca2**。有三个特点：

- 它的 rango 比较平均，但是**负数会比正数多一位**，因为 0 也占一位。
- 负数的**首位**一定为 1，正数的首位一定为 0（”0“ 也是正数）。
- 更容易分辨这个数是否无法表达（现在还看不懂，之后细说）。

和两个优点：

- 使用的加法模块与 natural 一致（除了不可表达的数字）。这让计算机**使用一种模块就可以进行两种数字的运算**。
- 这种加法模块比计算 signo y magnitud **代价要低**很多（更少的硬件与 Tp）。

**总结：Ca2 可以与普通二进制向量共用计算器，但是它们的意义是不同的。比如 110 在 Ca2 中代表 -2，但在普通二进制中代表 6。


### 5.3 Representación de enteros en complemento a dos
深究 enteros 的表达方式。
##### 5.3.1 Fórmula del valor de un número en función del valor de los dígitos que lo representan
让我们寻找将二进制向量的 n 个 bits 翻译成整数 Xs 的公式。正整数与负整数有不同的计算方法：

**正整数**
正整数的计算方法与自然数几乎一样。非要深究的话，我们可以把正整数的首位排除运算（必定为零）：
![5 - 5.3.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.3.11.png)

**负整数**
负整数的计算方法就需要一些理解了。

负数的首位参与计算，但是**它的 signo 为负**。之后再**让首位加上剩余的位数**，后者的计算方法与自然数一致。

证明：
	101 = -(2^(3-1)) + ( **0**\*(2^(3-2)) + **1**\*(2^(3-3)) ) = -4 + (2+1) = -1。正确！

![5 - 5.3.12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.3.12.png)

**最最最终公式**

![5 - EQ10.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ10.png)

这个公式糅合了前两个公式：
- 当首位为 0 时，第一部分不参与运算；
- 当首位为 1 时，第一部分参与运算。

##### 5.3.2 Rango

正负数的域接近相等，但是**负数比正数多一**。也就是说，**最大负数的绝对值 > 最大正数的绝对值**。

![5 - EQ11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ11.png)

##### 5.3.3 Extensión de rango

设 n 位的 X 是表达 Xs 的二进制向量，该如何得到 n+1 位的二进制向量，且保持它的值与 X 相同？

正数和负数的拓展方式是一样的：
1. 将首位复制到拓展出来的 bits 上。拓展几位就原封不动地复制几次；
2. 首位之外的 bits 同位继承。这个步骤不需要逻辑门，靠电线就可以做到：

例：101 = 111 101（前三个 1 是复制产物）；011 = 000 011（前三个 0 是复制产物。

![5 - 5.3.31.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.3.31.png)

##### 5.3.4 Cambio de representación para números enteros

###### <b style="color: #5DD0C8;">Ca2 到 decimal</b>
用之前讲解过的 EQ10 就可以。
![5 - EQ10.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ10.png)

###### <b style="color: #5DD0C8;">decimal 到 Ca2</b>
有两种方式：

**Procedimiento de las divisiones por dos**

- **正整数**
	1. 将 decimal 不断除以二，得出一个商与一个余数（0 或 1）。
		将此步的余数记作 Ca2 时第 0 位。
	2. 将上一步得出的商继续不断除以二，又得出一个商与一个余数。
		将余数记作 Ca2 的第 1 位。
	3. 继续将上一步的商不断除二，得出商与余数。
		记作第 2 位。
	
	...
	
	如此往复直到最后的余数（必定整除，余数为 0）。

- **负整数**
	负整数也是循环往复，但是**因为我们这辈子没接触过带余数的负数除法**，我们需要了解一些底层规则。我们需要**余数必定是正数**：
	
	![5 - 5.3.41.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.3.41.png)
	
	就和上图的第一个竖式一样：-5 / 2 = -3 **...1**。
	
	但是**最后一次除法的余数可以是负数**。这可以在二进制种证明它是负数。


**Procedimiento de las divisiones por dos**

我们将 EQ2 与 EQ10 糅合成一个公式，意为：
	”(自然数) = (整数) + (向量首位 * n bit 向量可表达的最大值）“：

![5 - EQ12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ12.png)

EQ12 可延申成 EQ13，意为：
	若整数为**正：自然数 = 整数**。
	若整数为**负：自然数 = 整数 + n bit 向量可表达的最大值**。

![5 - EQ13.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ13.png)

- 例 1：给出 Xs = -54，我们需要找到它在 Ca2 中的表达。
	
	1. 分析 X 的位数
		通过背板，我们知道 7bits 足矣表达 -54（2^7 = 128）。
		-64 ≤ -54 ≤ (64-1)
	
	2. 通过 EQ13 列出公式
		Xu = -54 + 2^7 = 74
	
	3. 反复除以 Xu，得出它的二进制表达
		最后，将 74 不断除以二，得出 74 的二进制表达是 1001010。这同样也是 -54 的 Ca2 表达。结束！
	
	4. 查看是否有补码需求
		如果有的话，比如题目要求我们写出 8 bit 的向量，我们就按照之前学过的补码方式得出结果：
			1001010 = 11001010


### 5.4 Suma con detección de resultado no representable
Ca2 的优点是能与二进制的正整数共用计算器，但是缺点在于不好检测 ”不可表达的值“。
##### 5.4.1 Cambio de representación para números enteros
在接下来的例子中，我们假设 n=3（-4 ≤ Xs ≤ 3）。

###### <b style="color: #5DD0C8;">Operandos con distinto signo</b>
一正一负的整数相加**不可能得出无法表达的值**。因为这俩之和的绝对值，一定比他们本身的绝对值，更接近 0。
	abs ( Xs, Ys ) ≥ abs ( Ws ) ≥ 0

###### <b style="color: #5DD0C8;">Operandos con el mismo signo</b>

- **Suma de positivos**
	通过 ADD 模块相加两个正整数时，答案 W 有两种可能：
	
	1. 0 至 ( (2^n-1) -1 )之间，**首位为 0** 的数。（若 n=3，代表 0 - 3 之间）：
		两正整数相加**得出正数，是正确的**。
	
	2. -( (2^n-1) -1 ) 至 -1 之间，**首位为 1 的数**。（若 n=3，代表 -4 - -1 之间）：
		两正整数相加**得出负数，是错误的**。
		n bits 的 Ca2 语法无法表达出答案，因为它的答案在 +4 - +7 之间，而Ca2 中 100（4）与 111（7）代表负数。

- *题外：答案的位数可能超过模块上限吗？*
	答案是不可能。
	相加时，最坏的可能是当两个加数都是 3 ( (2^n-1)-1 )，且进位 cin=1。
		3+3+1 = 7 (111)。它依然是 3 bits，就是答案错误了而已。

- **Suma de negativos**
	相加两个负数时，答案也有两种可能：
	
	1. -4 至 -2 间，**首位为 1 的数**：
		答案是**正确**的，无需赘述
	
	2. 0 至 3 间，**首位为 0 的数**：
		答案是**错误**的。
		n bits 的 Ca2 语法无法表达出答案，因为二者之和在 -8 - -5 之间。

- **Conclusión**
	同性相加，若结果的性质与加数**一致，正确**；若**性质相反，错误**，代表它们的和超过了 n bits 的 Ca2 语法可表达的范围。

EQ14 与 EQ15 是Conclu 的逻辑表达式。

- EQ14 公式解释：
	- 变量解释：
		- vn 
			代表溢出检测 / 无法表达检测。若 vn=1，存在溢出；若 vn=0，不存在溢出。
		
		- x_n-1, y_n-1, w_n-1 
			代表 xyw 的首位。表示其 signo。
		
		- 头上的小杠
			表示 NOT。1 变 0，0 变 1。
		
		- "·"
			中点代表 AND。
		
		- "+"
			加号代表 OR。
	
	- 逻辑解释：
		若 “X 首位” 与 “Y 首位” 性质相同，但二者与 ”W 首位“ 的性质相反，存在溢出。vn=1。
![5 - EQ14.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ14.png)
- EQ15 公式解释：
	当 vn=1 时，代表存在 OVF / overflow。
![5 - EQ15.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ15.png)

**Ejemplo 4**
让我们证明 EQ16 是对的。它用于检测 Ca2 的 “不可表达” 的结果。

EQ16 意为 “若 cn 与 cn-1 的值**相同，不存在 ovf**；若**不同，存在 ovf**。
	注，“^" 符号代表 Xor 逻辑。00=0，01=1，10=1，11=0。

![5 - EQ16.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ16.png)

下一步，列出 TV，画出最基础的 Fa 图表。该 TV 排列了 xyc 的所有可能，然后按照以下规则：

- 推理 wn-1：
	**xy 同同，w 同；xy 异同，w=x**（因为第一个被加数的 magnitud 更大）。
- 推理 cn：
	如果 xyc 中存在两个及以上的 1 时，cn=1。

- 前面二者完成之后之后，推理 vn：
	按照 EQ14 或 EQ16 的规则。

我们发现，vn 那一栏既可以通过 EQ14 表达，也可以通过 EQ16 表达。证明成功！
![5 - 5.4.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.4.11.png)

**Ejemplo 5**：寻找 Ca2 语法下，当 ”无法表达“ 时，Ws 的表达式。

- 过程
	通过观察下面由错误答案组成的表格，我们可以轻易发现华点（在 n=3 的前提下）：
		- 当答案理应是正数，但得出负数时，Ws = Xs + Ys -8；
		- 当答案理应是负数，但得出正数时，Ws = Xs + Ys +8
	
	![5 - 5.4.12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.4.12.png)

- 公式 EQ17：
	当结果 ”无法表达“ 时，正确结果的表达式应该视情况决定：
		当错误结果**大于正数 rango** 时，Ws = Xs+Ys+c0 **-** (2^n)
		当错误结果**小于负数 rango** 时，Ws = Xs+Ys+c0 **+** (2^n)

![5 - EQ17.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ17.png)

##### 5.4.2 Sumador binario para naturales y enteros con detección de resultado no representable

下图的 ADD 模块可以同时作用于 naturales 与 enteros。

- **cn 口只在 naturales 的情况下有意义**。若其 =1，代表 ”无法表达“。
- **vn 口只在 enteros 的情况下有意义**。若其 =1，代表 ”无法表达“。

![5 - 5.4.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.4.21.png)

- 该加法模块的公式 EQ18 如下。**展开以查看公式的解释**：
	
	在 unsigned integer 的情况下：
		如果 X+Y+c **小于等于最大域**，W = X+Y+c。**且 cn=0**；
		如果 X+Y+c **大于最大域，无法表达**，W = X+Y+c - (2^n)。**且 cn=1**。
	
	在 signed integer 的情况下：
		如果 X+Y+c **处于等于最大域之内**，W = X+Y+c。**且 vn=0**；
		如果 X+Y+c **大于最大域**，W = X+Y+c - (2^n)。**且 vn=1**；
		如果 X+Y+c **小于最小域**，W = X+Y+c + (2^n)。**且 vn=1**。

![5 - EQ18.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ18.png)

三个模块的 **公式化表达** 如下：

![5 - 5.4.22.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.4.22.png)

- **Ejemplo 6：搭建一个计算 n 入口，n+1 出口的计算器，并解释为什么这个计算器不可能 OVF。
	
	不可能 OVF：
		因为两个 n 位向量相加的最大值是 (2^n+1)-2，加上进位之后也可能只是 (2^n+1)-1。
	
	搭建 n+1 出口的两种方式：
		1. 暴力的使用 n+1 个 Fa，直截了当达成目标。
		2. 只使用 n 个 Fa，并用逻辑间接搭建出第 n+1 位出口。
	
	第二种方式的核心在于 EQ22：
	
	n 位的结果 = 处于 n-1 位的三个入口的 Xor 运算。
	![5 - EQ22.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ22.png)
	
	当使用的 ADD 模块存在 v 出口时，可以使用下面的公式。更简单！
	![5 - EQ23.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ23.png)


### 5.5 Cambio de signo
寻找 “取反” 用的电路。目标：Ws = -Xs
##### 5.5.1 Algoritmo aritmético de cambio de signo
我们通过下图的实例展示来寻找规律：

- Xs 列（半透明箭头）
	Xs 列的对称以 -4 / 100 为轴线，展开。
	- 规律：
		1 的值位于 -4 往上爬 3 位；-1 的值位于 -4 往下降 3 位。

- X2X1X0 列（虚线箭头与细箭头）
	这一列的对称以正负数的分隔为轴线，展开。
	规律：
	轴线两端的**字符**互为反数，110 ↔ 001。但是值会差 1。

所以，只要两个步骤就可以：
1. 将字符串取反
2. 使用 ADD 模块对其加一 / 减一

![5 - 5.5.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.5.11.png)

==Ej7 与 Ej8 很迷惑，先不记


##### 5.5.2 Detección de resultado no representable
在 ”取反器“ 中，只有一种情况可以发生 overflow，就是**当被取反的数 = 最小域的极致**的时候。所以，公式如下：

![5 - EQ24.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ24.png)

当 xn-1 位**与** wn-1 位为一时，无法表达。


##### 5.5.3 Implementación de un negador
- Negador 的初级电路结构如下（可以不看）：
	
	- 它向 最高进位 与 最高结果 连接了一个 AND 门，作第 n 位结果用。
	- Fa 的 c 口变为中间相互连接的形态。

![5 - 5.5.31.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.5.31.png)


**Ejemplo 9**

存在第二种检测 vn 的方式（除 EQ24 以外）。如下：

![5 - EQ25.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%20EQ25.png)

**Ejemplo 10**

然后，因为只需要使用一个入口，所以可以把 Fa 平替为 Ha。
再加上 EQ25，整体变为下图：

![5 - 5.5.32.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.5.32.png)

**Ejemplo 11**
？

**Ejemplo 12**
例题：找到 10110111 对应的整数。

方法1：使用 EQ10。
	Xs = -128 + 32 + 16 + 4 + 2 +1 = -73

方法2：先翻译成二进制再颠倒过去
	1. 颠倒 10110111
		得出 01001000
	2. 因为是负转正，所以需要加上 00000001
		得出 10111000
	3. 转化为整数
		64 + 8 + 1 = 73
	4. 转化正负
		得出 -73


### 5.6 Resta
我们可以通过刚刚学的知识稍微改改 negador，变成 restador。

目标是**输入两个正数** XY，然后**让 X 减去 Y**。
使用一种 W = X + (-Y) 的思路。

我们在 negador 的基础上：
- 将入口改为双出口。左边是 X，右边是 Y。
- 将 c0 入口 **默认为 1**（因为我们把 Y 从正转负，这个过程必定需要加一；）。

初级减法器：W = ADDn（X，!Y，1）成立！

![5 - 5.6.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.6.01.png)


### 5.7 Implementación de un sumador/restador
因为 sumador 和 restador 很相似，所以我们可以整个 sumador/restador。模式切换依靠切换 !s/r 来完成。

- 用于切换模式的电路的组建：
	
	1. 新建 "!s/r" 入口。连接到三个地方：c0、y入口 与 c3 出口。
	2. 将 y 入口的 NOT 门去掉，转而用 Xor-2 门代替。
	
	这下，当 !s/r **为 1 时，减法模式**；当 !s/r 为 **0 时，加法模式**。
	
	c3 在运算 natural 时作 ”进位“ 用。但运算 enteros 时，c3 固定输出为 0，无用。
	v3 用于运算 enteros 时检测 overflow。

![5 - 5.7.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.7.01.png)


### 5.8 Multiplicación por potencias de dos
先看 \*2 的计算方式再看 \*2^(n 次方) 的计算方式。

##### 5.8.1 Algoritmo aritmético de multiplicación por 2
思路和 [[Instrucció al computador/Tema 4/Tema 4#4.4.1 Algoritmo aritmético de multiplicación por 2 en binario\|Tema 4#4.4.1 Algoritmo aritmético de multiplicación por 2 en binario]] 中的一模一样。

总之乘二的方式就是将 X 左移一位，并在末位插入 0 。

**检测无法表达**
当输入的 n-1 位和 n-2 位**不同**时，无法表达。

##### 5.8.2 Algoritmo aritmético de multiplicación por 2^k y su implementación
思路和 [[Instrucció al computador/Tema 4/Tema 4#4.4.2 Algoritmo aritmético de multiplicación por potencias de 2\|Tema 4#4.4.2 Algoritmo aritmético de multiplicación por potencias de 2]] 中的一模一样。

总之乘以 2 的 k 次方的方式就是将 X 左移 k 位，并在末尾插入 k 个 0 。

**它与 naturales 乘法器的区别只有一点，对无法表达的检测**：
- naturales 时，只有当开头的 k 位全部相同时才可以表达（全是 0 或全是 1）；
- enteros 时，只有**开头的 k+1 位全部相同时才可以表达**（更加苛刻）。

此外，名称一模一样（Shift Left - n），结构也几乎一模一样。

![5 - 5.8.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.8.21.png)


### 5.9 División por potencias de dos
先看 /2 的计算方式再看 /2^(n 次方) 的计算方式。

##### 5.9.1 Algoritmo aritmético de división por 2
方法就是将 X 所有的位数**右移一格，并在首位复制符号数（负数复制 1，正数复制 0）**。同时，X 的末位被抹除。
	0111 → 0011；1001 → 1100

##### 5.9.2 Algoritmo aritmético de división por 2^k y su implementación
方法就是将 X 所有的位数右移 k 位，并在首位复制 k 次符号数。同时，X 末尾的 k 位被抹除。

###### <b style="color: #5DD0C8;">Implementación del divisor por 2^k (SRA-k)</b>
除以 2 的 k 次方的电路被称为 **Shift Right Arithmetically - k (SRA-k)**。

它与 naturales 除法器的区别也**只有一点**，它的出口不再固定为 0，而是**在出口的首 k 位复制它的符号数**。

![5 - 5.9.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.9.21.png)


### 5.10 Comparación de números enteros
和 naturales 的比较器一样，我们先让 X-Y。如果 X-Y < 0，X 比 Y 小。

在这次的比较器中，存在一个 v 口，用于检测结果是否溢出：

- 当 v=0，表示答案正确。我们需要检查 W 的首位。
	- 若 Wn-1 = 0，表示 X-Y = **正数**。X 比 Y **大**。
	- 若 Wn-1 = 1，表示 X-Y = **负数**。X 比 Y **小**。

但是当  v=1，表示答案错误。这时，可能有两种情况：
1. **X_S - Y_S** 实际上是一个负数，但由于溢出而显示为正数。
2. **X_S - Y_S** 实际上是一个正数，但由于溢出而显示为负数。

- 所以，当 v=1，此时的规律**与 v=0 时相反**：
	- 如果符号位为 1（表示负数），那么**结果实际是正数**。
	- 如果符号位为 0（表示正数），那么**结果实际是负数**。

- 将一切糅合成一个公式：**!v\*s + v\*!s** ——或者说 XOR (v, s)：**v ^ s**。
	- 其中，v 代表溢出检测口，s 代表 w 的符号位（首位）。
	- 解析：
		- 当没有溢出（**v = 0**）时，直接看符号位 **s** 是否为 1（即结果是否为负数）。
		- 当发生溢出（**v = 1**）时，符号位的含义相反，因此检查 **!s**

有两种 Comparadores，LT 与 LE：

- LT（Less Than）用于检测 X < Y：
	使用上方的公式 **v ^ s**。

- LE（Less or Equal），用于检测 X ≤ Y：
	将公式拓展为 **v ^ s + z** 。

两个 comparador 我们都没有到 borrow 口。因为……就是用不到啊。

![5 - 5.10.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%205/%E5%9B%BE%E7%89%87/5%20-%205.10.01.png)












