---
{"tags":["Cronograma","逻辑门","逻辑电路","IC","UPC"],"dg-publish":true,"permalink":"/Instrucció al computador/Tema 3/Tema 3/","dgPassFrontmatter":true,"created":"2024-09-18T00:06:08.912+02:00","updated":"2024-11-18T14:41:28.205+01:00"}
---


### 3.1 Introduccion

。
### 3.1.1 Introducción

###### Definición
**Circuito lógico combinacional (CLC)** 的输出取决与**当时的**输出，并且输入和输出是**固定的**，永远不变换的。
这种逻辑电路**没有记忆**，**瞬间反应**，与之前的结果无关。

###### Cronograma
这是一张图表，由三个部分组成：
- 最左边的图表示逻辑电路中**输入与输出口的名称**。
- 中间的图表表示**输入伏特-输出福特的对应关系**。
	*怎么看这张图表？*
	- 单张图表的信息：我们单拿 `x(t)` 段举例。图表中**纵向**表示 x 入口的**输入伏特**，**横向**表示**时间的进展**。我们看到，在第一段时间，x 入口输入为 0 伏特，第二段时间输入为 5 伏特，第三段时间，输入又变成了 0 伏特。
	- 所有图表联合的信息：因为这种逻辑电路的输入-输出是**瞬时**的，所以我们把三张图表纵向排列的话就可以获取它们输入-输出的**对照关系**：
		1. 第一时间段中，输入 x 为 0 伏特；输入 y 为 0 伏特，输出 w 同样为 0 伏特。**这就是一个输入-输出的对照关系**。
		2. 第二时间段中，输入 x 为 5 伏特，输入 y 为 0 伏特，输出 w 变成了 5 伏特。
		3. 略
		4. 我们发现，第四段与第二段一模一样，这展现了 circuito combinacional 没有记忆的特性。
- 最右边的表格用数字的方式集合了**全部的输入-输出对照关系**。

![Pasted image 20240918104333.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918104333.png)

###### Modelo matemático. Variables y funciones lógicas
人们会用各种模型来演绎现实生活中的电路。来让这些事情更加容易理解。

**给 circuito combinacional 设计的模型**
我们知道，**没有记忆**与**瞬时反应**是这种电路的特点。所以我们可以按照这两点为它定制一个更好理解的模型：
- 将 x(t) 这个随时间变化的变量换成不变的数值 x。
- 因为是二进制，所以我们不再讨论数学上的 0v / 5v，而是讨论二进制中的 有/没有（0/1）。这种二进制的变量被称为**variable lógica**。

因为电路的二进制输出取决于它的输入，**我们将输入-输出的对应关系称为 "función lógica"**。

我们还会制作一种展现**所有** función lógica 的表格，这种表格被称为 "tabla de verdad"。

下图展现真实电路转化成电路模型：

![Pasted image 20240918111653.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918111653.png)

*Comportamiento: 表现
Circuito lógico: 输入和输出的信号都是二进制的 circuito。*

###### Tabla de verdad
TV 中，每一个输入、输出都有它们对应的一列。此外，还有额外的一样用于输入名字；一列输入每一行的名字。

因为每一个输入有两种可能性，所以假如一个电路有 n 个输入，那么它的 TV 会有 2^n 行。

每一行的代称是哪一行中**二进制对应的自然数**（010 - 2行，000 - 0行）。就算那一行的本意不是表达的数字，但为了便捷，我们还是如此表达。


### De la descripción funcional a la tabla de verdad

有时，题目只会给我们一个 **descripción funcional**。这是一种**正经的，用文字/代数/代码短文来形容输入与输出**的用于形容逻辑电路的方式。这时，我们的目标就是通过这个形容来揪出它的 TV，并作出相应的逻辑表格（esquema lógico）。

###### Ejemplo 1 - Control de llenado de un depósito
展示了一个有**三个输入口**和一个输出口的逻辑电路，控制容器液体的填充。容器内的水会在 24 小时内被不断消耗。规则如下；
- x 是时钟：白天时为 1，晚上时为 0。
- y 与 z 是容器内的感应，y 位于上部，z 在下部：当它们被水盖住时为 1，不然为 0。
- 当输出 w 为 1，水被泵进容器；为 0 时无事发生。

