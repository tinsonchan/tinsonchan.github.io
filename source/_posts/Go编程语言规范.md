---
title: Go编程语言规范
comments: false
categories:
  - golang
date: 2019-04-11 07:20:41
tags:
  - 翻译
---

本文档只是本人在阅读golang语言规范是翻译的非正规文档，一个是学习英语，一个是让自己去阅读官方文档而已。如果有问题，请多多指出。

**版本：2018年11月16日 || 译者：TinsonChan <tinsonchan@163.com>**

<!--more-->


引言&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;下标表达式  
记法&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;切片表达式  
源码的表示&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;类型断言  
&emsp;&emsp;字符&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;调用  
&emsp;&emsp;字母和数字&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;传递实参至...形参  
词法元素&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;操作符  
&emsp;&emsp;注释&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;算数操作符  
&emsp;&emsp;标记&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;比较操作符  
&emsp;&emsp;分号&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;逻辑操作符  
&emsp;&emsp;标识符&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;地址操作符  
&emsp;&emsp;关键字&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;接收操作符  
&emsp;&emsp;运算符与分隔符&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;类型转换  
&emsp;&emsp;整数字面&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;常量表达式  
&emsp;&emsp;浮点数字面&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;求值顺序  
&emsp;&emsp;虚数字面&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;语句  
&emsp;&emsp;符文字面&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;终止语句  
&emsp;&emsp;字符串字面&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;空语句  
常量&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;标签语句  
变量&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;表达式语句  
类型&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;发送语句  
&emsp;&emsp;方法集&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;递增递减语句  
&emsp;&emsp;布尔类型&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;赋值  
&emsp;&emsp;数值类型&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;If语句  
&emsp;&emsp;字符串类型&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Switch语句  
&emsp;&emsp;数组类型&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;For语句  
&emsp;&emsp;切片类型&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Go语句  
&emsp;&emsp;结构类型&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Select语句  
&emsp;&emsp;指针类型&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Return语句  
&emsp;&emsp;函数类型&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Break语句  
&emsp;&emsp;接口类型&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Continue语句  
&emsp;&emsp;映射类型&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Goto语句  
&emsp;&emsp;信道类型&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Fallthrough语句  
类型与值的性质&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Defer语句  
&emsp;&emsp;类型标识&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;内建函数  
&emsp;&emsp;可赋值性&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;关闭  
&emsp;&emsp;可代表性&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;长度与容量  
块&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;分配  
声明与作用域&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;创建切片、映射与信道  
&emsp;&emsp;标签作用域&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;追加与复制切片  
&emsp;&emsp;空白标识符&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;映射元素的删除  
&emsp;&emsp;预声明标识符&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;复数操作  
&emsp;&emsp;可导出标识符&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;恐慌处理  
&emsp;&emsp;标识符的唯一性&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;引导  
&emsp;&emsp;常量声明&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;包  
&emsp;&emsp;Iota&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;源文件的组织   
&emsp;&emsp;类型声明&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;包子句  
&emsp;&emsp;变量声明&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;导入声明  
&emsp;&emsp;短变量声明&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;一个例子包  
&emsp;&emsp;函数声明&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;程序初始化与执行  
&emsp;&emsp;方法声明&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;零值  
表达式&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;包初始化  
&emsp;&emsp;操作数&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;程序执行  
&emsp;&emsp;限定标识符&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;错误  
&emsp;&emsp;复合字面&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;运行时恐慌  
&emsp;&emsp;函数字面&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;系统考虑  
&emsp;&emsp;主表达式&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;包 unsafe  
&emsp;&emsp;选择者&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;大小与对齐保证  
&emsp;&emsp;方法表达式&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;  
&emsp;&emsp;方法值&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;


##Introduction引言


This is a reference manual for the Go programming language. For more information and other documents, see golang.org.  
这是一份关于Go语言的参考手册。欲获取更多信息与文档， 请访问http://golang.org。

