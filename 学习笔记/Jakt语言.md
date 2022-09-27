Jakt编程语言利用rust进行开发，通过将当前的语法转换为c++文件，然后利用C++编译执行。
1、	转换为c++文件，jakt array_iterator.jakt，会生成xxx.cpp文件
2、	编译clang++ -std=c++20 -Iruntime -Wno-user-defined-literals array_iterator.cpp
