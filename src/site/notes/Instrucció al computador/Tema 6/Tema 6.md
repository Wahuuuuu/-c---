---
{"tags":["时序电路","Biestable","Cronograma","IC","Camino_Crítico","UPC"],"dg-publish":true,"permalink":"/Instrucció al computador/Tema 6/Tema 6/","dgPassFrontmatter":true,"created":"2024-10-16T19:00:34.324+02:00","updated":"2024-11-18T14:41:43.189+01:00"}
---



电路分为 combinacionales 与 secuenciales 两种。现在我们将开始学习后者：secuenciales。
- 本章将专注于讲解 circuito secuenciales 的 la síntesis。
- ==circuitos secuenciales / 时序逻辑电路==

Circuitos secuenciales 拥有 memorizar información 的能力。所以，这种电路的输出不仅仅依赖它的输入，也依赖**当前的** circuito 的 **estado / 状态**。

### 6.1 Introducción
1. **Circuitos secuenciales** 的特点在于 **memoria y sincronización**。其中：
- **el biestable D / 触发器 :** 最基础的储存物品，储存 1bit 的信息；
- **el registro / 寄存器:** 储存 n bits 的信息。

2. 连接 dispositivos combinacionales 和 biestables 的规则。用于组成更复杂的电路。

3. 在 tiempo de ciclo / 时序 期间，电路的变化。

##### 6.1.1 Necesidad de memoria
Circuitos secuenciales 拥有 memorizar información 的能力。这个能力也造就了它的特点：这种电路的输出不仅仅依赖它当时的输入，也依赖电路储存之前的输入导致的**当前的** circuito 的 **estado / 状态**。

接下来我们将初步的设计一个飞机着陆操作系统来帮我们更好的理解时序电路（具体设计参见章节6.5）。
###### <b style="color: #5DD0C8;">Ejemplo de un piloto automático para el aterrizaje de aviones</b>
例子：飞机着陆的自动控制系统

目标：
- 使飞机位置位于跑道的延长线

为了达到这一点，跑道上布置有两排天线，发射带有指向性的信号。左天线发射 Fi，右天线发射 Fd。两边天线之间的一段特定距离可以同时接收到两种信号。

飞机上的信号接收器 I 与 D 会接受跑道上发来的信号。当成功接收时，I 发射信号 si=1，接受不到时发射 si=0；接收器 D 这边同理。

所以，我们需要设计一个有以下逻辑的电路：
- 入口：si 与 sd ，出口：w1 w0 。
- 出口应与领导飞机行进的指令对应，需要根据出口的信号决定飞机应该 左拐/右拐/不动（参见下方 TV）

![6 - 6.1.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.1.11.png)

但飞行常常伴随着意外。如果着陆时有强风吹来，将飞机吹到任何信号都检测不到的距离，我们又该怎么办呢？

- 如果电路是 circ. combinacional，我们什么都做不到。只能让两个信号同时消失时将控制权交给飞行员，希望他能做出合理的判断。
- 但如果电路是有记忆功能的 circ. secuencial，我们可以让电路观察：在两个信号同时消失前的信号是什么
	- 如果最后发出的信号是 01，提示飞机向右，我们就可以断定飞机处于左边的无信号区域。只要让飞机大力向右，它就可以飞回到有信号区域。
	- 如果最后发出的信号是 10，提示飞机向左，我们断定飞机处于右边，并让它大力向左以修正。

##### 6.1.2 Conveniencia de sincronización
看不懂。

##### 6.1.3 Señal de reloj: Clk
Señal de reloj / 时钟信号是一种周期性的二进制信号。
- 二进制代表它的值要么是 0 要么是 1。
- 周期性代表它会每隔一个 tiempo de ciclo / Tc 就重复一次（但通常来说，会使用 u.t. 或者 s / ns 做单位）。

![6 - 6.1.31.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.1.31.png)

##### 6.1.4 Circuito secuencial síncrono
在一个 circuito secuencial síncrono / 复杂同步时序电路中，计算机会通过时钟信号的 **flaco ascendente / 上升沿，由 0 变 1 的瞬间** 来指示 **所有电路的输入与输出信号都是正确的**。

我们以 “计数器” 举例（用于计数输入口的信号变 1 的次数）。我们发现每次输入变 1 的时间段都和时钟电路的上升沿的瞬间对应，所以这个计数器可以完美工作。

![6 - 6.1.41.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.1.41.png)

所以我们可以说，**时序电路内的时间单位是 Tc，是每一次时钟电路的循环**。

###### <b style="color: #5DD0C8;">核心概念：Señal síncrona - 同步信号</b>
在一个同步的时序电路中，信号值的即时状态并不重要，重要的是**稳定后的正确值**。
	这个稳定值指的是在每个时钟周期结束前，电路中所有信号都已经达到的正确状态。