Go is a general-purpose language designed with systems programming in mind. It is strongly typed and garbage-collected and has explicit support for concurrent programming. Programs are constructed from packages, whose properties allow efficient management of dependencies.  
Go是通用型编程语言，它为系统编程而设计。它是强类型化的语言，具有垃圾回收机制，并显式支持并发编程。 程序由包构造，以此来提供高效的依赖管理功能。当前实现使用传统的“编译-链接”模型来生成可执行的二进制文件。

The grammar is compact and regular, allowing for easy analysis by automatic tools such as integrated development environments.  
其语法紧凑而规则，便于IDE等自动化工具分析。

## Notation记法 ##

The syntax is specified using Extended Backus-Naur Form (EBNF):  
语法使用扩展巴克斯-诺尔范式（EBNF）定义：

> Production  = production_name "=" [ Expression ] "." .  
Expression  = Alternative { "|" Alternative } .  
Alternative = Term { Term } .  
Term        = production_name | token [ "…" token ] | Group | Option | Repetition .  
Group       = "(" Expression ")" .  
Option      = "[" Expression "]" .  
Repetition  = "{" Expression "}" .


> 生成式 = 生成式名 "=" [ 表达式 ] "." .  
表达式 = 选择项 { "|" 选择项 } .  
选择项 = 条目 { 条目 } .  
条目   = 生成式名 | 标记 [ "…" 标记 ] | 组 | 可选项 | 重复项 .  
组     = "(" 表达式 ")" .  
可选项 = "[" 表达式 "]" .  
重复项 = "{" 表达式 "}" .  

Productions are expressions constructed from terms and the following operators, in increasing precedence:  
生成式由表达式构造，表达式通过术语及以下操作符构造，自上而下优先级递增（低=>高）：

> |   alternation  
()  grouping  
[]  option (0 or 1 times)  
{}  repetition (0 to n times)  

|   选择  
()  分组  
[]  可选 （0 或 1 次）  
{}  重复 （0 到 n 次）  

Lower-case production names are used to identify lexical tokens. Non-terminals are in CamelCase. Lexical tokens are enclosed in double quotes "" or back quotes ``.  
小写生成式名用于确定词法标记。非最终（Non-terminals）词法标记使用驼峰记法（CamelCase）。 置于双引号 "" 或反引号 `` 中的为词法标记。

The form a … b represents the set of characters from a through b as alternatives. The horizontal ellipsis … is also used elsewhere in the spec to informally denote various enumerations or code snippets that are not further specified. The character … (as opposed to the three characters ...) is not a token of the Go language.  
形式 a … b 表示把从 a 到 b 的字符集作为选择项。 横向省略号 … 也在本文档的其它地方非正式地表示各种列举或简略的代码片断。 单个字符 …（不同于三个字符 ...）并非Go语言本身的标记。

## Source code representation 源码的表示##
Source code is Unicode text encoded in UTF-8. The text is not canonicalized, so a single accented code point is distinct from the same character constructed from combining an accent and a letter; those are treated as two code points. For simplicity, this document will use the unqualified term character to refer to a Unicode code point in the source text.  
源码是采用UTF-8编码的Unicode文本。 该文本是非标准化的，因此单一的着重号码点不同于结合了字母与着重号的字符结构， 那些应当视为两个码点。为简单起见，本文档将使用未限定的术语字符在源文本中代替Unicode码点。

Each code point is distinct; for instance, upper and lower case letters are different characters.  
每个码点都是不同的，例如，大写与小写的字母就是不同的字符。

Implementation restriction: For compatibility with other tools, a compiler may disallow the NUL character (U+0000) in the source text.  
实现限制：为兼容其它工具，编译器会阻止字符NUL（U+0000）出现在源码文本中。

Implementation restriction: For compatibility with other tools, a compiler may ignore a UTF-8-encoded byte order mark (U+FEFF) if it is the first Unicode code point in the source text. A byte order mark may be disallowed anywhere else in the source.  
实现限制：为兼容其它工具，若UTF-8编码的字节序标记（U+FEFF）为源文本中的第一个Unicode码点， 编译器就会忽略它。

### Characters 字符 ###

