---
title: Antlr
date: 2020-06-17 15:53:03
tags: 编译原理
categories: 编译原理
---

# 初识Antlr

之前我们写过一些词法分析器，但是这些只能确实demo。而且要把所有的规则都扩展需要很多工作量。接下来我们使用工具生成词法分析器和语法分析器，同时生成的代码比较专业的代码。Antlr 是一个开源的工具，支持根据规则文件生成词法分析器和语法分析器，它自身是用 Java 实现的。

# 安装Antlr

1.安装  brew install antlr(可能需要等一会，不过最后可以成功)

2.cd ~/

3.创建.bash_profile文件：(如果存在.bash_profile,直接跳第四步)

touch .bash_profile

4.打开.bash_profile并编辑：

open .bash_profile

5.这样就打开了一个记事本，会显示你之前配置过的path，修改记事本，一定要先备份，然后根据自己需要配置。

6.设置环境变量  export CLASSPATH=".:/usr/local/Cellar/antlr/4.8_1/antlr-4.8-complete.jar:$CLASSPATH"(添加到文件的前面点可以成功，添加后面不确定)

7.command+s保存关闭文件(不要关闭终端)

8.返回终端,使修改后的配置生效命令：

source .bash_profile

修改了.bash_profile的配置,就会导致很多内置命令不能使用,会出现command not found 甚至连rm、cd、vi这些命令都不能使用,所以在今后的修改重要文件的过程中,首先要备份这个文件,然后再对文件进行修改添加.

9.测试:终端输入antlr，可以输出比较正常的内容一般就是成功了。

# 用 Antlr 生成词法分析器

我们创建一个 Hello.g4 文件，用于保存词法规则，然后把之前用过的一些词法规则写进去。

```java
lexer grammar Hello;  //lexer关键字意味着这是一个词法规则文件，名称是Hello，要与文件名相同

//关键字
If :               'if';
Int :              'int';

//字面量
IntLiteral:        [0-9]+;
StringLiteral:      '"' .*? '"' ;  //字符串字面量

//操作符
AssignmentOP:       '=' ;    
RelationalOP:       '>'|'>='|'<' |'<=' ;    
Star:               '*';
Plus:               '+';
Sharp:              '#';
SemiColon:          ';';
Dot:                '.';
Comm:               ',';
LeftBracket :       '[';
RightBracket:       ']';
LeftBrace:          '{';
RightBrace:         '}';
LeftParen:          '(';
RightParen:         ')';

//标识符
Id :                [a-zA-Z_] ([a-zA-Z_] | [0-9])*;

//空白字符，抛弃
Whitespace:         [ \t]+ -> skip;
Newline:            ( '\r' '\n'?|'\n')-> skip;
```

接下来，我们来编译词法规则，在终端中输入命令：

```java
antlr Hello.g4
```

这个命令是让 Antlr 编译规则文件，并生成 Hello.java 文件和其他两个辅助文件。你可以打开看一看文件里面的内容。接着，我用下面的命令编译 Hello.java：

```java
javac *.java
```

接下来，我们来写个脚本文件，让生成的词法分析器解析一下：

```java
int age = 45;
if (age >= 17+8+20){
  printf("Hello old man!");
}
```

我们将上面的脚本存成 hello.play 文件，然后在终端输入下面的命令：

```
grun Hello tokens -tokens hello.play
```

grun 命令实际上是调用了我们刚才生成的词法分析器，即 Hello 类，打印出对 hello.play 词法分析的结果：

```java
(base) xuejinhuideMac-mini:1 ND$ grun Hello tokens -tokens hello.play
[@0,1:3='int',<'int'>,2:0]
[@1,5:7='age',<Id>,2:4]
[@2,9:9='=',<'='>,2:8]
[@3,11:12='45',<IntLiteral>,2:10]
[@4,13:13=';',<';'>,2:12]
[@5,15:16='if',<'if'>,3:0]
[@6,18:18='(',<'('>,3:3]
[@7,19:21='age',<Id>,3:4]
[@8,23:24='>=',<RelationalOP>,3:8]
[@9,26:27='17',<IntLiteral>,3:11]
[@10,28:28='+',<'+'>,3:13]
[@11,29:29='8',<IntLiteral>,3:14]
[@12,30:30='+',<'+'>,3:15]
[@13,31:32='20',<IntLiteral>,3:16]
[@14,33:33=')',<')'>,3:18]
[@15,34:34='{',<'{'>,3:19]
[@16,38:43='printf',<Id>,4:2]
[@17,44:44='(',<'('>,4:8]
[@18,45:60='"Hello old man!"',<StringLiteral>,4:9]
[@19,61:61=')',<')'>,4:25]
[@20,62:62=';',<';'>,4:26]
[@21,64:64='}',<'}'>,5:0]
[@22,65:64='<EOF>',<EOF>,5:1]

```

从结果中看到，我们的词法分析器把每个 Token 都识别了，还记录了它们在代码中的位置、文本值、类别。上面这些都是 Token 的属性。以第二行[@1, 4:6=‘age’,< Id >,1:4]为例，其中 @1 是 Token 的流水编号，表明这是 1 号 Token；4:6 是 Token 在字符流中的开始和结束位置；age 是文本值，Id 是其 Token 类别；最后的 1:4 表示这个 Token 在源代码中位于第 1 行、第 4 列。

