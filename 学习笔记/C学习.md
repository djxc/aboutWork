# C语言学习
- 1、很多工具都是由C编写，要了解使用这些工具，或开发自己的工具，需要学习C语言。

## vscode调试C
需要安装GCC，vscode插件，然后

## 一、Lua语言
- 1、Lua语言是用C编写，实现简单，可以用来学习编译原理。  
- 1.1Lua程序结构  
&emsp;&emsp;lmathlib.c, lstrlib.c: get familiar with the external C API. Don't bother with the pattern matcher though. Just the easy functions.  
&emsp;&emsp;lapi.c: Check how the API is implemented internally. Only skim this to get a feeling for the code. Cross-reference to lua.h and luaconf.h as needed.  
&emsp;&emsp;lobject.h: tagged values and object representation. skim through this first. you'll want to keep a window with this file open all the time.  
&emsp;&emsp;lstate.h: state objects. ditto.  
&emsp;&emsp;lopcodes.h: bytecode instruction format and opcode definitions. easy.  
&emsp;&emsp;lvm.c: scroll down to luaV_execute, the main interpreter loop. see how all of the instructions are implemented. skip the details for now. reread later.  
&emsp;&emsp;ldo.c: calls, stacks, exceptions, coroutines. tough read.  
&emsp;&emsp;lstring.c: string interning. cute, huh?  
&emsp;&emsp;ltable.c: hash tables and arrays. tricky code.  
&emsp;&emsp;ltm.c: metamethod handling, reread all of lvm.c now.  
&emsp;&emsp;You may want to reread lapi.c now.
&emsp;&emsp;ldebug.c: surprise waiting for you. abstract interpretation is used to find object names for tracebacks. does bytecode verification, too.  
&emsp;&emsp;lparser.c, lcode.c: recursive descent parser, targetting a register-based VM. start from chunk() and work your way through. read the expression parser and the code generator parts last.  
&emsp;&emsp;lgc.c: incremental garbage collector. take your time.  
&emsp;&emsp;Read all the other files as you see references to them. Don't let your stack get too deep though.


## 二、zlib
&emsp;&emsp;zlib是一个数据压缩库，如果数据量小于内存可以一次性压缩，否则需要循环压缩。当前zlib版本1.2.13仅支持一种压缩算法deflation。zlib压缩之后的数据格式为zlib格式，RFC 1950，被deflate数据流包装。gzip压缩之后的格式为gzip格式，RFC 1952格式，也被deflate数据流包装。deflate数据流为RFC 1951格式。zlib也可以读取gzip以及deflate数据流。
- 1 项目结构  
  - 1.1 代码文件，根目录下的c与h文件，主体为zlib
  - 1.2 其他文件，test、doc、examples等。

- 2 zlib.h
zlib.h定义了z_stream_s结构体、gz_header_s，zlib用到的重命名的类型在zconf.h声明的。