目标：
- 早上时尽量不泵水 - 当 x = 1，w = 0；
- 晚上时尽量让水是满的 - 当 x = 0 ^ y = 0，w = 1；
- 满的时候不泵水（无论早晚）- 当 y = 1，w = 0；
- 没水的时候泵水（无论早晚）- 当 y & z = 0， w = 1。

![Pasted image 20240918131220.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918131220.png)

对于 (0, 1, 0) 或 (1, 1, 0) 这种异常情况，我们选择忽视。不管出口是多少。

出口处的 叉（×）代表结果可以是 0，也可以是 1。（也可以是小横杠（-）或者字母 "d"。

在一番对题目的分析后，我们发现选项 d) 的两行就是最合适的。

anómal@：（器械、情况）异常


###### Ejemplo 2 - Multiplexor
这次我们需要找一个 Multiplexor 的 TV。我们将这个电路代称为 Mx-2-1（Mx 是简称，此外有两个入口 x0、x1 与一个出口 w）。特殊的是，这个电路还有一个另外的入口 s。

入口 s 代表 “正在连接的那个入口”。当 s 为 0，x0 正在输入；当 s 为 1 时，x1 正在输入。

![Pasted image 20240918135044.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918135044.png)

这个连接方式用 c 语言表达的话（爷青回）如下：
```c
if (s == 0) {
	w = x0;
} else {
	w = x1;
}
```

我们可以按照这个编程思路（以 s 的数值为核心）来编辑 TV，所以我们把 s 放在第一列：

![Pasted image 20240918135621.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918135621.png)

TV 的排列方式没有定法，只要逻辑正确就是正确。但最好还是选出最好理解的排列方式（至少对于自己得好理解吧）。


### 3.1.3 Puertas Lógicas

Puertas lógicas 是最简单的逻辑电路，它**把 función lógica 物理实现**了。它们可以像砖头一样一点一点堆砌起来，实现更宏大的电路。

implementación：实现（某个事情）（三维世界的）。
funciones triviales：没有价值的 función。

###### Not - gi
用于颠倒输入信号，数学表达式为： **w = !x**
作图样式如下：

![Pasted image 20240918165725.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918165725.png)

###### And
当 x 与 y 输入都为 1 时，输出才为 1。

![Pasted image 20240918165753.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918165753.png)

###### Or
只要有输入为 1，输出就为 1。

![Pasted image 20240918165804.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918165804.png)

###### Xor
当输入**不同**时，输出为1

### 3.1.4 Conectando circuitos combinacionales entre si
接下来，我们将用这三个最基础的逻辑门构建一个简易计算机。因为在大量的构建中很容易出错，所以我们采用一种 ”diseño modular“ 的方式来构建。

###### Esquema lógico
**Esquema lógico** 是**一张用于表达逻辑电路的图表**。在这张图表的绘制中需要注意几点：
- **表达两条相接的线时一定要在它们交汇处画上点**。否则，这两条线视为相互独立。
- 下图右方的小东西用于概括整个逻辑电路。

![Pasted image 20240918171049.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918171049.png)

###### Reglas de interconexión de dispositivos para formar ciucuitos combinacionales
组建逻辑电路时存在一定的规矩，我们必须遵守！不然考试会扣我分。

**1. 不能将两条输出线路直接的相连至同一个点/同一条线。**
	同理，**也不能让两个输入相连至同一个点**。在现实的电器学上看，这么相连会导致短路，与整个电路的报废。
	.
	在模拟器中，如果我们这么做了，那么在两条线交汇的那里会显示 `C`。既不是 0 也不是 1。


**2. 任意逻辑门的入口和出口都必须有一个逻辑值（0/1）。**
	也就是说，逻辑门的入口一定要被以下三种之一连接着：
		- 电路的输入；
		- 其它逻辑门的输出；
		- 有恒定的能源供给（0/1）。
	.
	对于一些逻辑门入口 “飘在空气中（al aire）” 的情况来说，我们称其为 **"puerta lógica a alta impedancia¨**（高电阻）。

下图是三种禁止出现的情况。
	在模拟器中，C 代表意外交汇，Z 代表无输入，X 代表无法确定输出。

![Pasted image 20240918174102.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918174102.png)

**3. 不可以出现回头电路或死路。**
	所有电路都应能**畅通无阻**的**向前**走。