[代码详见](https://github.com/xuejh/complicationBook/tree/master/06/1)

## 练习二

这个例子是将例如`"hello abc"`的字符串解析为抽象语法树。

在目录下新建语法文件`Hello.g4`：

```java
// Define a grammar called Hello
grammar Hello;
r  : 'hello' ID ;         // match keyword hello followed by an identifier
ID : [a-z]+ ;             // match lower-case identifiers
WS : [ \t\r\n]+ -> skip ; // skip spaces, tabs, newlines
```

接着运行ANTLR，根据语法文件`Hello.g4`生成对应的词法分析和语法分析程序：

```java
antlr Hello.g4
```

这时候文件夹下会多出几个文件（只选择典型的2个文件说明）

```java
HelloLexer.java 词法分析
HelloParser.java 语法分析
```

然后利用javac对Java文件进行编译：

```java
javac Hello*.java
```

### 测试

首先是词法分析测试。启动`grun`，测试之前的语法：

```java
$ grun Hello r -tokens
hello parrt      # 输入要被识别的语句
EOF              # 在UNIX/MAC系统上输入 Ctrl+D 来输入文件结束符
```

下面就会显示词法分析结果：

```java
[@0,0:4='hello',<'hello'>,1:0]
[@1,6:10='parrt',<ID>,1:6]
[@2,12:11='<EOF>',<EOF>,2:0]
```

然后进行语法分析测试。

```java
$ grun Hello r -tree
hello parrt
EOF
```

显示

```java
(r hello parrt)
```

还可以将语法树可视化：

```java
$ grun Hello r -gui
hello parrt
EOF
```

接着会弹出一个对话框。

![hello-parrt](https://fz-cs.101.com/v0.1/download?dentryId=833aab6a-4b85-4e04-beb6-3c727036a25e&size=480&ext=jpg&token=im_disk:BKG5OJg63T7xFIm5xJvb:FeKZDvlng7WeLpJ1RndzD-a5DiM&expireAt=1592485193516&policy=eyJkZW50cnlJZCI6IjgzM2FhYjZhLTRiODUtNGUwNC1iZWI2LTNjNzI3MDM2YTI1ZSIsInVpZCI6ODcxMDMwLCJyb2xlIjoidXNlciIsInBvbGljeVR5cGUiOiJkb3dubG9hZCJ9)

[完整代码](https://github.com/xuejh/complicationBook/tree/master/06/2)

## 练习三

这一次的文件名叫做 PlayScript.g4。playscript 是为我们的脚本语言起的名称，文件开头是这样的：

```java
grammar PlayScript;
import CommonLexer;   //导入词法定义

/*下面的内容加到所生成的Java源文件的头部，如包名称，import语句等。*/
@header {
package antlrtest;
}
```

然后把之前做过的语法定义放进去。Antlr 内部有自动处理左递归的机制，你可以放心大胆地把语法规则写成下面的样子：

```java
expression
    :   assignmentExpression
    |   expression ',' assignmentExpression
    ;

assignmentExpression
    :   additiveExpression
    |   Identifier assignmentOperator additiveExpression
    ;

assignmentOperator
    :   '='
    |   '*='
    |  '/='
    |   '%='
    |   '+='
    |   '-='
    ;

additiveExpression
    :   multiplicativeExpression
    |   additiveExpression '+' multiplicativeExpression
    |   additiveExpression '-' multiplicativeExpression
    ;

multiplicativeExpression
    :   primaryExpression
    |   multiplicativeExpression '*' primaryExpression
    |   multiplicativeExpression '/' primaryExpression
    |   multiplicativeExpression '%' primaryExpression
    ;
```

我们继续运行下面的命令，生成语法分析器：

```java
antlr PlayScript.g4
javac *.java
```

然后测试一下生成的语法分析器：

```java
grun antlrtest.PlayScript expression -gui
```

报错

```java
(base) xuejinhuideMac-mini:antlrtest ND$ grun antlrtest.PlayScript expression -gui
Can't load antlrtest.PlayScript as lexer or parser
```

这一步报错，始终没找到原因

这个命令的意思是：测试 PlayScript 这个类的 expression 方法，也就是解析表达式的方法，结果用图形化界面显示。

我们在控制台界面中输入下面的内容：

```java
age + 10 * 2  + 10
^D
```

其中 ^D 是按下 Ctl 键的同时按下 D，相当于在终端输入一个 EOF 字符，即文件结束符号（Windows 操作系统要使用 ^Z）。当然，你也可以提前把这些语句放到文件中，把文件名作为命令参数。之后，语法分析器会分析这些语法，并弹出一个窗口来显示 AST：

![parseTreeInspector](https://fz-cs.101.com/v0.1/download?dentryId=419d3dd3-64a5-47d1-b624-dfe89669ef7c&size=480&ext=jpg&token=im_disk:BKG5OJg63T7xFIm5xJvb:HqsQe4WBhLrieRCi_tJOM0OfqGk&expireAt=1592486422268&policy=eyJkZW50cnlJZCI6IjQxOWQzZGQzLTY0YTUtNDdkMS1iNjI0LWRmZTg5NjY5ZWY3YyIsInVpZCI6ODcxMDMwLCJyb2xlIjoidXNlciIsInBvbGljeVR5cGUiOiJkb3dubG9hZCJ9)

看得出来，AST 完全正确，优先级和结合性也都没错。所以，Antlr 生成的语法分析器还是很靠谱的。以后，你专注写语法规则就行了，可以把精力放在语言的设计和应用上。

[完整代码](https://github.com/xuejh/complicationBook/tree/master/06/3)

# 参考

[Mac安装Antlr](https://www.jianshu.com/p/7969d076183f)

[Mac修改.bash_profile后补救方法](https://www.jianshu.com/p/1d1ed2e949c2)

[Antlr4 入门](https://www.jianshu.com/p/b7cee507b967)

这个是本人学习极客时间课程[《编译原理之美》](http://gk.link/a/10j0x)的笔记记录。详情请参考极客时间[《编译原理之美》](http://gk.link/a/10j0x)课程。