The following terms are used to denote specific Unicode character classes:  
具体的Unicode字符类别由以下术语表示：

> newline        = /* the Unicode code point U+000A */ .  
unicode_char   = /* an arbitrary Unicode code point except newline */ .  
unicode_letter = /* a Unicode code point classified as "Letter" */ .  
unicode_digit  = /* a Unicode code point classified as "Number, decimal digit" */ .  


> 换行符      = /* Unicode码点 U+000A */ .  
unicode字符 = /* 除newline以外的任意Unicode码点 */ .  
unicode字母 = /* 类型为“字母”的Unicode码点 */ .  
unicode数字 = /* 类型为“十进制数字”的Unicode码点 */ .  

In The Unicode Standard 8.0, Section 4.5 "General Category" defines a set of character categories. Go treats all characters in any of the Letter categories Lu, Ll, Lt, Lm, or Lo as Unicode letters, and those in the Number category Nd as Unicode digits.  
在Unicode标准6.2， 章节4.5 “一般类别” 中定义了字符集类别。 其中类别Lu，Ll，Lt，Lm及Lo被视为Unicode字母，类别Nd被视为Unicode数字。

### Letters and digits字母和数字 ###
The underscore character _ (U+005F) is considered a letter.  
下划线字符_（U+005F）被视为一个字母。


> letter        = unicode_letter | "_" .  
decimal_digit = "0" … "9" .  
octal_digit   = "0" … "7" .  
hex_digit     = "0" … "9" | "A" … "F" | "a" … "f" .  


> 字母         = unicode字母 | "_" .  
十进制数字   = "0" … "9" .  
八进制数字   = "0" … "7" .  
十六进制数字 = "0" … "9" | "A" … "F" | "a" … "f" .  


## Lexical elements 词法元素 ##
###Comments 注释 ###
Comments serve as program documentation. There are two forms:  
注释有两种形式：

1. Line comments start with the character sequence // and stop at the end of the line.
General comments start with the character sequence /* and stop with the first subsequent character sequence */.  
行注释 以// 开始，至行尾结束。一条行注释视为一个换行符。
2. A comment cannot start inside a rune or string literal, or inside a comment. A general comment containing no newlines acts like a space. Any other comment acts like a newline.  
 块注释 以 /* 开始，至 */ 结束。 块注释在包含多行时视为一个换行符，否则视为一个空格。

### Tokens标记###
Tokens form the vocabulary of the Go language. There are four classes: identifiers, keywords, operators and punctuation, and literals. White space, formed from spaces (U+0020), horizontal tabs (U+0009), carriage returns (U+000D), and newlines (U+000A), is ignored except as it separates tokens that would otherwise combine into a single token. Also, a newline or end of file may trigger the insertion of a semicolon. While breaking the input into tokens, the next token is the longest sequence of characters that form a valid token.  
标记构成Go语言的词汇。它有4种类型：标识符，关键字， 运算符与分隔符以及字面。空白符由空格（U+0020）， 横向制表符（U+0009），回车符（U+000D）和换行符（U+000A）组成，除非用它来分隔会结合成单个的标记， 否则它将被忽略。此外，换行符或EOF（文件结束符）会触发分号的插入。 当把输入分解为标记时，可形成有效标记的最长字符序列将作为下一个标记。

### Semicolons分号###
The formal grammar uses semicolons ";" as terminators in a number of productions. Go programs may omit most of these semicolons using the following two rules:  
正式语法使用分号 ";" 作为一些生成式的终止符。Go程序会使用以下两条规则来省略大多数分号：

1. When the input is broken into tokens, a semicolon is automatically inserted into the token stream immediately after a line's final token if that token is
	+ an identifier  
	 标识符
	+ an integer, floating-point, imaginary, rune, or string literal  
	整数， 浮点数， 虚数， 符文或 字符串 字面
	+ one of the keywords break, continue, fallthrough, or return  
	关键字 break， continue， fallthrough 或 return
	+ one of the operators and punctuation ++, --, ), ], or }
	运算符与分隔符 ++， --， )， ]或 }