### 3.2 Análisis de circuitos combinacionales
在知道了构建电路的禁忌之后，现在我们来了解如何**看着图表，用文字描述电路**的方法。这个过程被称为 **Análisis**。


### 3.2.1 Análisis: creando la tabla de verdad por filas
虽然整个逻辑电路最重要的是整体输出和它的 TV，但在这之前，为了知道它们，计算全部的逻辑门并构建 TV 是必要的。

现在介绍的这种方法是让我们**将两个入口，四种可能性全部计算一遍**。这种方式比较繁琐，我们需要计算 32 次（每个门四次）才能得出 TV。

![Pasted image 20240918190208.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918190208.png)
上图最下方的图标是 Xor 逻辑门。

### 3.2.1 Análisis: creando la tabla de verdad por columnas
这种方法可能会好一些：

1. 参照电路整体输入，写出第一列与其标题。
	也就是 x、y 与它们对应的四种排列方式。

2. 给每一个逻辑门的输出命名，并放上标题。
	为了更加易懂，我们也应把输出的相应的 función logica 写在旁边。

3. 给那些和电路整体输入相关的逻辑门写出输出。
	结束后，我们得出第一阶逻辑门的输出。

4. 以此类推，给第二阶、第三阶…… 的逻辑门逐步计算，直到得出电路的整体输出。

![Pasted image 20240918191728.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918191728.png)

最后得出下面答案：

![Pasted image 20240918191751.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918191751.png)
*我们发现，虽然这个逻辑电路的组成与上个不一样，但是它们有着相同的 TV。这代表它们是 “ciucuitos equicalentes”，它们可以实现相同的事情。*


### 3.3 Síntesis de ciucuitos combinacionales

### 3.3.1 Definición
**Síntesis de un circuito combinacional**是指通过文字、TV 来画出逻辑电路的图示。

### 3.3.2 Caso sencillo de síntesis en suma de minterms （这个章节的讲述可能有误）
现在我们要构建一个电路，以让它达到 TV 显示的那样。

![Pasted image 20240918225751.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918225751.png)

一番思考过后，我们把这个 Síntsis 分为两步：

1. **作出 funciones minterm**。
	Función minterm：这种逻辑门只有唯一一种输入可以让输出为 1，其它都为 0。
	这种 función 有自己的一套命名方式：m0 - **只有第0行**的输出为 1；m1 - **只有第1行**的输出为 1，以此类推。

![Pasted image 20240918214009.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918214009.png)

我们觉得逻辑门 And 很适合这个主题。因为我们不想让中间两行有输出。同时，它是个天生的 función minterm，它有且仅有一个输出为 1。因为它的 FM 在第三行，所以它被称为 m3：m3(x,y) = x·y。

但如果我们想让它变成一个 m2，我们就需要做一些主动的改变。比如把输入 y 更改一下，变成 m2(x,y) = x·!y。就像下面这样：

![Pasted image 20240918214238.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918214238.png)

其它两个可能的 función minterm 也可以按照这种方式被做出……

2. **往上面安装一个 Or**
我们还记得，目标 TV 上 0行 与 3行 的位置是 1，现在我们得想办法让它发生。

在上一步把四种 minterms 做出来之后，答案就呼之欲出了：m0，m3 与 Or 逻辑门的组合就可以做到。

Or 的特性是只要输入有 1，输出就同样是 1。在 m1 与 m3 的叠加，与 Or 的笼罩下，中间两行永不见天日（和要求一样）；头末两行的输出也将永远光明。

最终的表达式：w = !x·!y + x·y。

![Pasted image 20240918231354.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918231354.png)

###### Ejemplo 5 - Puerta Xor-2
逻辑门 Xor 也是由两个 And 与 一个 Or 组成的。不过这边的 And 分别是 m1 与 m2。

![Pasted image 20240918232238.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918232238.png)


### 3.3.3 Algunas propiedades de la And y la Or. Puertas de *n* entradas

###### And 的两个特性
首先，我们来了解一下 **And 的两个特性**：

- **Conmutativa: x·y = y·x**
	这种特性只需要两个算式就可以证明：
	- 0·1 = 0；
	- 1·0 = 0；
	- 双零相乘和双一相乘就不需多言。

- **Asociativa: (x·y)·z = x· (y·z)**
	这种特性就需要一点证明了：
	
![Pasted image 20240918233046.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918233046.png)

