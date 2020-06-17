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



# 参考

[Mac安装Antlr](https://www.jianshu.com/p/7969d076183f)

[Mac修改.bash_profile后补救方法](https://www.jianshu.com/p/1d1ed2e949c2)

这个是本人学习极客时间课程[《编译原理之美》](http://gk.link/a/10j0x)的笔记记录。详情请参考极客时间[《编译原理之美》](http://gk.link/a/10j0x)课程。