2. To allow complex statements to occupy a single line, a semicolon may be omitted before a closing ")" or "}".  
为允许复合语句占据单行，闭合的 ")" 或 "}" 之前的分号可以省略。

To reflect idiomatic use, code examples in this document elide semicolons using these rules.  
为符合习惯用法，本文档中的代码示例将使用这些规则省略分号。

### Identifiers 标识符###
Identifiers name program entities such as variables and types. An identifier is a sequence of one or more letters and digits. The first character in an identifier must be a letter.  
标识符被用来命名程序实体，例如变量和类型。一个标识符由一个或多个字母和数字组成。 标识符的第一个字符必须是字母。

> identifier = letter { letter | unicode_digit } .  
> 标识符 = 字母 { 字母 | unicode数字 } .
> 
> a  
> _x9  
> ThisVariableIsExported  
> αβ  

Some identifiers are predeclared.  
有些标识符是预声明的。

###Keywords 关键字 ###
The following keywords are reserved and may not be used as identifiers.  
以下为保留关键字，不能用作标识符。
> break&emsp;&emsp;default&emsp;&emsp;func&emsp;&emsp;interface&emsp;&emsp;select  
> case&emsp;&emsp;defer&emsp;&emsp;&emsp;go&emsp;&emsp;&emsp;map&emsp;&emsp;&emsp;&emsp;struct  
> chan&emsp;&emsp;else&emsp;&emsp;&emsp;goto&emsp;&emsp;&emsp;package&emsp;&emsp;switch  
> const&emsp;&emsp;fallthrough  if&emsp;&emsp;&emsp;&emsp;range&emsp;&emsp;&emsp;type  
> continue&emsp;for&emsp;&emsp;&emsp;import&emsp;&emsp;return&emsp;&emsp;&emsp;var

### Operators and punctuation 运算符与分隔符###
The following character sequences represent operators (including assignment operators) and punctuation:  
以下字符序列表示运算符，分隔符和其它特殊标记：

> \+    &     +=    &=     &&    ==    !=    (    )  
> -    |     -=    |=     ||    <     <=    [    ]  
> *    ^     *=    ^=     <-        >=    {    }  
> /    <<    /=    <<=    ++    =     :=    ,    ;  
> %      %=    >>=    --    !     ...   .    :  
>      &^          &^=

### Integer literals 整数字面###
An integer literal is a sequence of digits representing an integer constant. An optional prefix sets a non-decimal base: 0 for octal, 0x or 0X for hexadecimal. In hexadecimal literals, letters a-f and A-F represent values 10 through 15.  
整数字面由数字序列组成，代表整数常量 。非十进制数由这些前缀定义： 0 为八进制数前缀，0x 或 0X为十六进制数前缀。 在十六进制数字面中，字母 a-f 或 A-F 表示值10到15。

> int_lit     = decimal_lit | octal_lit | hex_lit .  
> decimal_lit = ( "1" … "9" ) { decimal_digit } .  
> octal_lit   = "0" { octal_digit } .  
> hex_lit     = "0" ( "x" | "X" ) hex_digit { hex_digit } .  

> 整数字面       = 十进制数字面 | 八进制数字面 | 十六进制数字面 .  
> 十进制数字面   = ( "1" … "9" ) { 十进制数字 } .  
> 八进制数字面   = "0" { 八进制数字 } .  
> 十六进制数字面 = "0" ( "x" | "X" ) 十六进制数字 { 十六进制数字 } .  
> 
> 42  
> 0600  
> 0xBadFace  
> 170141183460469231731687303715884105727  

### Floating-point literals 浮点数字面 ###
A floating-point literal is a decimal representation of a floating-point constant. It has an integer part, a decimal point, a fractional part, and an exponent part. The integer and fractional part comprise decimal digits; the exponent part is an e or E followed by an optionally signed decimal exponent. One of the integer part or the fractional part may be elided; one of the decimal point or the exponent may be elided.  
浮点数字面由十进制浮点常量表示。 它由整数部分，小数点，小数部分和指数部分构成。整数部分与小数部分由十进制数字组成； 指数部分由一个 e 或 E 紧跟一个带可选正负号的十进制指数构成。 整数部分或小数部分可以省略；小数点或指数亦可省略。