为了确保所有信号在时钟周期结束前都能稳定，时钟周期的时长必须**足够大**，以便信号在电路中有足够时间**通过所有逻辑门并稳定到正确值**。
	电路中真正有意义的值是**与时钟电路上升沿对应的那个值**。

###### <b style="color: #5DD0C8;">核心概念：Cronograma simplificado</b>
我们知道，因为 Tp 延迟，一个电路的信号可能有多次的波动。但是真正重要的是与上升沿对应的那个值。

为了忽略波动的干扰，我们会使用**简化了的 cronograma**。如下图的底部，secuencia x 一栏。它只记载上升沿瞬间的值。
	时间表**以 Tc 为单位**。

###### <b style="color: #5DD0C8;">核心概念：Circuito secuencial síncrono</b>
时钟电路的上升沿还指定了**储存设备更新的时间**。

![6 - 6.1.42.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.1.42.png)


##### 6.1.5 El biestable D activado por flanco
首先，我们来研究时序同步电路中基础的基础：Biestable D。它们就像 circuitos complejos 中的逻辑门一样基础。

###### <b style="color: #5DD0C8;">定义</b>
**组成**：一个触发器 D 由一个入口 D，一个出口 Q 和一个时钟入口 Clk 组成，三者都是 1 bit 的。

**原理**：当时钟信号从 0 变 1 的瞬间，D 会把此时入口的值显示在出口 Q。这个值会保持到下一个由 0 变 1 的瞬间。
	在两个瞬间之间，因为电路的 Tp，D 入口的值可能会不断变化，但并不会导致 Q 的变化。

###### <b style="color: #5DD0C8;">符号</b>
下图左边展示了**大写 D** 入口与**大写 Q** 出口。

Clk 入口不需要符号内的标识，但是我们可以看见模块内存在 **" > " 样式的记号**。这代表当时钟电路**到达上升沿**时，作用于模块。

###### <b style="color: #5DD0C8;">Cronograma</b>
除法器 D 的时间表在于 Clk、D 与 Q 之间的关系。
- Clk 的上升+下降 = 一个循环。以上升沿作分界；
- d 可能不断变化，但被记录下来的只有上升沿的一瞬间；
- q 记录那些瞬间。

###### <b style="color: #5DD0C8;">Tp</b>
除法器和逻辑门一样，都有 Tp。在触发器中，Tp 指的是 **从 Clk 上升沿开始，到 q 成功记录 d 的值的间隔**。

###### <b style="color: #5DD0C8;">Cronograma de Tp</b>
我们以下图为例，讲解绘制 Cronograma 时需要注意的一些点（设 Tp=50u.t.）。

我们需要用箭头标注**入口-出口**的值的对应关系，展现 **q 记录的是 d 哪个瞬间的值**（从 d 指向 q）。
	但如果时钟到了但 **q 没变化，就不用标记**了。

![6 - 6.1.51.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.1.51.png)

简洁一点的时序图：
![6 - 6.1.52.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.1.52.png)

###### <b style="color: #5DD0C8;">将 Cronograma 作为延时器</b>
通过观察上面最后一幅图，我们发现，d 在 n 循环时的值 = q 在 n+1 循环时的值。
在同一个上升沿时，q 中的值是 d 在上一个上升沿时候的值。
- 下图是 Cronograma simplificada，清晰地展示了这个理论：

![6 - 6.1.53.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.1.53.png)

##### 6.1.6 El registro
因为现在的计算机至少是 16 bits 的，所以只储存 1bit 是不够的。我们需要使用 **Regitro / 寄存器** 以响应时代号召。

一个用于储存 n bits 的寄存器由 n 个触发器组成，结构如下：
![6 - 6.1.61.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.1.61.png)

##### 6.1.7 Reglas de interconexión de dispositivos combinacionales y biestables
用于连接触发器与寄存器的规则
###### <b style="color: #5DD0C8;">Reglas de interconexión de las señales de datos</b>
1. 整个电路的入口必须与 dispositivos combinacionales 或者 biestables 相连。
	然后，这个 dispositivo 的出口必须与 其它的 dispositivos combinacionales、biestables 或者 整个电路的出口相连。

2. 全部模块的入口都**必须有 valor lógico**，不可以 estar al aire。
	此外，所有模块的出口也**不可以直接相连**。

其实前面两个规则也同样适用于 circuitos combinacionales。

3. 时序电路的特权在于**可以组建回路**，不过这个回路也有一定的限制：
	回路中需要**包含至少一个触发器或寄存器**。
		- 回路：caminos cerrados que empiezan en un dispositivo y terminan en el mismo dispositivo / 从设备A开始也从设备A结束的闭路。
		- 比如下图中 CLC2 的那个。

