---
{"tags":["Programació","UPC"],"dg-publish":true,"permalink":"/真编程大道之c++/课后笔记/Tema 6 - Functions/","dgPassFrontmatter":true,"created":"2024-10-25T10:45:51.397+02:00","updated":"2024-11-18T14:39:48.530+01:00"}
---


## 6.1 Subprograms

#####  <b style="color: #5DD0C8;">分类与定义</b>
**Subprogram**（子程序）有两种： **functions** 与 **procedures**。

- Function - 有返回值的函数。被调用时类似于**计算表达式**。
	```c++
	int times (int x, int y) {
		return x;
	}
	```
	```c++
	i = times (3, i+2) + 1;       // 调用格式
	```

	其中：
	- `int` 是 **type of result**，返回类型
	- `times` 是函数名
	- `int x` 与 `int y` 是形式参数
	- `return` 代表需要返回的值
	- 花括号内的是函数体

	调用格式大意：
	1. 将 3 传给 x；将 i+2 传给 y
	2. 执行函数体
	3. 将运算过后的 x 返回至目标函数
	4. 将运算过后的 x +1
	
	注意，调用时，**argument / 实际参数 的变量类型必须与 parameter / 形式参数 的类型一致！**

- Procedures - 无返回值的函数。被调用时类似于 **statements / 一般语句**。
	```c++
	void factors (int x) {
		
	}
	```
	```c++
	factors(i);                  // 调用格式
	```
	
	其中，`void` 是 procedures 专用的返回类型

	调用格式大意：
	1. 将 i 的值传给 x
	2. 执行函数体
	3. 视情况，有没有返回（可能有返回，但绝不是一个 “值”）


Subprogram 可以在主函数模块**之前**定义，也可以在主函数模块**之后**定义。
	**重中之重：一定要 declare / 声明**。只有在声明之后，函数才可以被调用。


#####  <b style="color: #5DD0C8;">Parameter passing</b>
在 c++ 中，argument 是被 **pass / 传** 给 parameter 的。这个 passing 的方式共有两种：
1. Call-by-value
2. Call-by-reference

**Call-by-value**
这种传值方式是将 argument **赋值**给 parameters 。两个函数中的变量**在内存中的地址不同**，毫无瓜葛：

```c++
swap(a,b);

// 就相当于 ↓

int swap (int x, int y) {
	x = 10086; y = 170001;
}

```

这时，就算 swap 函数中的 `x=10086`，主函数中的 `a` 也还保持着他自己的值。

**Call-by-reference**
这种传值方式是将 parameter 与 argument 合二为一。它们的名字虽然不一样，却是同个内存地址中的同个东西，变成了一种鲁迅与周树人之间的关系。

如果想使用 call-by-reference，就需要在 subprogram 的 parameter 圆括号内，往**变量类型的右方加上一个 `&`**。

```c++
swap(a, b);
```
```c++
int swap (int& x, int& y){
	int i = x;
	x=y; y=i;
```

这时，如果对 `x` 的值动手脚，返回后，与 `x` 绑定的 `a` 也会被动同样的手脚。

**总结：**
- Call-by-value，在函数独立的时候使用。且可以**与任何类型的表达式**作绑定。
- Call-by-reference，在函数需要直接影响 argument 的时候使用。但**只能与变量**作绑定。




**该笔记内容来源于 c 语言程序现代设计 P.140 函数章节。c 语言是没有 call-by-reference 的极致的**。
## 6.99 函数的定义和调用

#####  <b style="color: #5DD0C8;">函数的结构</b>
一个函数的结构如下：
```c++
double Average ( double a, double b) {

return (a + b) / 2;
}
```

- **第一部分：return type / 返回类型**
	这个例子中，函数的 return type 是第一行中的第一个 “double”。它定义了每次调用函数时**返回值的类型**，或者说，整个函数最后输出的值的类型。
	
	- 当这个函数的任务是**返回一个值**的时候，返回类型就和变量一样。可以是 `int, double, bool` 什么的。
	- 但是当这个函数**没有返回值**的时候，比如用作输出语句时，这个部分需要是 `void`。

- **第二部分：标识符 / 变量名**
	这个例子中，函数的标识符是 “Average”。这和变量概念中的标识符一个意思，用于概括函数的目的。

- **第三部分：parameter / 形式参数**
	这个例子中，函数的形式参数是圆括号里的两个值。代表在调用该函数时，需要提供的两个值。注意，**每个变量前面都必须跟着它们的变量类型**：
		上面的 `(double a, double b)` 是正确例子，但 `(double a, b)` 是完全错误的例子。

- **第四部分：body / 函数体**
	这个例子中，函数体代表花括号内的语句。
	
	需要返回值的函数以 `return 什么什么` 作为最后一个语句。在执行 return 语句后，**return 后面跟着的变量会被返回至调用函数的地方**。
	
	不需要返回值的函数就不需要 return。


#####  <b style="color: #5DD0C8;">调用函数的结构</b>
我们可能会在主函数中调用上面的例子：

```c++
#include <iostream>
using namespace std;

double average (double a, double b);          // DECLARATION

int main (void) {
	double x, y;
	cin >> x >> y;
	
	cout << average(x,y) << endl;             // 调用的格式
	
	return 0;
}

double average ( double a, double b) {        // DEFINITION
	return (a+b)/2;
}

```


















