> float_lit = decimals "." [ decimals ] [ exponent ] |  
>&emsp;&emsp;&emsp;&emsp;decimals exponent |  
>&emsp;&emsp;&emsp;&emsp;"." decimals [ exponent ] .  
> decimals  = decimal_digit { decimal_digit } .  
> exponent  = ( "e" | "E" ) [ "+" | "-" ] decimals .  

> 浮点数字面 = 十进制数 "." [ 十进制数 ] [ 指数 ] |  
>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;十进制指数 |  
>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;"." 十进制数 [ 指数 ] .  
> 十进制数   = 十进制数字 { 十进制数字 } .  
> 指数       = ( "e" | "E" ) [ "+" | "-" ] 十进制数 .  
> 
> \0.  
> 72.40  
> 072.40  // == 72.40  
> 2.71828  
> 1.e+0  
> 6.67428e-11  
> 1E6  
> .25  
> .12345E+5  

### Imaginary literals虚数字面###
An imaginary literal is a decimal representation of the imaginary part of a complex constant. It consists of a floating-point literal or decimal integer followed by the lower-case letter i.  
虚数字面由十进制复数常量的虚部表示。 它由浮点数字面或十进制整数紧跟小写字母 i 构成。

> imaginary_lit = (decimals | float_lit) "i" .  
> 虚数字面 = (十进制数 | 浮点数字面) "i" .
> 
> 0i  
> 011i  // == 11i  
> 0.i  
> 2.71828i  
> 1.e+0i  
> 6.67428e-11i  
> 1E6i  
> .25i  
> .12345E+5i  

### Rune literals符文字面###
A rune literal represents a rune constant, an integer value identifying a Unicode code point. A rune literal is expressed as one or more characters enclosed in single quotes, as in 'x' or '\n'. Within the quotes, any character may appear except newline and unescaped single quote. A single quoted character represents the Unicode value of the character itself, while multi-character sequences beginning with a backslash encode values in various formats.  
符文字面由一个符文常量表示，一个整数值确定一个Unicode码点。 一个符文字面由围绕在单引号中的一个或更多字符表示。通常一个Unicode码点作为一个或更多字符围绕在单引号中。 在引号内，除单引号和换行符外的任何字符都可出现。引号内的单个字符通常代表该字符的Unicode值自身， 而以反斜杠开头的多字符序列则会编码为不同的形式。

The simplest form represents the single character within the quotes; since Go source text is Unicode characters encoded in UTF-8, multiple UTF-8-encoded bytes may represent a single integer value. For instance, the literal 'a' holds a single byte representing a literal a, Unicode U+0061, value 0x61, while 'ä' holds two bytes (0xc3 0xa4) representing a literal a-dieresis, U+00E4, value 0xe4.  
最简单的形式就是引号内只有一个字符；由于Go源码文本是UTF-8编码的Unicode字符， 多个UTF-8编码的字节就可以表示一个单一的整数值。例如，字面 'a' 包含一个字节， 表示一个字面 a，Unicode字符U+0061，值 0x61，而 'ä' 则包含两个字节（0xc3 0xa4），表示一个字面 分音符-a， Unicode字符U+00E4，值 0xe4。

Several backslash escapes allow arbitrary values to be encoded as ASCII text. There are four ways to represent the integer value as a numeric constant: \x followed by exactly two hexadecimal digits; \u followed by exactly four hexadecimal digits; \U followed by exactly eight hexadecimal digits, and a plain backslash \ followed by exactly three octal digits. In each case the value of the literal is the value represented by the digits in the corresponding base.  
反斜杠转义允许用ASCII文本来编码任意值。将整数值表示为数字常量有4种方法： \x 紧跟2个十六进制数字；\u 紧跟4个十六进制数字； \U 紧跟8个十六进制数字；单个\紧跟3个八进制数字。 在任何情况下，字面的值都是其相应进制的数字表示的值。

