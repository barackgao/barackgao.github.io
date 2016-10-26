---
title: Markdown公式使用手册
tags: [Markdown,公式,mathtype]
date: 2016-10-26 17:03:20
categories: 技术

---
<script type="text/x-mathjax-config">
	MathJax.Hub.Config({TeX: {equationNumbers: { autoNumber: "AMS" } }});
</script>
Mou本身支持了公式编辑，可是上手还需要一定的时间，让我们先来总结一下吧。
<!--more-->
## 公式使用参考
### 如何插入公式
Mou的数学公式有两种：行中公式和独立公式。行中公式放在文中与其它文字混编，独立公式单独成行。
行中公式可以用如下方法表示：

	$$$ 数学公式 $$$

行中公式可以用如下方法表示：

	$$ 数学公式 $$

自动编号的公式可以用如下方法表示：

	\begin{equation}
	数学公式
	\label{eq:当前公式名}
	\end{equation}	

自动编号后的公式可在全文任意处使用`\eqref{eq:公式名}`语句引用。

* 例子：

		$$$ J\_\alpha(x)=\sum\_{m=0}^\infty\frac{(-1)^m}{m!\Gamma(m+\alpha+1)}{\left({\frac{x}{2}}\right)}^{2m+\alpha}\text{，行内公式示例} $$$
	
* 显示：$$$ J\_\alpha(x)=\sum\_{m=0}^\infty\frac{(-1)^m}{m!\Gamma(m+\alpha+1)}{\left({\frac{x}{2}}\right)}^{2m+\alpha}\text{，行内公式示例} $$$
* 例子：

		$$ J\_\alpha(x)=\sum\_{m=0}^\infty\frac{(-1)^m}{m!\Gamma(m+\alpha+1)}{\left({\frac{x}{2}}\right)}^{2m+\alpha}\text{，独立公式示例} $$

* 显示：$$ J\_\alpha(x)=\sum\_{m=0}^\infty\frac{(-1)^m}{m!\Gamma(m+\alpha+1)}{\left({\frac{x}{2}}\right)}^{2m+\alpha}\text{，独立公式示例} $$
* 例子：

		在公式 \eqref{eq:sample} 中，我们看到了这个被自动编号的公式。
		\begin{equation}
		E=mc^2 \text{，自动编号公式示例}
		\label{eq:Sample}
		\end{equation}
* 显示：在公式 \eqref{eq:Sample} 中，我们看到了这个被自动编号的公式。
\begin{equation}
E=mc^2 \text{，自动编号公式示例}
\label{eq:Sample}
\end{equation}
注意：以上的实现需要开启mathjax的自动编号配置

		<script type="text/x-mathjax-config">
			MathJax.Hub.Config({TeX: {equationNumbers: { autoNumber: "AMS" } }});
		</script>
		
### 如何输入上下标
`^`表示上标，`_`表示下标。如果上下标的内容多于一个字符，需要用`{}`将这些内容括成一个整体。上下标可以嵌套，也可以同时使用。
* 例子：

		$$ x^{y^z}=(1+{\rm e}^x)^{-2xy^w} $$

* 显示：$$ x^{y^z}=(1+{\rm e}^x)^{-2xy^w} $$
另外，如果要在左右两边都有上下标，可以用`\sideset`命令。
* 例子：

		$$ \sideset{^1_2}{^3_4}\bigotimes $$

* 显示：$$ \sideset{^1_2}{^3_4}\bigotimes $$
### 如何输入括号和分隔符
()、[] 和|表示符号本身，使用`\{\}`来表示{}。当要显示大号的括号或分隔符时，要用`\left`和`\right`命令。
一些特殊的括号：

|输入      |显示         |输入      |显示          |
|---------|-------------|---------|-------------|
|`\langle`|$$$\langle$$$|`\rangle`|$$$\rangle$$$|
|`\lceil` |$$$\lceil$$$ |`\rceil` |$$$\rceil$$$ |
|`\lfloor`|$$$\lfloor$$$|`\rfloor`|$$$\rfloor$$$|
|`\lbrace`|$$$\lbrace$$$|`\rbrace`|$$$\rbrace$$$|

* 例子：

		$$ f(x,y,z) = 3y^2z \left( 3+\frac{7x+5}{1+y^2} \right) $$

