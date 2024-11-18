---
{"tags":["IC"],"dg-publish":true,"permalink":"/Instrucció al computador/Tema 8/Tema 8/","dgPassFrontmatter":true,"created":"2024-10-27T15:17:21.818+01:00","updated":"2024-11-18T14:41:52.333+01:00"}
---

### 8.1 Introducción
我们也要设计一种 PPE，但 UP 变成 UPG（Unidad de Proceso General）。它由两个模块组成：
- ALU（Arithmetic-Logic Unit），用于集成所有的运算单元
- REGFILE（Register File），用于集成所有的 REG
目的是在一个循环内从 REGFILE 中读取两个 REG，并加以运算。

### 8.2 Unidad aritmético-lógica: ALU
XY 进入 ALU 后，会被三个运算模块运算，产生相应的三个结果。最后由 OP 选择，输出三种结果之一。
- 如果执行的是 comparaciones，那么 W 会输出 0000000000000000 或 0000000000000001。判断的结果要参照 W0 位，并保证 W15 - W1 全是 0。

#####  <b style="color: #5DD0C8;">出入口</b>
- **Buses de control**：用于变换需要执行的任务。
	- OP（2 bits），用于决定激活哪个模块；
	- F（3 bits），用于决定在激活的模块内执行哪种运算：
		- 如果是 op. aritmética / lógica，共有八种选择（两种加减乘除）；
		- comparaciones 有三种选择（LT，LEU，EQ）；
		- op. miscelánes 有两种选择。

重申，所有三个模块中，对应 F 的运算都在进行。但是只从 W 输出被 OP 选中的那个。

- **Buses de datos**
	- X，Y（16 bits）
- **Bus de salida**
	- W（16 bits）
	- z（1 bit），用于检测 W 的结果。如果全是 0，z=1；否则，z=0。可以说，这是 ALU 的逻辑判断检测口。

#####  <b style="color: #5DD0C8;">内部模块</b>
- AL：当 OP=00 时，执行 “operaciones **A**ritméticas o **L**ógicas / 逻辑或运算任务”。
- CMP：当 OP=01 时，执行 “**C**o**MP**araciones / 对比”。
- MISC：当 OP=10 时，执行 “其它任务 / 杂项任务 / operaciones **MISC**elánas“。
- z：检测输出是否全 0 。

![8 - 8.2.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.2.01.png)
![8 - 8.2.02.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.2.02.png)


### 8.3 Banco de registros - REGFILE

由 8 个 16bits 的 REG 组成，被称为 R0 ... R7。用于从 REG 中选出两个进行输出，并在时钟信号的上升沿时从 D 返回运算的结果并储存（前提是 WrD=1，允许读入）。

#####  <b style="color: #5DD0C8;">出入口</b>

- **Buses de salida / lectura**
	- A，B
- **Buses de entrada / escritura**
	- D
	- @A, @B, @D（3 bits）
		- @A 与 @B 选择 “从哪个 REG 读取到 A 与 B“；
		- @D 选择 ”将输入写入到哪个 REG 中“。同时，也得看 WrD 的脸色。
		这通过 MUX 选择连接的线来决定。
- **Buses de control**
	- WrD：如果 =1，允许在时钟上升沿时，从 D 读入值到 @D 对应的 REG；否则，不读入。

![8 - 8.3.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.3.01.png)
![8 - 8.3.02.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.3.02.png)


### 8.4 Unidad de Proceso General (UPG)
##### 8.4.1 Estructura de la UPG
UPG 由 ALU 与 REGFILE 组成，二者形成一个**循环体 / bucle**。
目的是在一个循环内完成数据输出，计算，再读入。
因为 UPG 是 16bits 的，所以所有关于数据的 buses、REG 都是 16bits 的。

**接口**
- RD-IN（read imput）用于从 UPG 外输入值进来。
	此外，会有一个 MUX，用于选择读入的是 “外部的值” 还是 “ALU 计算后的值” 。

- WT-OUT（write output）用于从 总线A 向外面输出值。

位于 ALU 的 Y口 上方的 MUX 用于选择读入的是 “外部的值” 还是 “来自REGFILE的值”。

![8 - 8.4.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.4.11.png)

##### 8.4.2 Conexionado entre la UPG y la unidad de control

**UC 的接口**：由于接受处理结果，并以此作出下一个抉择
- Control-In
- Control_Out
- **Bit de condición，共 1bit：**
	- z，用于向 UC 展示，ALU 的结果是否为 0。