上面两个特性组合，证明下列算式也是相等的：

![Pasted image 20240918233250.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918233250.png)
![Pasted image 20240918233301.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918233301.png)
**x·y·z = (x·y)·z**

由此拓展，我们发现一个规律。我们以 And-n 逻辑门举例：
**堆叠 And-n 逻辑门时，只有在它之前的输入全部为 1 时，总输出才为 1。**
换句话说：
**但凡途中有一个 And 逻辑门为 0，总输出就为 0。**

![Pasted image 20240918234409.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918234409.png)

###### Or 的两个特性

- **Conmutativa: x+y = y+x**

- **Asociativa: (x+y)+z = x+(y+z)**

堆叠的 Or 逻辑门也有差不多（相反？）的规律：
**堆叠 Or-n 逻辑门时，只有在它之前的输入全部为 0 时，总输出才为 0。**
换句话说：
**但凡途中有一个 Or 逻辑门为 1，总输出就为 1.**

![Pasted image 20240918234423.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918234423.png)

###### 逻辑门的堆叠方式
**多入口逻辑门的底层思路是双入口逻辑门的堆叠**。但是堆叠的方式也是有讲究的。它们分为**线性结构**与**树状结构**。两种结构都由上面的俩特性发展而来。

- **线性结构**的表达式是 ((a·b)·c)·d。
- **树状结构**的表达式是 (a·b)·(c·d)，这种结构**更常被使用**，因为它在**传递时消耗的时间更少**。

![Pasted image 20240918235527.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240918235527.png)
线性结构：estructura lineal
树状结构：estructura en arbol
###### Elementos Neutros
当我们需要三入口的逻辑门，但我们只能使用四入口的门时，很尴尬。但我们还可以利用 **elementos neutros** 的特性来强行消耗一个入口：

- Elemento neutro de **And es 1**: **x·1 = x**；
- Elemento neutro de **Or es 0: x+0 = x**。

这只要**把 And 看成乘法**；**Or 看成加法**就可以理解。图解的话如下方所示：

![Pasted image 20240919000142.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240919000142.png)


### 3.3.4 Síntesis en suma de minterms. Caso general

###### Las funciones minterm
假设我们在由 n 个逻辑门变量（Xn）组成的电路中找 minterm i。

举例：由四个逻辑门变量组成的电路中找 minterm 6。

- 第一步：将 6 转化为 4bits 的 2进制：0110。
- 第二步：分析 2 进制编码。得出结论，如果只想要第六行的输出为 1 的话，x3 位应为负，x2 位应为正，x1 位应为正，x0 位应为负。
- 第三步：写出逻辑表达式：m6 (x3, x2, x1, x0) = !x3 · x2 · x1 · !x0。
- 完事！

**Literal k** 是重要的。这个符号表示**在第 k 位的变量的两种可能性：Xk / !Xk**（k 位变量本身 / 第 k 位变量取反）。我们第二步就是在这两种可能性里取其一。

###### Implementación en suma de minterms
现在我们已经有组合逻辑电路的能力了。就是通过**叠加不同的 minterms（And 逻辑门）**，并在最后**使用 Or 逻辑门将 And 全部连接起来**。

比如下图的真值表：

![Pasted image 20240919103517.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240919103517.png)

我们发现输出列有 3 个 1，所以我们往逻辑电路里塞三个相应的 minterms，并在最后用 Or 组合起来。这个逻辑电路就大功告成。

它的表达式为：
w (x3, x2, x1, x0) = !x3*!x2\*x1\*!x0 + x3\*x2\*!x1\*!x0 + x3\*x2\*x1\*x0
w (x3, x2, x1, x0) = m2 + m12 + m15

**小细节**：
	- Variable（x）代表逻辑电路的输入。有多少 variable，逻辑电路就有多少个入口，也代表第一阶级的 And 门有多少入口。
	- 表达式中每一个 \* 部分代表一个 And 门；每一个 + 代表 Or 门的一个入口，三个加号代表 Or 有三个入口。

叠加：suma

###### 例子：Mx-2-1
还记得我们在 ejemplo 2 做过的 Mx-2-1吗？要是用这种方式解题的话，步骤会简单很多。

![Pasted image 20240919104642.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Pasted%20image%2020240919104642.png)