* 显示：$$ f(x,y,z) = 3y^2z \left( 3+\frac{7x+5}{1+y^2} \right) $$
有时候要用`\left.`或`\right.`进行匹配而不显示本身。
* 例子：

		$$ \left. \frac{{\rm d}u}{{\rm d}x} \right| _{x=0} $$
		
* 显示：$$ \left. \frac{{\rm d}u}{{\rm d}x} \right| _{x=0} $$
### 如何输入分数
通常使用`\frac {分子} {分母}`命令产生一个分数，分数可嵌套。  
便捷情况可直接输入`\frac ab`来快速生成一个$$$\frac ab$$$。  
如果分式很复杂，亦可使用`分子 \over 分母`命令，此时分数仅有一层。  
* 例子：

		$$\frac{a-1}{b-1} \quad and \quad {a+1\over b+1}$$

* 显示：$$\frac{a-1}{b-1} \quad and \quad {a+1\over b+1}$$
### 如何输入开方
使用`\sqrt [根指数，省略时为2] {被开方数}` 命令输入开方。  
* 例子：

		$$\sqrt{2} \quad and \quad \sqrt[n]{3}$$

显示：$$\sqrt{2} \quad and \quad \sqrt[n]{3}$$
### 如何输入省略号
数学公式中常见的省略号有两种，`\ldots`表示与文本底线对齐的省略号，`\cdots`表示与文本中线对齐的省略号。

* 例子：

		$$f(x_1,x_2,\underbrace{\ldots}_{\rm ldots} ,x_n) = x_1^2 + x_2^2 + \underbrace{\cdots}_{\rm cdots} + x_n^2$$
* 显示：$$f(x_1,x_2,\underbrace{\ldots}\_{\rm ldots},x_n) = x_1^2 + x_2^2 + \underbrace{\cdots}\_{\rm cdots} + x_n^2$$* 
### 如何输入矢量
使用`\vec{矢量}`来自动产生一个矢量。也可以使用`\overrightarrow`等命令自定义字母上方的符号。
* 例子：
		
		$$\vec{a} \cdot \vec{b}=0$$
* 显示：$$\vec{a} \cdot \vec{b}=0$$
* 例子：

		$$\overleftarrow{xy} \quad and \quad \overleftrightarrow{xy} \quad and \quad \overrightarrow{xy}$$

* 显示：$$\overleftarrow{xy} \quad and \quad \overleftrightarrow{xy} \quad and \quad \overrightarrow{xy}$$
### 如何输入积分
使用`\int_积分下限^积分上限 {被积表达式}`来输入一个积分。
* 例子：

		$$\int_0^1 {x^2} \,{\rm d}x$$

* 显示：$$\int_0^1 {x^2} \,{\rm d}x$$
本例中`\,`和`{\rm d}`部分可省略，但建议加入，能使式子更美观。
### 如何输入极限运算
使用`\lim_{变量 \to 表达式} 表达式`来输入一个极限。如有需求，可以更改`\to`符号至任意符号。
* 例子：

		$$ \lim_{n \to +\infty} \frac{1}{n(n+1)} \quad and \quad \lim_{x \to\leftarrow{示例}} \frac{1}{n(n+1)} $$
* 显示：$$ \lim_{n \to +\infty} \frac{1}{n(n+1)} \quad and \quad \lim\_{x\leftarrow{示例}} \frac{1}{n(n+1)} $$
### 如何输入累加、累乘运算
使用`\sum_{下标表达式}^{上标表达式} {累加表达式}`来输入一个累加。  
与之类似，使用`\prod` `\bigcup` `\bigcap` 来分别输入累乘、并集和交集。  
此类符号在行内显示时上下标表达式将会移至右上角和右下角。
* 例子：

		$$\sum_{i=1}^n \frac{1}{i^2} \quad and \quad \prod_{i=1}^n \frac{1}{i^2} \quad and \quad \bigcup_{i=1}^{2} R_i$$

* 显示：$$\sum\_{i=1}^n \frac{1}{i^2} \quad and \quad \prod\_{i=1}^n \frac{1}{i^2} \quad and \quad \bigcup_{i=1}^{2} R_i$$


转载请注明出处。