Although these representations all result in an integer, they have different valid ranges. Octal escapes must represent a value between 0 and 255 inclusive. Hexadecimal escapes satisfy this condition by construction. The escapes \u and \U represent Unicode code points so within them some values are illegal, in particular those above 0x10FFFF and surrogate halves.  
尽管这些表示方式都会产生整数，其有效范围却不相同。八进制转义只能表示 0 到 255 之间的值。 十六进制转义则视其结构而定。转义符 \u 和 \U 表示Unicode码点， 因此其中的一些值是非法的，特别是那些大于 0x10FFFF 的值和半替代值。

After a backslash, certain single-character escapes represent special values:  
在反斜杠后，某些单字符转义表示特殊值：

> \a   U+0007 alert or bell  
> \b   U+0008 backspace  
> \f   U+000C form feed  
> \n   U+000A line feed or newline  
> \r   U+000D carriage return  
> \t   U+0009 horizontal tab  
> \v   U+000b vertical tab  
> \\   U+005c backslash  
> \'   U+0027 single quote  (valid escape only within rune literals)  
> \"   U+0022 double quote  (valid escape only within string literals)  

> \a   U+0007 警报或铃声  
> \b   U+0008 退格  
> \f   U+000C 换页  
> \n   U+000A 换行  
> \r   U+000D 回车  
> \t   U+0009 横向制表  
> \v   U+000b 纵向制表  
> \\   U+005c 反斜杠  
> \'   U+0027 单引号(仅在符文字面中有效)  
> \"   U+0022 双引号(仅在字符串字面中有效)  

All other sequences starting with a backslash are illegal inside rune literals.  
在符文字面中，所有其它以反斜杠开始的序列都是非法的。

> rune_lit         = "'" ( unicode_value | byte_value ) "'" .  
> unicode_value    = unicode_char | little_u_value | big_u_value | escaped_char .  
> byte_value       = octal_byte_value | hex_byte_value .  
> octal_byte_value = `\` octal_digit octal_digit octal_digit .  
> hex_byte_value   = `\` "x" hex_digit hex_digit .  
> little_u_value   = `\` "u" hex_digit hex_digit hex_digit hex_digit .  
> big_u_value      = `\` "U" hex_digit hex_digit hex_digit hex_digit  
>                            hex_digit hex_digit hex_digit hex_digit .  
> escaped_char     = `\` ( "a" | "b" | "f" | "n" | "r" | "t" | "v" | `\` | "'" | `"` ) .  
> 
符文字面       = "'" ( unicode值 | 字节值 ) "'" .  
unicode值      = unicode字符 | 小Unicode值 | 大Unicode值 | 转义字符 .  
字节值         = 八进制字节值 | 十六进制字节值 .  
八进制字节值   = `\` 八进制数字 八进制数字 八进制数字 .  
十六进制字节值 = `\` "x" 十六进制数字 十六进制数字 .  
小Unicode值    = `\` "u" 十六进制数字 十六进制数字 十六进制数字 十六进制数字 .  
大Unicode值    = `\` "U" 十六进制数字 十六进制数字 十六进制数字 十六进制数字  
						 十六进制数字 十六进制数字 十六进制数字 十六进制数字 .  
转义字符       = `\` ( "a" | "b" | "f" | "n" | "r" | "t" | "v" | `\` | "'" | `"` ) .  
> 
> 'a'  
> 'ä'  
> '本'  
> '\t'  
> '\000'  
> '\007'  
> '\377'  
> '\x07'  
> '\xff'  
> '\u12e4'  
> '\U00101234'  
> '\''         // rune literal containing single quote character  
> 'aa'         // illegal: too many characters  
> '\xa'        // illegal: too few hexadecimal digits  
> '\0'         // illegal: too few octal digits  
> '\uDFFF'     // illegal: surrogate half  
> '\U00110000' // illegal: invalid Unicode code point