###### <b style="color: #5DD0C8;">Reglas de interconexión de las señales de datos</b>
La señal de reloj (Clk) 是同步时序逻辑电路的必有的总入口。它连接着所有的触发器与寄存器。
	有时，为了整洁，我们会省略掉 Clk 的线（但下图中是有的）。

![6 - 6.1.71.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.1.71.png)


##### 6.1.8 Tiempo de ciclo
这只是粗略介绍，详见 6.6。
###### <b style="color: #5DD0C8;">Presentación del ejemplo</b>
（总入口）X→  Registro1  Y→  Incrementador  Z→  Registro2  W→（总出口）
- Tp registro = 40 u.t.
- Tp incrementador = 100 u.t.

因为 Tp，我们还看得见前两个循环的末尾与后一个循环的开头。但我们不用管它们，只需要专注于 ciclo k：367 - 368 的过程：

- **ciclo k-1 末**
	这时，Registro1 的输入稳定在 367。经过 40 u.t. 之后，数值被储存在 Registro1 的出口。

- **ciclo k 期间**
	经过 40 u.t. 之后，数值被储存在 Registro1 的出口。
	
	Incrementador 开始工作。经过 100 u.t. ，等到 ciclo k 的末尾，数值被固定在 "368"。这个值**在 Registro2 的入口被稳定**（**但并未储存**，需要等到 k+1 才开始储存）
		由于 circuito combinacional 物件中的 Tp，我们可以在 Z 的时间表中看见数值的波动。我们对这些波动的细节并不感兴趣，所以我们会**用杂乱的X形纹路标记这段波动的时间**。
	
	*与此同时，等到 ciclo k 末期，Registro1 的入口稳定在了一个新的值：24（未储存）。*

- **ciclo k+1 初**
	Registro2 开始工作，经过 40 u.t.，“368” 出现在 Registro2 与 整个电路的出口 W，被稳定。
	
	*与此同时，也经过 40 u.t. ，Registro1 出口上的值被 “24” 代替。然后经过 100 u.t.，变成了 "25" 出现在 Incrementador 的出口。*

- **举例一些意外**
	- 如果这个电路的 Tc 小于 140 的话，Reg2 就拿不到正确的值了（Reg1 处理 + Incr 处理 = 40+100 u.t.） 。
		而是会拿到 Incrementador 波动期间的不正确的值。
	
	- 170 u.t. 的 Tc 使得 Registro 拿的是 (Reg1处理 + Incr处理) 完成并稳定了 30 u.t. 的值。

- **定义：Camino crítico del circuito secuencial**
	在 circuito secuencial 中，camino crítico 是**==从一个 biestable/registro 的出口 到 另一个 biestable/registro 的入口** **耗时最长**的那段路段==。
		- 这个路段会穿过一些 dispositivos combinacionales，但不穿过任何 biestable/registro。因为一旦路段到达 biestable/registro，路段结束。
		- 在之前的例子中，这个路段是从 Registro1 经过 Incrementador，直到输出的那段路。
	
	==整个 circuito secuencial 的 Tc 必须大于 camino crítico 的 Tp。

![6 - 6.1.81.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.1.81.png)


### 6.2 Estructura general de un circuito secuencial

##### 6.2.1 Modelo de Mealy
我们在之前说过，同步时序电路是由 dispositivos combinacionales 与 dispositivos biestables 组合而成的。如下图所示：

![6 - 6.2.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.2.11.png)

上图展示的是一个时序电路的图示：

- CLC 代表所有 dispositivos combinacionales 的集合，在一个模块中。
- REG 代表 registro（biestables 的集合）。

这里 REG 的入口与出口有点不一样：
- 出口 Q 读作 “estado actual”，代表**当前时钟周期内**的状态；
- 入口 Q+ 读作 “estado siguiente”，因为**需要 +1 个时钟周期**才能知道它的状态。

同时，可以看见，总出口 W 和 Q+ 源自于 X 和 Q 的处理。
	H 代表通过 XQ 组合计算出 W 的方法
	G 代表通过 XQ 组合计算出 Q+ 的方法

###### <b style="color: #5DD0C8;">咩阿栗简介</b>
上图的 CLC 也可以按照出口 G 与 H 分解成两个模块。CLC-G 专注于计算 el estado siguiente；CLC-H 专注于计算整个电路的出口。

分解的结果如下图所示，这就是著名的 ==Modelo de Mealy - 咩阿栗模型== 。
- 好啦，人家的正经名字是 **米利模型**

![6 - 6.2.12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.2.12.png)

对于咩阿栗模型最好的**描述**如下：
- （总入口有**多少种**输入变量）n，（叫什么）X
- （总出口有**多少种**输出变量）m，（叫什么）W
- （电路内需要处理**多少 bits** 的信号）k
	不需要给 Q 与 Q+ 命名，它们就叫 Q 和 Q+。

