# C语言学习
- 1、很多工具都是由C编写，要了解使用这些工具，或开发自己的工具，需要学习C语言。

# vscode调试C
需要安装GCC，vscode插件，然后

## Lua语言
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