**UPG 的接口**
- RD-IN
- WR-OUT
- **Palabras de control，共 33bits：**
	- @A, @B, @D - 每个 3bits
	- OP - 2bits，F - 3bits
	- N - 16bits
		通过 MUX 选择向 ALU 输入 B 还是 N
	- WrD - 1bit
		是否读入
	- In/ALU，Rb/N - 每个 1bit
		从外部读入/从ALU读入；从 总线B 读入/从 总线N 读入。

![8 - 8.4.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.4.21.png)

我们通常使用以下排序来输出哪个 33bits 的 palabras de control（实际上排序不重要，但是习惯这么排）。
	其中 16bits 的 总线N 被简化为四位 16进制 的值，用大写X代表。

![8 - 8.4.22.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.4.22.png)


### 8.5 Acciones en la UPG
我们来看看 UPG 的能力。

![8 - 8.5.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.5.01.png)


1. **Aritmético-lógicas y de comparación con dos operandos.**
	两个操作数的算术运算或逻辑运算。
	
	一共有十二种：
	- 2 种算术运算：ADD，SUB
	- 1 种算术移位：SHA
	- 1 种逻辑移位：SHL
	- 3 种 bit a bit 的逻辑运算：AND，OR，XOR
	- 2 种 enteros 对比：CMPLT，CMPLE
	- 2 种 naturals 对比：CMPLTU/Comparison on Less or Equal for Unsigned，CMPLEU
	- 1 种 igualdad 对比：CMPEQ
	
	==举些例子==：
	- 向 REG6 写入 “REG3 与 REG5 的和”，表示如下：
		ADD R6，R3，R5
	- 如果 REG5 要大于或等于 REG1，则在 REG3 中写入 1；不然，写入 0。
		因为不存在 “大于或等于”，只有 ”小于或等于“。所以我们调换 R1 与 R5 的顺序，并使用 ”小于等于“ 模块。
		CMPLEU R3，R1，R5
	
	当第二个操作数是 UC 生成的数 N，而不是 REGFILE 中的数时，标记会发生一些改变：
	1. 在模块名称之后，加上 mnemotécnico/助记符 “I”，表示 Immediate/即时；
	2. 更改 第二个操作数N 的表达方法：
		- 如果 N 是十进制数，它将被直接用数字表达：
			ADDI  R7，R1，-1
			将 REG1 加上 -1，最后写入 REG7。
		- 如果 N 是十六进制数，他将由 “0x” 开头，并接上四位十六进制：
			ANDI  R2，R3，0xFF00
			将 R3 与 FF00 作 bit a bit 的 AND 运算，最后写入 REG2。

2. **Aritmético-lógicas de un operando.**
	单个操作数的算术运算或逻辑运算
	
	只有一种选项：NOT，用于将 bus A 的值取反。
		NOT  R4，R2
		将 REG2 中的值 bit a bit 取反，最后写入 REG4。
	因为取的是 A 口的值，所以不可能用到助记符 “I”。

3. **De movimiento**
	将值移动到指定寄存器内
	
	有两种可能：
	- 从一个 REG 移动到另一个 REG：
		MOVE  R1，R5
		将 R5 的值复制到 R1
	
	- 将 N 值移动到一个 REG
		MOVEI R3，0xFA02
		将 FA02 复制到 REG3

4. **De entrada de datos**
	读入当前输入口的值到某个 REG
	
	就一种可能：
		IN  R2
		读入当前输入口的值到某个 REG2。

5. **De salida de datos**
	将某个 REG 中的值输出到 WR-OUT
	
	也就一种可能：
		OUT  R4
		将 R4 的内容输出。

##### 8.5.1 Palabra de control para cada acción

###### <b style="color: #5DD0C8;">Valor de cada bit de la palabra de control para un ejemplo de acción</b>
在运算时，UC 指令的意思。以 NOT  R4, R2 为例。

1. 从 busA 读取 R2 中的值：
	UC 需要在 @A 中编写 2 以读取到 R2。

2. 让 ALU 执行 NOT，并输出相应的模块：
	将 OP 编为 00；将 F 编为 011。

3. 让 UPG 顶端的 MUX 选择 “从 ALU 读入”
	将 In/Alu 编为 0。

4. 下令 “允许读入”：
	将 WrD 编写为 1。

5. 将值精确读入进 R4：
	将 @D 编写为 4 / 100

6. 随意处置 @B，Rb/N 与 N，因为我们根本用不到 busB：
	随意处置时，我们填入 x，用于表达 “既可以是0也可以是1 无所谓谁会爱上谁”。