###### <b style="color: #5DD0C8;">TV 绘制：</b>
绘制 TV 时，我们一般会把**关于 estado 的变量尽量放在左边**（q 变量），把 **entrada 变量尽量放在右边**（x 变量）。这样更清晰明确。

**El estado inicial** 代表 estado 变量的预设值。也就是当整个电路**起步时，estado 变量会是什么**。
	在下面的例子中为 q1=0 与 q0=0（图中没标）。

- CLC-G 的 TV 被称为 **Tabla del estado siguiente / table de treansiciones**
	用于查看不同的 QX 组合会得出怎么样的 Q+。
![6 - 6.2.13.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.2.13.png)

- CLC-H 的 TV 被称为 **Tabla de salida**
	用于查看不同的 QX 组合会得出怎么样的 W。
![6 - 6.2.14.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.2.14.png)

我们发现，两个例子中没有任何一个 Q=10 或 Q+ =10。既不存在（q1=1, q0=0）也不存在（q1+ =1, q0+ =0）。
- 遇到这些不可到达的状态时，我们会用 xx 表示这个状态是不可到达的 / estados inalcanzables。
- **（不可达状态应该是取决与具体设计，而不是模型限制）**

##### 6.2.2 Modelo de Moore
Modelo de Moore / 摩尔模型 是 咩阿栗模型 下的一个特殊状态。

当电路的出口**仅仅是 estado actual Q 的 función 时**（不再依赖整个电路的输入 X），就是 Moore 模型。
- 此时 CLC-H 的入口会从两个（源于 XQ）减少至一个（仅仅是 Q）。

![6 - 6.2.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.2.21.png)

此时，CLC-H 的 TV 也从 32 个可能缩小成了 4 个可能。

此外，这个例子中，“不可达状态” 变成了 11，而不是 10**（应该是取决与具体设计，而不是模型限制）**。


![6 - 6.2.22.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.2.22.png)


### 6.3 Grafo de estados
让我们看看摩尔模型的图表表达。

##### 6.3.1 La tabla de transiciones en el grafo
图表表达的 Tabla de transiciones (CLC-G)。
- 每个 Q 都对应着什么 Q+？
- 每个 XQ 都对应着什么 Q+？

###### <b style="color: #5DD0C8;">Los nodos - 结点</b>

图表中每个**结点都代表**一个可抵达的 Q。
	电路的 bits 与 “理论上可抵达的 Q” 存在一定联系。公式：Q = 2^k

在下面的电路中，Q=11 是不可到达的，所以只剩下 2^2 -1 种 Q，也就是三种。
- Los estados iniciales 使用双层圆圈表示

![6 - 6.3.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.3.11.png)

###### <b style="color: #5DD0C8;">Los arcos dirigidos - 箭头 - las flechas</b>

 每个结点**发射的箭头的数量**与 valores de entrada X 的排列组合数量相等，因为每个箭头都代表一种组合 X ：00, 01, 10, 11。

 **箭头的目的地**是 XQ 组合对应的 Q+。
	代表 Q 的结点 + 代表 X 的箭头 = Q+ = XQ 的排列组合。

以上图为例，被高光标记的那一行，与图示中被高光标记的节点与箭头表达：
- 当 Q=01，X=00 时，它们的 Q+ 会是 00。

###### <b style="color: #5DD0C8;">De la tabla al grafo y del grafo a la tabla</b>
因为下面这个例子中的 11 不可到达，所以 XQ 组合中会有一对组合得出相同的 Q+。
	在图示中，每个结点都会发射出一个 “同时承载两种 entrada，但只指向一个结点” 的箭头。

![6 - 6.3.12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.3.12.png)


##### 6.3.2 La tabla de salida en el grafo
在摩尔模型下，因为 tabla de salida 非常简单，我们会倾向把 tabla de salida 糅合进 tabla de transiciones 里面。此时，结点将被划分为**上下两层**：
- 结点上层依然代表所有**能抵达的 Q**；
- 结点下层代表该能抵达的 Q **对应的 W**。

![6 - 6.3.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.3.21.png)

###### <b style="color: #5DD0C8;">La leyenda del grafo - 标注</b>
设想一下，如果我们只呈现图示，而不呈现 TV 的话，我们不就不知道图示上的值的顺序与名字了吗？是的。所以，我们需要在图示旁边做一个**标注**。

标注由**单个结点**与**单个箭头**组成。结点和箭头上标注的是**变量的名称**，而**不是具体的比特值**。

![6 - 6.3.22.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.3.22.png)

##### 6.3.3 Seguimiento de grafos de estados
让我们仅仅通过图示，来得出 cronogramas simplificadas。

下图中虽然展示了并未编码的变量名 E0，E1，E2，但无伤大雅。我们也可以使用变量名制作 cronograma simplificada。

