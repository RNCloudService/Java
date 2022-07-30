# JDK和JRE

**JDK**

Java 2 SDK (Development Kit)包含：JRE的超集，包含编译器和调试器等用于程序开发的文件

**JRE**

Java Runtime Environment (JRE) 包含：Java虚拟机、库函数、运行Java应用程序和Applet所必须文件

Java运行环境的三项主要功能：

加载代码：由class loader 完成；

校验代码：由bytecode verififier 完成；

执行代码：由 runtime interpreter完成。

**区别和联系**：

 sdk（也就是jdk）是jre的超集，是在jre的基础上增加了编译器及其他一些开发工具。

 jre就是java运行时环境，包括了jvm和其它一些java核心api,任何一台电脑，只有安装了jre才可以运行

java程序.

如果只是要运行JAVA程序，之需要JRE就可以。 JRE通常非常小，也包含了JVM.

如果要开发JAVA程序，就需要安装JDK。 

