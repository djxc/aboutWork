# python调用c方法
## 一、引言
&emsp;&emsp;当前的开发环境为python时，一些功能已被用c实现，或为了更快的速度时，就需要用python调用c的方法。本文描述的运行系统为linux环境。python调用c主要步骤如下四步：
- 1、搜集/编写C方法
- 2、测试方法是否运行正常，
- 3、编译生成动态链接库
- 4、用python加载动态链接库，调用C的函数  

```mermaid
graph LR;  
编写C方法 --> 测试方法 --> 编译为静态链接库 --> python加载调用
```

## 二、实例
举个hello world的例子：
- 1、编写C方法，创建main.c文件，内容如下
```c
int add_int(int a, int b) {
    return a + b;
}
```
- 2、测试，可以生成可执行文件看看是否可以正常运行`gcc main.c -o main`.
- 3、测试正常之后可以生成动态链接库，`gcc main.c -shared -fPIC -o libmain.so`其中-shared表示生成动态链接库；-fPIC表示编译为位置独立的代码，如果不选择默认位置相关，编译时还是会将函数拷贝到代码段中，发挥不出动态链接库省内存空间的优势。
- 4、用python加载动态链接库，并调用c函数
```python
import ctypes
lib = ctypes.cdll.LoadLibrary("./libmain.so")
result = lib.add_int(2, 4)
```
## 三、进阶
&emsp;&emsp;以上通过一个实例描述了python调用c方法的整个流程，但是在实际使用中大部分需求不会这么简单。如果一个已经开发好的C项目需要调用其中的方法，需要将其编译为静态链接库（.a）文件，在编译静态链接库时可以需要安装各种依赖库；然后编写对python暴露接口的C程序。将C程序静态链接库编译为动态链接库(gcc xxx.c some_c.a -L. -shared -fPIC  -o libxxx.so)。另外在python与C交互过程中可能不仅仅为基本类型的参数与返回值，输入参数可能需要是一个C结构体，返回值也可能是C的结构体，在C结构体与python的数据类型之间的转换个人认为较为麻烦。下面通过构建一个输入参数为结构体，输出也为结构体的实例进行演示说明。
- 1、编写C方法，创建main.c文件，内容如下
```c
/**
 * 时间结构体
 */
typedef struct DateTime {
    double second;
    int minute;
    int hour;
    int day;
    int month; 
    int year;
} dateTime;
/**
 * 矩阵结构体
 */
typedef struct MatrixResult {   
    int col;
    int row;
    double data[9];
} matrixResult;

matrixResult demo(dateTime date_time) {
    double data[9] = {1.2, 2.3, 3.5, 1.2, 2.3, 3.5, 1.2, 2.3, 3.5}; 
    matrixResult matrix_result;
    matrix_result.col = 3;
    matrix_result.row = 3;
    for(int row = 0; row < 3; row++) {
        for(int col = 0; col < 3; col++) {
            int index = row * col + col;
            matrix_result.data[index] = rc2it[row][col];
        }
   }
    return matrix_result;
}
```
本人对C不是太熟悉，C结构体内的数组赋值感觉比较麻烦，可以考虑使用数组指针，但是在结构体内数组指针在与python调用过程中没解决怎么获取数组指针内容，:sob:。以上定义了c的代码，以及数据结构。但是在python中如何将c中定义的结构体作为参数进行c方法的掉用，以及如果获取c返回的结构体返回值。
- 2、编写python类对象对应c的结构体  
&emsp;&emsp;需要在python中编写类来对应c的结构体，注意python中属性顺序需要和c中的顺序一致，否则会发现数据对应不上。python的属性数据与c中的属性数据不是按照变量名称对应的，而是按照顺序！
```python
import ctypes
from ctypes import *
# 定义类，属性与c中的结构体对应

"""矩阵计算结果"""
class Result(Structure):
    _fields_ = [
        ("col", ctypes.c_int),
        ("row", ctypes.c_int),
        ("data", ctypes.c_double * 9)
    ]

"""日期参数"""
class DateValue(Structure):  
    _fields_ = [
        ("year", ctypes.c_int),
        ("month", ctypes.c_int),
        ("day", ctypes.c_int),
        ("hour", ctypes.c_int),
        ("minute", ctypes.c_int),
        ("second", ctypes.c_float)
    ]

def create_date_value():
    date = DateValue()
    date.year = 2023
    date.month = 2
    date.day = 17
    date.hour = 10
    date.min = 49
    date.sec = 20
    return date


if __name__ == "__main__":
    lib = ctypes.cdll.LoadLibrary("./libmain.so")
    demo_fun = lib.demo
    demo_fun.argtypes = [DateValue]
    demo_fun.restype = Result

    date = create_date_value()
    in_param = create_param_value()  

    result = demo_fun(date)
    
    
    if bool(result):
        data = result.data
        result_list = []
        for r in range(result.row):
            sub_result_list = []
            for c in range(result.col):
                sub_result_list.append(data[c + r*c])
            result_list.append(sub_result_list)
    else:
        print("Return value is None")
    return result_list
```
在python中需要定义两个类来对应输入参数结构体以及输出的结构体，在python中需要定义动态库方法的输入参数类型`demo_fun.argtypes = [DateValue]`,输出参数类型`demo_fun.restype = Result`,调用成功之后可以直接获取result中的值。如果返回的结果为结构体指针，在python中需要`fun1.restype = POINTER(Result)`,获取值时需要用`result.contents.row`获取值。

# 四、总结
用python调用c程序，本文采用的是Python/C API，另外还可以采用cython。cython更简单，cython文件为.pyx，其中可以写python代码，在编译过程中可以生成c的动态库。cython首先需要创建.pyx,如下：
```python
# 引用第三方库
cdef extern from "dmath.h":
    cpdef int dsadd(int a, int b)

    ctypedef struct matrix:       
          int col
          int row
          int *data
      
    cpdef matrix create_matrix(int col, int row)


# 引用c自带库
cdef extern from "math.h":
    cpdef int sin(double x)

def add(int a, int b):
    return a + b
#end-def

cdef double dsin(double x):
    return sin(x * x)

def dsin_py(double x):
    return dsin(x)
```
然后又创建setup.py文件，
```python
from distutils.core import setup, Extension
from Cython.Build import cythonize

ext_modules=[
    Extension("demo",
              sources=["demo.pyx"],
              extra_objects=['dmath.a']
    )
]

setup(name="dj", ext_modules = cythonize(ext_modules))
```
然后编译生成动态链接库，`python3 setup.py build_ext --inplace --force`,会生成so文件，然后在python文件中直接引入这个动态链接库文件即可。
```python
from demo import add, dsin_py, pydabs, dsadd
print(add(3, 7))
```
在cython中可以直接使用c的自带库函数，
```python
# 引用c自带库
cdef extern from "math.h":
    cpdef int sin(double x)
```
也可以引入c语言第三方库，但是需要对该库打包生成静态链接库.a文件，在setup.py中指定即可。具体如果用cython传递结构体，还没弄明白。