![8 - 8.5.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.5.11.png)

###### <b style="color: #5DD0C8;">Valores de los bits de la palabra de control para los ejemplos tratados</b>
在进行 MOVE 时，存在两种操作的方法：
1. 将 入口X 与 出口W 直连（从 总线A 读取）
	OP=10，F=000
2. 将 入口Y 与 出口W 直连（从 总线B 读取）
	OP=10，F=001
	同时也需要 Rb/N=1

###### <b style="color: #5DD0C8;">Acciones que no modifican los registros</b>
当这轮运算的目的 **不包含写入 REG 时**
我们往 “目标寄存器” 一格填入 “-” 。
- 如果 z=1，代表 R3 的首位=0。
	ANDI  -，R3，0x8000
	此时 WrD=0，@D=x，In/Alu=x。参见下面表格倒数第三行。

###### <b style="color: #5DD0C8;">Acción NOP.</b>
当这轮的目的 **也不包含运算时**
这种情况下，我们会使用 NOP（No OPeration）。比如，当电路需要在 循环k 时读入一个数，循环k+1 时停止，循环k+2 时读入第二个数并计算时，执行如下：
- k：IN  R2
- k+1：NOP

###### <b style="color: #5DD0C8;">Varias acciones en paralelo</b>
当这轮的目的是 **同时执行两个任务时**
不是全部任务都可以同时执行，但也确实存在这种情况。我们使用 **//** 分隔开两个任务，用于表示它们在同一循环内执行。
- 比如 IN R2 与 OUT R4。
	IN  R2 // OUT  R4
- 也比如下面这个，用于将 R3R4 的差写入 R1 的同时，输出 R3。
	SUB R1，R3，R4 // OUT R3

但是 ADD R6，R3，R5 // IN R2 这种是不行的，因为一个循环内只可能输入一个值。

![8 - 8.5.12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.5.12.png)


### 8.6 Del código en lenguaje C al grafo de estados de la UC (sin entrada/salida)

##### 8.6.1 Un ejemplo de PPE para calcular el MCD (sin entrada/salida de datos)
这章节旨在将 R0 与 R1 的值做最大公约数 MCD，在计算结束后填回 R0 。
- MCD 概念回忆：
	求24和60的最大公约数，先分解质因数，得24=2×2×2×3，60=2×2×3×5，24与60的全部公有的质因数是2、2、3，它们的积是2×2×3=12，所以，（24，60）=12。
	
	R0=24  R1=60
	24  60
	24  36   -   R1=60-24
	24  12   -   R1=36-24
	12   X    -   R0=24-12，两个数相等，MCD 是12，填入 R0

```c
while (R0 != R1) {
	if (R0>R1) R0 = R0-R1;
	else R1 = R1-R0;
}
```

为此，我们写出了上面的 c 语法，之后画出了下面的图示：

![8 - 8.6.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.6.11.png)

我们通过对箭头的设置完成了 while 的作用。此外，我们给每个逻辑判断编写了 nodo，并给每个相应的算术执行也编写了 nodo。


### 8.7 Entrada/salida síncrona
我们将深入探讨 UC 与 UPG 的联合。并且，为了将 UC 与 UPG 的时序同步，UC 会使用 Begin 信号 与 End 信号。

所以，UC 在每一轮需要生产：
- 33bits 的 palabras de control
- salidas de control End
- Q+，取决与 Begin 与 z。

##### 8.7.1 SUMA-4 con la UPG
相对于使用 UP 的 PPE，使用 UPG 的在流程上会有些改变：
	- 不可能从 RD-IN 直连 ALU，必须经过 REGFILE
	- 不可能在同一循环内既从 RD-IN 写入 REGFILE，再从 RD-IN 写入新的值进 REGFILE。

- 回顾 UP 的 PPE 流程：
	1. RD-IN 取到第一个值。将其存在 REG 中。
	2. 读入的第二个值不经过 REG，直接去 ADD 与第一个值相加。循环结束时，存入 REG。
	3. 第三个值炮制第二个值的操作。
	4. 第四个值炮制前面的操作。
	5. 输出。
	共五个循环。

- 使用 UPG 的 PPE 流程：
	1. 写入四个值（四次 IN）//  End=0
	2. 相加三次（三次 ADD）//  End=0
	3. 输出一次  //  End=1
- 共八个循环，结束后等待新的值到达。