![6 - 6.3.31.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.3.31.png)

我们需要尝试在上面时间表的基础上填完整个时间表。我们拿 ciclo 4（在表上与图示上被高光）来举例推理的思路（已知每次的 entrada X）。我们看见：

1. 看 04 结点，知道 04 ciclo 的 Q 与 W；
2. 根据时间表上给出 的 04X，找出它之后指向哪个结点；
3. 找到前者指向的结点，看 05 结点，知道 05 ciclo 的 Q 与 W；
4. 根据时间表上给出的 05X，找出它之后指向哪个结点；
5. 找到前者指向的结点，看 06 结点，知道 06 ciclo 的 Q 与 W；
6. 循环往复

由此，我们开始填空：
- 10：E2 0 1
- 11：E0 1 0
- 12：E2 0 1
- 13：E0 1 0
- 对辣！

##### 6.3.4 Grafos de estados para circuitos de Mealy
咩阿栗模型比莫勒模型复杂了不止一点。所以将 tabla de salida 与 table de transiciones 糅合成一个的办法行不通了。我们只能 **仅仅做 grafo de la tabla de transiciones，并在它旁边放一个 tabla de salida**。如下图所示。

![6 - 6.3.41.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.3.41.png)


### 6.4 Análisis lógico
想要分析一个时序电路的话，需要注意以下几点。
###### <b style="color: #5DD0C8;">Nombre de las señales</b>
- 首先：确认 biestables, entrada y salida 的**名称**。
	- 一般来说，输入输出的名称会在电路图中被标注。
	- 但 biestables 的名称是任意取的，或许可以是 q_k-1, ... , q1, q0（k = bits 的位数）。
	- 此外还有一个关于 biestable 的规则：如果它的 estado actual 是 Q，那么 estado siguiente 必须叫 Q+。

###### <b style="color: #5DD0C8;">¿Circuito de Mealy o de Moore?</b>
- 其次：我们需要判定电路的性质。咩阿栗还是莫勒？

###### <b style="color: #5DD0C8;">Obtención de la tabla de transiciones y la de salida</b>
- 第三步：对 CLC-G 与 CLC-H 进行逻辑分析（anàlisis lògica），**得出 Tabla de transiciones 与 tabla de salidas**。

![6 - 6.4.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.4.01.png)
![6 - 6.4.02.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.4.02.png)

###### <b style="color: #5DD0C8;">Obtención de la tabla de transiciones y la de salida</b>
最后一步：根据 TV 画出 grafo de estados。
- 注意画出 leyendas。
- 注意画出 estados iniciales。


### 6.5 Síntesis de circuitos secuenciales
我们将时序电路的制造分为三步：
1. 从**功能描述**到**状态图示**
2. 从**状态图示**到 **tabla de transiciones y salida**
3. 从 **tabla de transiciones y salida** 到 **esquema lógico**

##### 6.5.1 De la descripción funcional al grafo de estados
对电路的功能描述可以通过 文字、高级编码语言、数学式子 等等载体进行。也可以提出一序列的输入值，并将其一一对应到预期的输出值来指定，组成一个 cronograma simplificada。

将一个描述实现成图表很难，靠理解与经验。无定法。

###### <b style="color: #5DD0C8;">Ejemplo 3：试着做出 biestable D</b>

**功能描述**
k+1 循环的电路出口 q = k 循环的电路入口 d ：q(k+1) = d(k)

**推理**
通过观察，我们断定：
- estado siguiente Q+ 的值 = 电路入口 d 的值；
- estado actual Q 的值 = 电路出口 q 的值。

同时，因为出口只有两个可能的 bit，而出口 = Q，所以 Q 只可能有两种 estados：
- Q = E0：对应 q = 0
- Q = E1：对应 q = 1
	当然，把 E0 定义成出口为 1，把 E1 定义成出口为 0 也是没问题的。

然后，做出简易时间表，再做出图示。

![6 - 6.5.11.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.11.png)
![6 - 6.5.12.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.12.png)

###### <b style="color: #5DD0C8;">Ejemplo 4: Reconocedor de la secuencia 011</b>

根据下图做出一个逻辑电路的图表。电路的目标是 “检测输入是否是 011” ：若是，输出 1；若否，输出 0：

![6 - 6.5.13.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.13.png)

因为这是个莫勒模型，所以电路的出口肯定仅取决于 Q。因此，我们敢保证，W 对于输入的**判定绝对延后输入至少一个循环**。思路如下：

**输入**
- k-2 循环：输入 0
- k-1 循环：输入 1
- k 循环：输入 1。此时，011 已被完整输入。

**输出**
- k 循环结束时：w=1 这个判定在最快的情况下，也只是被卡在 estado siguiente（biestable 模块的入口，时钟电路尚未让 biestable 模块更新）。
- k+1 循环开始后：经过一定 u.t. 后，biestable 模块结束更新，w=1 被输出到 w 口。