1. 有逻辑的列出真值表；
2. 揪出输出为 1 的几行；
3. 将那几行转化为 minterm 表达式；
4. 将 minterm 表达式翻译成 And 逻辑门；
5. 绘出相应图表，完事！没有任何计算！
6. w (s, x1, x2) = m1 + m3 + m6 + m7 = !s*!x1\*x0 + ······


### 3.3.5 Síntesis con decodificador y puertas Or
现在，让我们学学 Decodificador 的制作。

###### 解码器 - Decodificador
用于计算 entradas 可能组成的所有 minterms。

一般来说，设逻辑电路入口数量为 n，出口数量会是 2^[n]。所以 Decodificador 的命名公式为：**Dec-n-2^[n] 。**

表达式公式为：

![decod 表达公式.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/decod%20%E8%A1%A8%E8%BE%BE%E5%85%AC%E5%BC%8F.png)

它的制作方式并不难，以一个三入口的 Dec-3-8 举例：

![Dec-3-8.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Dec-3-8.png)

步骤依然是：

1. 有逻辑的列出输入变量排列可能（这次 a2 最高权重，排最左）；
2. 写出每个可能对应的 minterm（从左到右，从大到小）；
3. 推演出每个 minterm 对应的公式（ d3： d3(a2, a1, a0) = !a2\*a1\*a0 )；
4. 按照公式给每个and 绘图，组成逻辑电路（从上到下，从小到大）；
5. （不需要用 Or 套着）；
6. 出口。

###### El multiplexor con decodificador y puertas Or como ejemplo
现在，我们的目标是将我们刚刚学过的两个逻辑电路：Mx-2-1 与 Decodificador 结合起来，形成唯一出口。图例如下：

![Mx-2-1 + Decodificador.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Mx-2-1%20+%20Decodificador.png)

只需要把我们需要的那几个 minterm 揪出来，并连接在一个 Or 逻辑门的入口，就制作完成。

这种方法制作 Mx-2-1 要比 suma de minterms 更简单（真的假的），但是会造成一些硬件（逻辑门）的浪费，不过这对我们不重要。

###### Ej 10 - 基础双出口

如下图所示，这次我们需要制作一个双出口（w1，w2）逻辑电路。

这个例子比较简单，因为 w1 出口**只有一个输出为 1 的情况，所以我们不需要为 w1 安装一个 Or 逻辑门**。让它用 minterm 的线直出就可以。

![Ej 10 基础双出口.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/Ej%2010%20%E5%9F%BA%E7%A1%80%E5%8F%8C%E5%87%BA%E5%8F%A3.png)


### 3.3.6 Síntesis con una ROM
一般，在遇到**超过 4 或 5 个入口时**会使用 ROM，以防逻辑电路太过复杂和大个。

**ROM的组成**

一个简易的 ROM 由三部分组成：
- 一个 Decod.（用于产生所有可能的 minterms）；
- 三个连接点矩阵（决定输出信号样式/minterms 的使用）；
- 三个 Or-4-1 逻辑门（用于决定输出的位数）。

连接点矩阵中，空心小圈代表没连接；实心小点代表连接。

下图展示了这个简易 ROM 的三种画法。左边是 ROM 的符号标志；中间是 ROM 的结构组成；右边是工艺改进后的 ROM 结构，更好画。

![简易ROM.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/%E7%AE%80%E6%98%93ROM.png)


根据 TV 画 ROM 很简单，就是点豆豆而已。

1. 确定 **Dec. 入口与出口（minterms）的数量**。
2. 画出连接点矩阵，**矩阵的行/列数量 = Dec. 出口的数量**。
3. 根据整体出口数量**接上 Or 门**。
4. 看着 TV 的 Output 点豆豆，有 0 不点；有 1 要点。

另一种表达方式更加综合，如下：
它把 TV 的 Output 搬到了绘图上面。，也标出了最上面的 fila 与最末端的 fila。

![ROM 表达 1.png](/img/user/ZZZZ%20%E5%9B%BE%E7%89%87%E4%BB%93%E5%BA%93/ROM%20%E8%A1%A8%E8%BE%BE%201.png)


## // 还有三页没看


### 3.4 Completando el modelo de un circuito combinacional. Tiempos de propagación

接下来，我们将专注于了解从逻辑电路的入口到逻辑电路出口的 **tiempo de propagación**（英语：Propagation delay）。而不管电路占据的空间和功耗。