### String literals字符串字面###
A string literal represents a string constant obtained from concatenating a sequence of characters. There are two forms: raw string literals and interpreted string literals.  
字符串字面表示字符串常量，可通过连结字符序列获得。 它有两种形式：原始字符串字面和解译字符串字面。

Raw string literals are character sequences between back quotes, as in `foo`. Within the quotes, any character may appear except back quote. The value of a raw string literal is the string composed of the uninterpreted (implicitly UTF-8-encoded) characters between the quotes; in particular, backslashes have no special meaning and the string may contain newlines. Carriage return characters ('\r') inside raw string literals are discarded from the raw string value.  
原始字符串字面为反引号 `` 之间的字符序列。在该引号内， 除反引号外的任何字符都是合法的。原始字符串字面的值为此引号之间的无解译（隐式UTF-8编码的）字符组成的字符串； 另外，反斜杠没有特殊意义且字符串可包含换行符。原始字符串字面中的回车符将会从原始字符串的值中丢弃。

Interpreted string literals are character sequences between double quotes, as in "bar". Within the quotes, any character may appear except newline and unescaped double quote. The text between the quotes forms the value of the literal, with backslash escapes interpreted as they are in rune literals (except that \' is illegal and \" is legal), with the same restrictions. The three-digit octal (\nnn) and two-digit hexadecimal (\xnn) escapes represent individual bytes of the resulting string; all other escapes represent the (possibly multi-byte) UTF-8 encoding of individual characters. Thus inside a string literal \377 and \xFF represent a single byte of value 0xFF=255, while ÿ, \u00FF, \U000000FF and \xc3\xbf represent the two bytes 0xc3 0xbf of the UTF-8 encoding of character U+00FF.  
解译字符串字面为双引号 "" 之间的字符序列。 在该引号内，不包含换行符的文本形成字面的值，反斜杠转义序列如同在符文字面中一样以相同的限制被解译 （其中\'是非法的，而 \" 是合法的）。 3位八进制（\nnn）和2位十六进制（\xnn） 转义表示字符串值的独立 字节 ；其它转义符表示（可多字节）UTF-8编码的独立 字符。 因此在字符串字面中，\377 和 \xFF 表示值为 0xFF=255的单个字节， 而ÿ、\u00FF、\U000000FF 和 \xc3\xbf 则表示UTF-8编码的字符U+00FF的两个字节0xc3 0xbf。

> string_lit             = raw_string_lit | interpreted_string_lit .  
> raw_string_lit         = "`" { unicode_char | newline } "`" .  
> interpreted_string_lit = `"` { unicode_value | byte_value } `"` .  
> 字符串字面     = 原始字符串字面 | 解译字符串字面 .  
原始字符串字面 = "`" { unicode字符 | 换行符 } "`" .  
解译字符串字面 = `"` { unicode值 | 字节值 } `"` .  

> `abc`                // same as "abc"  
> `\n  
> \n`                  // same as "\\n\n\\n"  
> "\n"  
> "\""                 // same as `"`  
> "Hello, world!\n"  
> "日本語"  
> "\u65e5本\U00008a9e"  
> "\xff\u00FF"  
> "\uD800"             // illegal: surrogate half  
> "\U00110000"         // illegal: invalid Unicode code point  

These examples all represent the same string:  
这些例子都表示相同的字符串：
> "日本語"                                 // UTF-8 input text  
> `日本語`                                 // UTF-8 input text as a raw literal  
> "\u65e5\u672c\u8a9e"                    // the explicit Unicode code points  
> "\U000065e5\U0000672c\U00008a9e"        // the explicit Unicode code points  
> "\xe6\x97\xa5\xe6\x9c\xac\xe8\xaa\x9e"  // the explicit UTF-8 bytes  

If the source code represents a character as two code points, such as a combining form involving an accent and a letter, the result will be an error if placed in a rune literal (it is not a single code point), and will appear as two code points if placed in a string literal.  
如果源码将两个码点表示为一个字符，例如包含着重号和字母的结合形式， 那么将它放置在符文字面中就会产生一个错误（它不是单一码点），而放置在字符串字面中则会显示为两个码点。