###### <b style="color: #5DD0C8;">Un grafo con 8 estados</b>
将电路描述转化为实践的思路很灵活，其中重要的一点是 “**电路需要记住多少比特才能达成目标**” ？

因为需要记住的比特与 Q 的位数关联，而 Q 的位数又与图示中的结点数量关联。

假如，电路的目的是检测 011，这样电路需要记住 **输入的最后三个比特**，我们将这个信号命名为 tdu（tres dos uno）：

**思路**
- 当输入格式为 011 ，相应循环后输出为 1，其余输入格式的输出都为 0。
- 三个比特输入代表会有 2^3 个 estados。

1. 题目没告诉我们 estado inicial 是什么，我们**默认它是 000**。

2. 然后，**理清哪些 Q 会导致 Q+ 为 011。** 因为那些 Q 的箭头会指向 011
- 011 代表 k-2 循环时输入了 0，k-1 循环时输入了 1，k循环时输入了 1。最后，在 k+1 循环时，电路输出判定 1。
- 所以，**当前一个 Q 的后两位是 01 且下一个输入也是 1 时**，导致 Q+ 是 011。

3. 接下来，**理清 011 会导致哪些 Q+**，也就是 011 的箭头指向的目标
- 如果在 011 后输入进了个 0，那么接下来的状态 Q+ = du0；若输入进了个1，那么接下来的状态 Q+ = du1。
- 所以，011 的 Q+ 有可能是 111 与 110。

![6 - 6.5.14.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.14.png)

###### <b style="color: #5DD0C8;">Un grafo con 15 estados</b>
之前，我们假设了初始状态是 000。现在，我们**假设 estado inicial 中没有任何值**。

为了应对这种情况，我们需要换个办法画图:
![6 - 6.5.15.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.15.png)

1. 第一阶级，我们使用 **... 代表一个没有值的 registro** 。
	下个循环时，输入可能是 1，也可能是 0。我们给每一种可能画出一个分支
	别忘了给 estado inicial **画同心圆**！即使它没有值！

2. 第二阶段，出现了输入，占据了第三位，但前两位依然没有值。我们使用 ..0 或 ..1 来代表上个阶级分下来的两个可能。
	下个循环时，每个可能又可能有两个输入，我们给每个可能继续分出两个分支。

3. 如此循环往复，直到所有位数都被占据为止。
	**别忘了在每种输出下标 W 对应的值！**

###### <b style="color: #5DD0C8;">Un grafo con 8 estados equivalente al de 15</b>
如果我们将上上图中的 estado inicial 换成 111 的话，我们就得到了一个仅使用 8 个结点，但和上图一样价值的图示（？不理解）。

###### <b style="color: #5DD0C8;">El grafo óptimo con 4 estados</b>
一个只包含四个 estados 的图表无疑是效率最好的。但它不会那么全面，只需要 “挑出一些最重要的信息来表达”。

**Fusionando estados - 第一种作图法：裁去无意义的状态**
对于一个只需要检测 011 的图表来说，树状图中的一些枝丫是无意义的。
- 比如枝丫 ..1，它在第一个分叉就注定不可能到达 011。所以**它与它的延申都是无意义**的。
- 还有枝丫 .00 及其延申。
- 以及那些在第三阶级，输入为 0 的所有枝丫。

**Empezando de nuevo - 第二种作图法**

1. 我们假设现在电路状态在 ... ，没有任何输入。
	- 此时，电路期待输入为 0，因为这样才可以到达 011。
	- 如果此时输入为 1，电路让它返回至 ... 。

2. 在获得 ..0 后，如果输入为 1，进位至 .01。
	如果此时输入为 0，返回至 ..0 。

3. 在获得 .01 后，如果输入为 1，进位至 011，输出 1。
	如果此时输入为 0，**它无法返回至 .01，因为 .01 的最后一位是 1，而这个循环的输入是 0**。
	所以，电路只能让它**返回到上一个同为 0 结尾的** ..0。

4. 获得 011 并输出 1 之后，电路并未停止，我们还需要检测之后的输入。我们**期待另一个 0，获得 ..0**。
	如果此时**输入为 1，电路需要重置到 ...** 。

理清逻辑后，图示如下。**这个图的功能与上上图那个复杂的老东西完全一致，但它只有四个节点**：
![6 - 6.5.16.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.16.png)

设输入为 10010111，电路的建议时间表与逻辑路程如下，在图示中被高光画出:
![6 - 6.5.17.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.17.png)

