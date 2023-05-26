# Tcl
Tcl（Tool command language）为创建集成应用提供了强大的平台，可以将多种多样的应用、协议、设备和框架集成在一起。当与Tk toolkit配合使用时，Tcl可以提供快速以及强大的方式来创建GUI应用，可以运行在unix、mac的PC上。Tcl也被应用于各类web相关的任务以及为应用创建命令语言。Tcl是一门解释性语言，是基于字符串的命令语言，基础结构与语法非常简单，易于学习与掌握。tcl/tk可以跨平台运行，并且与c/c++有良好的兼容性。

## tcl安装
在windows下安装可以下载安装包，解压放在需要的目录下，并将tcl目录下的bin加入到path环境变量。安装包可以在`https://github.com/teclab-at/tcltk`选择所需的版本下载。linux下好像默认已经安装了tcl，如果没有可以通过命令`apt-get install tcl`.tcl是解释性语言，因此可以打开交互式命令行，tclsh，来输入指令，也可以通过创建.tcl文件通过tclsh xx.tcl执行该文件。tcl安装会默认安装tk，可以用来开发GUI界面程序。

## tcl语法
- 1、puts，用来输出字符串，如果字符串之间没有间隔可以直接用`puts xxxxx`，如果有空格则需要用双引号或中括号包括。
- 2、变量创建与赋值，在tcl中所有的数据格式都是字符串，通过set命令给变量赋值，set需要两个参数，第二个参数为内存存储的数据，第一个数据为指向该内存数据的引用。set返回值为第二个参数。变量的使用需要在变量名称前添加$符号，`puts $name`
```sh
set name djxc
set age 12
puts $name
puts "$name is $age years old."
```
- 3、数学运算，tcl支持大部分的数学运算，需要通过expr命令执行


## tk
tk（）是tcl重要的扩展，可以提供视图功能。运行wish命令会在弹出两个界面，一个为终端一个为wish，终端为用户需要输入指令的地方，wish则时指令执行结果的界面。一些指令为创建一个按钮，名称为.b，显示内容为-text值，背景为蓝色。该指令执行完成wish界面并没有显示，需要调用grid命令绘制结果.当然以上命令也可以在tclsh中输入，需要首先引入tk包，`package require tk`.
```sh
button .b -text "dj xc" -background blue
grid .b
```