**信号从电路入口传递到电路出口是需要一定时间的，让这个时间变得尽可能短就是我们的目标！**


### 3.4.1 Análisis temporal de las puertas básicas
这个章节中，我们会看到 el **modelo temporal** de las tres puertas básicas。

###### Not

我们看到，**第一瞬间**时，入口为 0，出口为 1。当入口的信号从 0 切换成 1 的瞬间，出口并没有瞬间反应，而是**在经过了一段名为 TpHL 的时间后才切换到对应的出口**。

TpHL 的意思是 **Tiempo de propagació de Alto(High) a Bajo(Low)**。
- 一个 “High” 的值代表 1；一个 “Low” 的值代表 0。

与其相对的，也存在 TpLH，也就是由 “Low” 转到 “High“。

**它们的时间长度相等， TpHL = TpLH**。也因此，在接下来的教学中我们将二者统称为 Tp。

![Tp - Not.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%203/%E5%9B%BE%E7%89%87/Tp%20-%20Not.png)

###### And-2

在双入口的逻辑门中，区分的东西会多一些。我们看到：

- 当 x=0，y=0，w=0。
- 当 x=0 但 y=1，依然 w=0。
- 当 x=1 且 y=1，**经过 Tp_y-w 后，变成 w=1**。
- 当 x=1 且 y=0，**经过 Tp_x-w 后，重回 w=0。

在之后的教学中，如果**入口的数值改变不导致出口的数值改变，我们当作 Tp = 0s**。

图表中也有些新东西：

- 我们会使用箭头，**从改变的起始时间与起因，指向改变的结束时间与结果**。以展示得更加清晰。


![Tp - And-2.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%203/%E5%9B%BE%E7%89%87/Tp%20-%20And-2.png)

###### Or-2

基础规则同 And-2，无特殊规则。

此外，对于 And-2 与 Or-2，我们有了新的看法：

- 逻辑层面上来看，这两扇门入口处的 “x” 与 “y” 是不重要的。因为将它们的 xy 对调不影响输出的结果，（x=1, y=0）\==（x=0, y=1）。
- 时间层面上来看，为了教学的便捷，我们假设 Tpxw = Tpyw。所以我们将它们用 Tp 统称。

![Tp - Or-2.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%203/%E5%9B%BE%E7%89%87/Tp%20-%20Or-2.png)


###### Tiempo de propagación de las puertas básicas
（融合在上一小章中了）

###### Tiempos en la librería de dispositicos Digit@Lib
在接下来的教学中，我们假设基础三门的 Tp 分别如下。**u.t. 是为了方便取的一个抽象的时间单位。我们不讨论 ns, ms 之类的。**

- **Tp(NOT) = 10 u.t.
- **Tp(And-2) = 20 u.t.
- **Tp(Or-2) = 20 u.t.

最后，**Cronograma 表达的不是全面的可能性，而是那些重要的，需要表达的可能性**。它并不像 TV 那样严谨。


### 3.4.2 Análisis temporal de un circuito combinacional
逻辑门 Tp 进化！逻辑电路 Tp！

###### Caso simple: circuitos con un solo camino de cada entradaa a cada salida

当计算逻辑电路时，Tpxw 很可能就不等于 Tpyw 了。就和下图一样。

<u>整个逻辑电路的Cronograma 该怎么画呢？</u>

1. 列出所有逻辑门的时间表。顺序如下：
	- 最上方的是整个电路的入口；
	- 中间的是电路中的逻辑门。按照处理的先后顺序，分别从上到下排列；
	- 最下方的是整个电路的出口。

2. 从最上方的时间表一步一步的向下绘制，中途标出 Tp。**在最后的最后，等到逻辑门的 Tp 都被揭晓，才能开始画电路出口的时间表**。

所以，只要会画单个逻辑门的时间表，就可以拼凑出整个电路的时间表！但是……

<u>单个逻辑门的 Cronograma 该怎么画呢？</u>

1. 获得那个逻辑门的 TV，并**零时差的初步画出时间表**：

2. 获得那个逻辑门的 Tp（一般来说是 10 与 20），**将出口的时间表向右平移 Tp 距离**并**标注**：
	Tp 向右平移会导致每个表格出现时间缺口。这些缺口时的值是未知的，所以我们一般会**往这些缺口上填充阴影**（如下图的蓝色实心格子）。