###### <b style="color: #5DD0C8;">Ejemplo 5：Detector de la secuencia 1101 sin y con solapamiento</b>
电路分为 ”可重叠电路“ 与 ”不可重叠电路“。假设我们需要检测字符串 1101：
- 当电路输入为 1101101 时，电路如果做出了两次正确判定，该电路为可重叠电路。
- 当电路输入为 1101101 时，电路不做出两次正确判定，该电路为不可重叠。

![6 - 6.5.18.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.18.png)
![6 - 6.5.19.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.19.png)

**a) sin solapamiento**

1. 初始状态依然是 ....
	如果此时入口是 1，进位至 ...1；若否，返回 .... 。

2. ...1
	如果此时输入是 1，进位至 ..11；若否，返回 ...1 。

3. ..11
	如果此时输入是 0，进位至 .110；
	若否，无法返回至 ..11 或 ...1，只能从 .... 开始。

4. .110 
	如果此时输入是 1，进位至 1101，在下次循环输出 1；
	若输入是 0，无法返回至 .110。因为这次输入与之前输入的后三位是 .100，和 .110 毫无瓜葛。只能返回至 .... 。

5. 1101
	在下次循环输出 1。
	如果此时输入是 1，返回至 ...1；若否，返回至 .... 。

**b) con solapamiento**

1. 同上
2. 同上
3. 同上
4. 同上

5. 1101
	在下次循环输出 1。
	如果此时输入是 1，返回至 ..11，若否，返回至 ....。

![6 - 6.5.110.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.110.png)

###### <b style="color: #5DD0C8;">Ejemplo 6：El piloto automático</b>
对于这个主题，输出 W 的位数至少有两位才能满足这三种输出：直行 - 00、向左 - 10 与 向右 - 01。

同时，我们将飞机接收到的信号作为电路输入，也需要两位 si 与 sd 。前者表示是否接收到了跑道左边的信号，后者表示是否接收到了跑道右边的信号，

思维如下：
- 当输入是 11，代表两边都接受到了，输出 00。
	我们将这种状况命名为 C，centro。

- 当输入是 10，代表飞机偏移，只接收到了左侧的信号。
	我们将这种状况命名为 CI，centro izquierda
	应命令飞机往右拐（输出 01）。一旦输入变成 11，将模式切换成 C。
- 当输入是 01，代表飞机偏移，只接收到了右侧的信号。
	我们将这种状况命名为 CD。
	应命令飞机往左拐（输出 10）。一旦输入变成 11，将模式切换成 C。


- 当输入先是 10，再变成 00，代表飞机向左偏移得离谱，处于无信号区。
	应命令飞机往右拐（输出 01）。一旦输入变成 10，将模式切换成 CI。
	我们将这种状况命名为 I。
- 当输入先是 01，再变成 00，代表飞机向右偏移得离谱，处于无信号区。
	应命令飞机往左拐（输出 10）。一旦输入变成 01，将模式切换成 CD。
	我们将这种状况命名为 D。

图示暂时如下，后面还有优化。
![6 - 6.5.111.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.111.png)

下图在上图的基础之上改变了几点：

1. 添加了 “不可能抵达“ 的箭头，比如 I 状况下的 01（飞机不可能从左偏移无缝切换到右偏移）：
	这些 ”不可能抵达“ 的箭头是可加可不加的。因为我们默认，如果一个箭头理论存在但是没被表达在图示中，就是因为它在实例中不可抵达。把它们加上纯粹是为了满足理论要求（2 bits 导致每个结点发射四个箭头）。

2. 将 C/CI 糅合，还有将 D/CD 糅合。
	因为它们的输出是一样的。

![6 - 6.5.112.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.112.png)

##### 6.5.2 Del grafo de estados a las tablas transiciones y salidas

###### <b style="color: #5DD0C8;">Biestable D</b>

