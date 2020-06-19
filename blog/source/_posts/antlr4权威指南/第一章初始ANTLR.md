---
title: 第一章初始ANTLR
date: 2020-06-19 11:15:55
tags: antlr4权威指南
categories: antlr4权威指南
---

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

## 设置别名

alias antlr4='java -jar /usr/local/Cellar/antlr/4.8_1/antlr-4.8-complete.jar'

## 写入/usr/local/bin

java -cp "/usr/local/Cellar/antlr/4.8_1/antlr-4.8-complete.jar:$CLASSPATH" org.antlr.v4.Tool $*

输入antlr4

输出如下信息证明成功了：

```java
ANTLR Parser Generator  Version 4.8
 -o ___              specify output directory where all output is generated
 -lib ___            specify location of grammars, tokens files
 -atn                generate rule augmented transition network diagrams
 -encoding ___       specify grammar file encoding; e.g., euc-jp
 -message-format ___ specify output style for messages in antlr, gnu, vs2005
 -long-messages      show exception details when available for errors and warnings
 -listener           generate parse tree listener (default)
 -no-listener        don't generate parse tree listener
 -visitor            generate parse tree visitor
 -no-visitor         don't generate parse tree visitor (default)
 -package ___        specify a package/namespace for the generated code
 -depend             generate file dependencies
 -D<option>=value    set/override a grammar-level option
 -Werror             treat warnings as errors
 -XdbgST             launch StringTemplate visualizer on generated code
 -XdbgSTWait         wait for STViz to close before continuing
 -Xforce-atn         use the ATN simulator for all predictions
 -Xlog               dump lots of logging info to antlr-timestamp.log
 -Xexact-output-dir  all output goes into -o dir regardless of paths/package
(base) xuejinhuideMac-mini:bin ND$ antlr4
ANTLR Parser Generator  Version 4.8
 -o ___              specify output directory where all output is generated
 -lib ___            specify location of grammars, tokens files
 -atn                generate rule augmented transition network diagrams
 -encoding ___       specify grammar file encoding; e.g., euc-jp
 -message-format ___ specify output style for messages in antlr, gnu, vs2005
 -long-messages      show exception details when available for errors and warnings
 -listener           generate parse tree listener (default)
 -no-listener        don't generate parse tree listener
 -visitor            generate parse tree visitor
 -no-visitor         don't generate parse tree visitor (default)
 -package ___        specify a package/namespace for the generated code
 -depend             generate file dependencies
 -D<option>=value    set/override a grammar-level option
 -Werror             treat warnings as errors
 -XdbgST             launch StringTemplate visualizer on generated code
 -XdbgSTWait         wait for STViz to close before continuing
 -Xforce-atn         use the ATN simulator for all predictions
 -Xlog               dump lots of logging info to antlr-timestamp.log
 -Xexact-output-dir  all output goes into -o dir regardless of paths/package
```