<u>整个逻辑电路的 Cronograma 也可以通过上面两步得出。</u>

 ==总的来说：

1. 列出两个入口的时间线，再根据 TV 画出相应方波。
2. 看看下一个受影响的逻辑门 “A” 会变化几次，根据 Tp **提前预留出阴影面积**，最后画出该逻辑门。
3. 标注**箭头**与**Tp**。
4. 看看下一个受影响的逻辑门 “B” 会变化几次，根据 Tp 提前预留出**更多的**阴影面积，最后画出该逻辑门。
5. 标注**新的箭头**与**新的Tp**。
6. 债来一次……
7. 债来…………...
8. 直到画出最后的逻辑门为止，**加上 Tpxw 与 Tpyw**。

**逻辑电路的 Cronograma**最重要的还是 **Tpxw, Tpyw 与其箭头**。其它的甚至可以为了图表的干净而忽略掉。

==公式：

Tp = 路径上所有逻辑门的延迟的**和**。

在逻辑电路中，Tpxw 极大可能不等于 Tpyw。
	但讨论单个逻辑门时，这俩相等。

![真 3.2.4.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%203/%E5%9B%BE%E7%89%87/%E7%9C%9F%203.2.4.png)

###### Circuitos con más de un camino entre una entrada y una salida

下图是一个逻辑电路。A1、A2 分别是两个 And 门，图像上标注着它们的 Tp（20 u.t.）。右边是电路的 TV。

<u> Caso 1: (1, 0, 0) → (1, 1, 0) </u>

**情况分析**
在这个变化中，x 与 z 保持不变，只有 y 从 0 变 1。我们**查看 TV，发现电路的结果由 1 变 0**。

**解题**
这种只有一个入口变化的情况，我们**只画正在变化的入口的 Cronograma 就可以**，不需要画 x 与 z 的。

1. 这个变化影响到的第一个逻辑门是 !y，所以我们从它画起：
	1.1断定**该门的结果会因 y 的变化而变化**。
	1.2  察觉到 !y 的 Tp 是 10，我们**预留出一段时间**；
	1.3 在此之上，画出这个门中**信号的变化**（由 1（y 变化前的结果） 到 0（y 变化后的结果）；
	1.4 使用**箭头**标注出 Tp 与变化的瞬间。

2. 接下来影响到的逻辑门是 x·!y 与 y·z，我们从前者开始：
	2.1 断定**该门的结果会因 y 的变化而变化**。
	2.2 察觉到 A1 的 Tp 是 10，我们在前者的预留上再**叠加 20 的预留**；
	2.3 画出这个门中信号的变化 （1 && 1 \== 1 （y 变前）→ 1 && 0 == 0（y 变后）)；
	2.4 使用**箭头**标注出 Tp 与变化的瞬间。

3. 后者 y·z：
	3.1 断定该门的结果**不会**因 y 的变化而变化。
	3.2 预留出 20 u.t. （不经过 NOT）
	3.3 没有变化，一条直线
	3.4 没有变化，无需标注

4. 总出口 w：
	4.1 断定该门的结果会因 y 的变化而变化；
	4.2 察觉 Tp，预留；
	4.3 画出信号的变化；
	4.4 箭头标注；
	**4.5 总结所有的 Tp。**

5. 式子：T = Tp(NOT) + Tp(A1) + Tp(Or-2) = 10 + 20 + 20 = 50 u.t.

![3 - 3.49.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%203/%E5%9B%BE%E7%89%87/3%20-%203.49.png)


<u> Caso 2: (0, 1, 1) → (0, 0, 1) </u>

**情况分析**
这道题的情况与上题很像，都是 x 与 z 不变，只有 y 变。我们通过 TV 发现，这个**逻辑电路结果由 1 变 0**。

**解题**

1. 先画 !y：
	1.1 断定会导致结果变化
	1.2 预留
	1.3 画图
	1.4 标注（但因为**此路不通，不导致出口的变化，会在最后被忽略**）

2. 再画 x·!y：
	2.1 断定不会导致结果变化
	2.2 前者的基础上预留
	2.3 无变化
	2.4 无变化，不标注

3. 再画 y·z：
	3.1 有变化
	3.2 预留
	3.3 画出变化
	3.4 标注