**1. 通过公式 log2^(Q的数量）来知道需要多少 bits。**
一个 biestable D 只有两个状态，log2^2 = 1，只需要 1bit 。
	我们将 E0 赋给 0；E1 赋给 1。

**2. 列出相应的 TV**
其实 Q 的一列都可以省，这个例子太弱智了。

![6 - 6.5.21.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.21.png)


###### <b style="color: #5DD0C8;">Un ejemplo más complejo</b>
- 通过计数 nodos 的数量，我们知道下图示有三种 Q。
	分别命名 E0 - 00；E1 - 01：E2 - 10。

- 然后按照理解观察图表，得出 TV。

![6 - 6.5.22.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.22.png)
![6 - 6.5.23.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.23.png)


##### 6.5.3 De las tablas al esquema lógico
好，根据 TV 画逻辑电路图！

###### <b style="color: #5DD0C8;">Biestable D</b>
略
###### <b style="color: #5DD0C8;">Un ejemplo más complejo</b>
让我们来画出上个章节同名小节中的逻辑电路图叭！我们将用到一个 ROM，一个 decodificador，一个用于连接出口的 Or 门，与两个 biestables de estados 来拼出它。

**第一步，确认 k、m、n 的数量与命名**

k 代表 biestables 的位数；n 代表 entrada 的位数；m 代表salidas 的位数。
- Estados 的名字一般是 qk-1 ... q1, q0；estados siguientes 对应的是 q+k-1, ... , q+1, q+0 （与 TV 上的保持一致）。
- Entrada 一般是 xn-1, ... , x1, x0。
- Salida 一般是 wm-1, ... , w1, w0。
	这个例子中，k = n = m = 2 。

**第二步，开画**

我们把这个步骤拆分为两小部分：
1. 使用 tabla de transiciones 画出 el circuito del estado siguiente 。应应用到所有变量。
2. 使用 tabla de salidas 画出el circuito de las salidas 。

![6 - 6.5.24.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.24.png)

## 0.0 看不懂
Con una ROM y un multiplexor de buses. Es posible sintetizar a la vez el circuito del estado siguiente junto con el de las salidas usando una ROM y un multiplexor de buses. El ejemplo anterior se muestra en la figura 6.50. En general, si tenemos un circuito secuencial con n bits de entrada, m de salida y k de estado, la ROM tiene 2k palabras de kx2n + m bits por palabra y el multiplexor de buses tiene n bits de selección para elegir entre 2n buses de k bits cada bus. Los bits de dirección de la ROM son los bits de estado del circuito y los bits de selección del multiplexor son los bits de entrada del circuito. Hay una palabra de la ROM para cada estado del circuito y cada una tiene la salida del circuito para ese estado y además el estado siguiente para cada uno de los posibles arcos que salen de ese estado (para cada combinación de valores de las entradas). Es un diseño sencillo, pero es fácil olvidarse alguna de las etiquetas de la ROM o del multiplexor o cometer un error al especificar la tabla que representa el contenido de la ROM, lo que haría que el circuito no quedara correctamente especificado.

![6 - 6.5.25.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.5.25.png)


### 6.6 Análisis temporal
目标：计算出最短的 Tc！

逻辑电路的 **时间分析** 需要根据逻辑电路图来制作。

###### <b style="color: #5DD0C8;">Camino crítico de un circuito secuencial y tiempo de ciclo</b>
一个 Tc 的时间就是两个时钟电路上升沿之间的间隔。这个间隔需要大于整个电路的 **Camino crítico** 所消耗的 Tp。

**Camino crítico de un circuito secuencial** 指的是整个电路中最长的，从一个 biestable 的出口到另一个 biestable 的入口所消耗的时间。
	这段路径中会穿过一些 circuito combinacional ，但绝不穿过第三个 biestable。如果穿过，只能证明这个路径的 Tp 还能缩短。
	就跟队伍里带斯卡蒂一样（被打）。

但因为时序电路常常都是组合起来使用的，会出现时序电路 A 的出口 = 时序电路 B 的入口的情况，所以 **camino critico 也可能出现在两个电路相互连接的路径上**。

总结：一共有三种路径可能。它们的名字与计算公式如下：

- 从 biestable 入口到 biestable 入口（camino b-b）
	= 开始的 biestable 传播时间 + 路径经过的 dispositivos combinacionales 的总和

- 从 entrada 到 biestable（camino e-b）
	= 输入端时间 + 路径经过的 dispositivos combinacionales 的总和

- 从 biestable 到 salida（camino b-s）
	开始的 biestable 传播时间 + 路径经过的 dispositivos combinacionales 的总和 + 输出端时间

上面的 输入端时间 与 输出端时间 会在题目中给出。
有时，我们的电路还连接着个上家电路；或者连接个下家电路：
	在前者，e-b 需要加上 “上家 biestable 延迟 + 经过电路 + 上家 salida 延迟；
	后者，b-s 需要视情况而定。

###### <b style="color: #5DD0C8;">Encontrando el camino crítico del ejemplo</b>
设：自时钟上升沿开始，信号需要 110 u.t. 才能稳定在电路中；在出 salida 之后，信号需要 20 u.t. 才能稳定。下图中：
- 传播时间最长的 camino e-b 是 entrada + Not + And-2 + Or-2 = 110 + 20 + 20 + 30 = 180 u.t.
- 传播时间最长的 b-b 是 q1 + NOT + And-2 + Or-2 = 150 + 20 +30 + 30 = 230 u.t. 
- 传播时间最长的 b-s 是 q1 + NOT + And-2 + Or-2 + salida = 150 + 20 + 30 + 20 + 20 = 240 u.t.

所以，这个电路的 Tc 要大于等于 240 u.t. 才行。

![6 - 6.6.01.png](/img/user/Instrucci%C3%B3%20al%20computador/Tema%206/%E5%9B%BE%E7%89%87/6%20-%206.6.01.png)