此外，在第一次 Begin=1 之后与第一次 End=1 之前，电路需要忽略 Begin 的值。
但是一旦 End=1，同一循环内，Begin 也可以=1 了。

- 总结：
	一般来说，使用 UP 的 PPE **效率更高**。但是使用 UPG 的 PPE **更加泛用**，只需更改 UC 即可执行多种任务。

![8 - 8.7.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.7.11.png)
![8 - 8.7.12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.7.12.png)

##### 8.7.2 SUMA-n con la UPG
- 当 Begin=1 时，RD-IN 读入 “n“。我们需要求接下来输入 n 个循环的进入的值的总和。
- 在求和结束后，从 WR-OUT 输出结果，并 End=1。
- 此外，在 Begin=1 与 End=1 之间忽视 Begin 的值，但在 End=1 时重新开始关注。

使用 UP 的 PPE 中，我们可以采用 ”加法器求和的**同时**，在减法器将 n-1 来了解还有几个求和循环“ 来实现上面的功能。但是在使用 UPG 的 PPE 中，我们不能做到这点。
- 但是，UPG 既不能做到同时储存两个结果，也不能做到同时做出两个运算（只能依靠 OP 输出三选一）。


![8 - 8.7.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.7.21.png)

End=1 的条件如下：
- 在 Begin=1 的**同个循环**读入 n 。假设这是第 c 个 循环。
	==nodo0 需要做的事情，就是在 Begin=1 时需要做的事情。这个例子中是 “读入”=
- 在 第 c+1 个循环，出现第一个需要相加的值。
- 在 第 c+3 个循环，出现第二个需要相加的值（前提是 n>1）
- 之后，每三个循环，出现一个需要相加的值（c+6, c+9, c+12, ...）
- 直到 n-1=0，End=1。
End=1 之后，检测 Begin。

##### 8.7.3 MCD con la UPG
使用 UP 时，计算 MCD 需要从两根输入总线同时读取两个值，但是 UPG 做不到这点。让我们更改一下流程：
- 当 Begin=1，**下个循环**的 RD-IN 会读入第一个值，再下一个循环读入第二个值。
	==因为 Begin=1 的循环不需要做事，所以 nodo0 时做 “NOP”==
- End 需要在 WR-OUT 前一个循环时就 =1。
- 在流程中忽略 Begin……

**注意 Leyenda 的格式如下：**
![8 - 8.7.31.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.7.31.png)
![8 - 8.7.32.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.7.32.png)

这里，因为题目要求 “需要 End=1 的下一个循环才输出”。所以我们在得出结果后使用 NOP // End=1 来消耗掉一个循环。 并在其之上根据 Begin 作条件指令。
- 此外，我们注意到，图例中并没有真的在 NOP // End=1 后增加一个节点作输出，而是将 nodo0 的指令改成了输出。
- 其实，增加一个 nodo8 专门用于输出也是可以的，但是会增加电路的复杂性，而且也没有优化性能。所以，我们选择将原本 nodo0 的 NOP 替换成 OUT R0，来简化电路。


总结：
Es importante resaltar que en el proceso que nos lleva, paso a paso, de un diseño específico para resolver cada problema a un único procesador de propósito general, que nos servirá para resolver cualquier problema, ganamos en generalidad y en tiempo de diseño a costa de perder en tiempo de ejecución, en tiempo de resolución de cada problema concreto. Con la UPG, la resolución de un problema requerirá, en general, más ciclos que con una unidad de proceso de propósito específico, como las diseñadas en el capítulo anterior, en las que puede haber mucho hardware disponible para ser usado en paralelo, en un mismo ciclo. También es muy probable que el tiempo de ciclo del procesador de propósito general sea mayor que el que podríamos obtener con una implementación de propósito específico para un problema.
必须强调的是，在我们一步步从解决每个问题的特定设计到可用于解决任何问题的单个 通用处理器的过程中，我们获得了通用性和设计时间，但代价是失去了执行时间，即解决 每个特定问题的时间。一般来说，使用 UPG 解决一个问题所需的周期要比使用特定用途处理单元（如上一章所设计的处理单元）所需的周期长，因为在同一周期内，可能有许多硬件可以并行使用。此外，通用处理器的周期时间也很有可能比我们使用特定问题实施方案的周期时间更长。


### 8.7 更多电路范例

![8 - 8.7.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.7.01.png)
![8 - 8.7.02.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.7.02.png)
![8 - 8.7.03.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.7.03.png)
![8 - 8.7.04.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%208/%E5%9B%BE%E7%89%87/8%20-%208.7.04.png)




