4. 最后画出口 w：
	4.1 有变化
	4.2 前者基础上预留
	4.3 画出变化
	4.4 标注
	4.5 总结延迟 u.t.

5. 式子：T = Tp(A2) + Tp(Or-2) = 40 u.t.

![3 - 3.51.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%203/%E5%9B%BE%E7%89%87/3%20-%203.51.png)


<u> Caso 3: (1, 1, 1) → (1, 0, 1) </u>

**情况分析**
依然是 x 与 z 不变，只有 y 变的情况。但是，这次**总出口 w 的结果不变**。

**解题**

1. 从 !y 开始：略
2. x·!y：略

3. y·x
	1.1 
	1.2 
	1.3
	1.4 标出延迟，并用箭头标注出变化的瞬间。在这个情况，**因为该逻辑门的上家是电路入口，所以箭头得从第一张图拉到下面，即使它们差的很远。

4. w
	因为这个例子中存在两条通路，而它们的**抵达时间存在差异**，这可能导致结果存在波动（但总结果还是恒定的）。
		- T_camino1 = Tp(NOT) + Tp(A1) + Tp(A2) + Tp(Or-2) = 10 + 20 + 20 = 50 u.t.
		- T_camino2 = Tp(A1) + Tp(A2) + Tp(Or-2) = 20 + 20 = 40 u.t.
	
	可见路2要比路1更快抵达，同时路2会导致电路输出由 1 变 0；而随后赶来的路 1 又会让电路由 0 变 1。这导致输出电路的方波图形成了一个快速的小变化。这种**在期望之外的小变化**被称为 ***Glitch***。
	
	最终，逻辑门的输出恒定在 1。

5. 式子：
	因为这个例子的电路**存在两条通路**，但只有一条路是**决定性的**。这种 ***Camino crítico***一般都是**最后到达的那条通路**，这条道路的结果就是**TV上的值**。

![3 - 3.52.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%203/%E5%9B%BE%E7%89%87/3%20-%203.52.png)


<u> Caso 4 / Ejemplo 12 </u>

即使逻辑门的数值不改变，**在同时改变多个信号的时，Glitch 也有可能发生**（0, 0）→（1, 1）。

一个例子就是下方的逻辑门。这个逻辑门在两种输入下的最终结果都是 0。但当信号切换的时候，发生了 Glitch。因为**两条路上逻辑门的数量不对等，容易导致 Glitch 的发生**。

![Pasted image 20241005173748.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%203/%E5%9B%BE%E7%89%87/Pasted%20image%2020241005173748.png)


### 3.4.3 Tiempos de propagación de un circuito combinacional
为什么知道逻辑电路的 Tp 是一件那么重要的事呢？
##### Tiempos de propagación de un circuito combinacional
随着电路复杂性的提高，它同时存在的通路也会越来越多，Glitch 的可能与散布的范围也会越来越大。知道 Tp 就是为了知道，我们需要**等待多久**才可以在出口**很多的 Glitch 中采集到那个正确的值**。

这个 “采集” 的动作是通过构成一个 **circuito secuencial síncrono (同步时序电路**) 做到的。它会发出**señal reloj（时钟信号，Clk）** 来决定 “观察（mirar）” 动作的周期。

- 每次信号 **从 0 变 1** 的瞬间，方波第一次抬起的瞬间，代表一次采集的时刻。
- Clk 的作用有二：**发出 “观察” 指令** 与 **“储存” 观察到的值**。（不太确定是不是这样）
- 观察的周期被称为 **tiempo del ciclo**。是从第一次由 0 变 1 的 **flanco（边沿）** 到第二次由 0 变 1 的边沿之间的距离 / 时间差。

![3 - 3.54.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%203/%E5%9B%BE%E7%89%87/3%20-%203.54.png)

电路的 tiempo del ciclo (tc) 取决于电路的 Tp。

- 如果 Tp 大，tc 也大。计算机处理的效率会**很慢**。
- 如果 Tp 小，tc 也小。计算机处理的效率会**更快**。但如果 Tp 与 tc 太小，电脑有可能在上一次 Glitch 还没平复之前就进行观察，**这会导致错误的观察**。

所以，我们的目标是：
**让 Tp 与 tc 在合理的范围内尽量的小，但不能小得太极端，不然会导致错误的值。**






































