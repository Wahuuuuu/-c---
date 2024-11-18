---
{"tags":["IC","PPE","Camino_Crítico","UPC"],"dg-publish":true,"permalink":"/Instrucció al computador/Tema 7/Tema 7/","dgPassFrontmatter":true,"created":"2024-10-26T11:03:28.116+02:00","updated":"2024-11-18T14:41:48.250+01:00"}
---


### 7.1 Introducción
这个章节中，我们将学到用于处理 n bits 字词的处理器。它们被称为 procesadores de propósito específico (PPE)。

我们将用下题来了解 PPE：
"Diseñar un circuito lógico secuencial que realice la suma módulo 2^8 de una secuencia de 4 números naturales codificados en binario con 8 bits cada uno"
"设计一个逻辑时序电路，对 4 个二进制编码的自然数序列进行 2^8 模求和，每个自然数 8 bits。

我们将要使用的模块如下 (a)：
1. 自然数从入口 DATO（8bits）进入电路，每次循环进入一个自然数。
	 入口 Ini = 1，代表循环开始。
2. 算数结束时，结果从出口 RESULT 口输出
	出口 Fin = 1，代表答案输出。

我们在电路中间接需要解决的问题大致有：
- 会输入什么，什么时候输入？
- 如何处理输入？
- 会输出什么，什么时候输出？

![Instrucció al computador/Tema 7/图片/7 - 7.1.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%207/%E5%9B%BE%E7%89%87/7%20-%207.1.01.png)

**关于 “会输入什么”**
我们可以把电路的入口出口分为两种：
1. Entradas / Salidas de datos：
	- 输入/输出的是值。
2. Entradas / Salidas de control：比如 Ini, Fin
	 - Ini=1 代表现在从 DATO 输入的是自然数数组中的第一个；
	 - Fin=1 代表电路已完成运算，且输出已经准备好了。

为了逻辑的清晰，我们会用**更高级些的形容**来预先描述我们的电路：
- MÓDULO：**命名**电路
- ENTRADAS/SALIDAS：
	将所有的出口、入口区分为 **de datos / de control**，并表明**名字与 bits 数量**。
- FUNCIONAMIENTO:
	使用 **代数语法**/表达式 来表达电路的目的。
	下面例子中的目的如下：
		- 如果 Ini(ciclo1)=1，那么：
			- 在 ciclo2 的 RESULT 出口输出的值 = 从 c1 时钟周期起的 4 个连续 DATO 输入之和，并进行 mod(2^8) 运算。
			- 之后，Fin(c2)=1。
		- 0 <= DATO <= 255((2^8)-1)，DATO 被以 8 位的二进制编码表达。
- FIN MÓDULO

![7 - 7.1.02.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%207/%E5%9B%BE%E7%89%87/7%20-%207.1.02.png)


### 7.2 Diseño de Procesadores de Propósito Específico con Unidad de Proceso (UP) y Unidd de Control (UC)
电路设计从一个比较书面的描述开始，此外设计者还需要注意 Tc、使用的逻辑门与模块、从输入到稳定输出的循环数量，等等等等。

一个 PPE 由两部分组成：
1. Unidad de Proceso / UP / camino de datos / data path / 数据通路
	它储存并处理数据，直到获得结果。
	与 entradas de datos 直连。
	- 这个例子中，UP 在每个循环都会输出一个 palabra de condición，以领导 UP 的执行。

2. Unidad de Control / UC / 控制单元
	它控制 UP 中执行的操作并确保排序正确。
	与 entradas de control 直连。
	- 这个例子中，UC 在每个循环都会输出一个 palabra de control，以领导 UP 的执行。

3. Palabra de control y palabla de condición
	是 UP 与 UC 交流的道路。
	- UP 在每个循环都会输出一个 palabra de condición，以告诉 UC 这次循环内发生了什么。
	- UC 在每个循环都会输出一个 palabra de control，以领导 UP 的执行。

![Instrucció al computador/Tema 7/图片/7 - 7.2.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%207/%E5%9B%BE%E7%89%87/7%20-%207.2.01.png)

##### 7.2.1 Unidad de Proceso
我们不使用刚学的 “先画图示，再做 TV，再……” 的方式设计 UP；而仅仅将 elementos de almacenamiento, combinacionales 和 逻辑门相互结合。
- 这种设计方式被称为 “**diseño ad-hoc**"。

思路如下：
1. 输入第一个数，循环结束时保存至 REG。
2. 输入第二个数，在 SUM 模块中与第一个数相加。循环结束时保存至 REG。
3. 输入第三个数，在 SUM 模块中与第一第二个数之和相加。循环结束时保存至 REG。
4. 输入第四个数，在 SUM 模块中与第一第二第三个数之和相加。循环结束时保存至 REG。

![7 - 7.2.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%207/%E5%9B%BE%E7%89%87/7%20-%207.2.11.png)
- 其中 multiplexor 的 Mx = palabra de control 。
	- 当 Mx=0，REG 入口的是 DATO。
		此时应该是 Ini=1，UC 还在待机中的时候。
		此时 REG 确实不需要记录 SUM 过后的值。
	- 当 Mx=1，REG 入口的是 (新的DATO) + (REG中的值)。
		此时 Ini=0，至少是第二循环（UC已启动）。
		此时 REG 确实需要记录 SUM 过后的值。

##### 7.2.2 Unidad de Control
UC 的设计难度比 UP 简单一些，因为它输入/输出的位数要少于 UP。我们使用先作图示的设计法。

也就是说，UC 会是一个 modelo Moore。
- Palabra de Control(Mx) 与 salidas de control(Fin) 都仅依赖 estado actual(Q)，不依赖电路入口。
- Estado siguiente(Q+) 需要依赖 palabra de condición、entrada de control(Ini) 与 estado actual(Q)。

题外：
- 在这个例子中，UP 的 REG 需要在每个周期都加载一次。这在这个例子中是正确的，但在其它的例子中，REG 中的值可能需要保持好几个循环不动。这时，UC 需要输出一个被称为 “Load” 的信号，以指示 REG 需要更新的循环。

![7 - 7.2.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%207/%E5%9B%BE%E7%89%87/7%20-%207.2.21.png)

我们注意到下图中 Q=Resultado disp. 时，如果 Ini 再次等于 1，会继续运算。
![7 - 7.2.22.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%207/%E5%9B%BE%E7%89%87/7%20-%207.2.22.png)


### 7.3 Tiempos de ciclo y de ejecución
假设这个电路被包含在一个更大的组合内：电路的入口是上个电路的出口；电路的出口也连接着下一个电路。全部的电路都在同个时钟电路之下。

设定 Tc 的方法还是找到 camino crítico。需要额外注意的点是，PPE 的 camino crítico 可能存在于 UP-UC 的路径上：
- 可能从 UC 的 ROM 到同在 UC 的 biestable
- 可能从 UP 的 REG 到另一个 UP 的 REG
- 可能从 UP 的 REG 到 UC 的 biestable
也：
- 可能从某个入口到某个 biestable
- 可能从某个 biestable 到出口

**概念：tiempo de ejecución - Te**
是执行整个 módulo 任务所需的时间。
	Te = Nc * Tc
	执行时间 = 执行次数 * 循环时间

我们的目标是 **找到 Te 最短的那个设计方式**。

##### ANEXO A: Ejemplo, el cálculo del Máximo Común Divisor
题外：MCD 模块的计算。

这次的目的是计算两个二进制编码，十六 bits 整数的 máximo común divisor。我们将使用欧几里得算法。

```
MÓDULO "MCD"

	ENTRADAS/SALIDAS 
	Entradas
		de datos: X, Y         16 bits
		de control: Ini        1 bit
	Salidas 
		de datos: MCD          16 bits
		de control: Fin        1 bit
	
	FUNCIONAMIENTO
	si Ini(c1) = 1 entonces 
		RESULT(c2) = MCD (X,Y)
		Fin(c2) = 1
	
	donde X(c1), Y(c1) y MCD(c2) son números enteros con 16 bits codificados en 
	complemento a 2.

FIN MÓDULO
```

###### <b style="color: #5DD0C8;">欧几里得算法</b>

```
Algoritmo de Euclides para MCD(X,Y) 
	
	A:= X; B:= Y;                      
	mientras (A <> B) hacer                  -- 当 A不等于B 时
		si (A > B) entonces A:= A-B;            -- 若 A>B, A-B
		sino                B:= B-A;            -- 若 B>A, B-A
	
	fmientras                                -- 直到 A=B 为止
	MCD:=A;
```

欧几里得算法在于有条件的使 A B 两值相减，直到它们相等。

**UP设计**：
- 需要两个 REG 用于储存 X 与 Y 的值
- 需要一个 restador 来减法
- 需要一个 comparador 来对比大小
- 需要一些 multiplexor 来选择路径
	- 我们需要两个 multiplexor 在 “储存入口的值” 和 “储存 restador 的结果的值” 之间做选择。
	- 需要另两个 multiplexor，根据 A<>B 做相应的指令。
![Instrucció al computador/Tema 7/图片/7 - 7.3.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%207/%E5%9B%BE%E7%89%87/7%20-%207.3.01.png)

**UC 设计**
UC 模块的入口是 entrada de control(Ini)，与 UP 传来的 A B 的大小关系。
输出如下：
- 让 multiplexores 作出正确选择（Mx1 - 4）
- 让 REG 作出正确选择（Load / LdA, LdB）
- 输出 Fin 信号

![Instrucció al computador/Tema 7/图片/7 - 7.3.02.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%207/%E5%9B%BE%E7%89%87/7%20-%207.3.02.png)

搞清 UC 的输入输出之后，可以设计 UC 的 Moore 模型了：
0. 一切平静，UC 待机
1. 有新的输入，Ini=1
2. UC 启动，根据 UP 传来的信号指挥 UP。
	...
3. 待到相等，输出 FIN=1 信号。等待新的 Ini=1。
4. 如果输出 FIN=1 后一个循环没有输入，FIN=0

因为这种情况下，UC 需要输出一堆信号。所以我们列了个表，**用 S1 - S6 来指代每个输出组合**。

![7 - 7.3.03.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%207/%E5%9B%BE%E7%89%87/7%20-%207.3.03.png)
- Mx1 / Mx2：
	- 若为 0，从入口录入；
	- 若为 1，从 RESTADOR 的结果录入
- Mx3 / Mx4：
	- 若为 0，A-B。这时 Mx1=1，LdA=1；
	- 若为 1，B-A。这时 Mx2=1，LdB=1。

**联动思路解析：**
- 如果输入的两个数相等，语法结束，输出 FIN=1。等待新的输入。
- 如果 A>B，输出 Mx3=Mx4=0：
	- 让 A-B，Load REGA，以更新 A 的值。
- 如果 A<B，输出 Mx3=Mx4=1：
	- 让 B-A，Load REGB，以更新 B 的值。

**优化思路** 
不写了来不及了 可以去看 PDF

##### ENUNCIADOS DE PROBLEMAS - 刷题
一般来说，UP 与 UC 的设计思路都是相似的：
- UP 通过 elementos combinacionales 的组合
- UC 通过画出图示

此外，题目可能不说，但我==每一题都必须找到 camino crítico 并算出 Tc== 。

























