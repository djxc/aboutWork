# PCB设计

## 《电子设计从零开始》
- 1、电池，电池属性包括：电压、容量。如果两个1.5v电池串联则会得到3v电压。
- 2、电阻，电阻会阻碍电流，从而使得电压改变。串联电路中各处电流都相同。欧姆定律：电流*电阻 = 电压。电阻具有三个属性：阻值，阻值越大其电压变越小；电阻功率，根据焦耳定律：电流^2 * 电阻 = 功率；电阻种类，目前贴片电阻在便携式电子产品中只用最多。有的电阻阻值随着环境变化而变化，如：光敏电阻、热敏电阻等。光敏电阻是利用半导体的光致导电原理。
- 3、电位器，是一种三端电阻，现在较为主要的电位器是数字电位器，可以通过电子控制实现阻值改变。
- 4、通过Multisim软件进行电路绘制以及模拟
- 5、二极管，二极管具有单向导电性，即电流只能从一个方向流向另一个方向，反过来则不行。二极管具有伏安特性，即电压很小的时候没有电流通过，当电压增大一定值开始有电流通过，随着电压增大，电流增长速度很快。
- 6、三极管，用于放大或开关电信号的半导体器件，
- 7、电容，蓄能器件，不允许直流电通过，但允许交流电通过。电路中是交流电流时，电容充电，当断开开关，电容中的电压产生反向的电流，放电。电容对交流信号具有削弱作用，削弱程度与电容的容量和信号有关，这种削弱作用称为容抗。容抗随着容量和频率增大而较少。
- 8、电感器，储能器件。电感器对直流电流具有阻碍作用,电感器允许直流电流通过，不允许交流电流通过。电感器阻碍交流信号的能力为感抗，感抗随着频率增大而增大。
- 9、天线，发射或接收电磁波的换能器。可以把变化的电流以电磁波的方式发送出去，反之则可以把天空中的电磁波接收转换为电流。
- 10、调制，一般的信息都是低频信号，为了将低频信号发射出去，需要借助高频信号作为载体，用高频信号携带低频信号的过程成为调制。调制分为幅度调制以及频率调制。幅度调制是频率与高频信号相同，但是信号变化的幅度来反映低频信号。频率调制，按照低频信号来修改高频的频率，高频信号的幅度不变。
- 11、调谐，空中包含这各种频率的无线电波，天线会没有选择的全部接收，设备不可能全部解析出来，为接收指定频率的无线电信号，需要进行调谐。调谐模块主要由电容以及电感器组成，电容和电感器并联具有过滤信号的作用，谐振频率计算方式为：f = ${1\over2Π \sqrt{LC}}$;其中L为电感量，C为电容容量。调谐仅允许靠近谐振频率的信号通过，其他频率的信号被过滤掉。
- 12、电路设计过程：1）需求规划；2）系统框图，类似于流程图；3）电路原理图设计；4）电路制作及调试，通过Multisim进行模拟，再用面包板进行测试，对电路图进行修改再调试，反复多次；5）PCB设计制作，可以用嘉立创工具设计并打样；6）元器件焊接及检验

### 一、单片机
- 1.1 单片机系统是由单片机与各种外设组成，各种外设与单片机管脚直接或间接相连，进行数据或信号传输。最简单的单片机系统是由：单片机、电源、时钟信号、复位、外部程序存储器访问控制端。


### 二、信号放大器
- 2.1 三极管可以实现信号的放大，信号放大倍数叫作增益。信号放大可以分为两种类型，小信号放大以及功率放大。小信号放大是对信号的幅度放大,是电压的放大；功率放大是电流的放大。
- 2.2 小信号放大器，是由三极管、场效应管、运算放大器等组成。三极管的三个特性：直流增益、输入(b极)参数（Ib - Vbe）、输出（C极）参数（Ic - Vce）。


## 《电子学》
- 1.电压：两点间的电压是将一单位的正电荷从低电位点搬移到高电位点所作的功。电压又称电位差或电动势。
- 2.电流：电荷经过一点的流量速率，单位是安培。1安电流就是每秒1库伦电荷的流动。
- 3.电阻：电流关于电压变化特性，电阻可以将电压转换为电流。
- 4.分压器：在给定的一个输入电压的情况下，他会将一个给定的输入电压的一部分作为输出电压。
- 5.电路分为模拟电路以及数字电路，模拟电路的组件包括：放大器、滤波器、混频器、振荡器、电源；数字电路组件：逻辑门、寄存器、转换器（A->D,D->A）、计时器、触发器。
- 6.电荷，质子带正电荷，电子带负电荷。不同物体反复摩擦会改变电荷的分布情况。同种电荷相互排斥。
- 7.磁体和磁场，

## 电子电路设计的基本步骤
- 1、需求分析，1）确定设计的需求，输入输出特性、工作电压、功耗以及环境温度等。2）设计规范，确定性能指标，如增益、带宽、噪声等。
- 2、芯片选择，根据对电路的需求分析，确定集成电路类型，如运放、比较器以及开关等。考虑芯片的参数，输入输出电压范围、温度特性以及功耗。
- 3、原理设计，绘制电路原理图；确定电路的拓扑结构；选择电路参数，选择合适的电阻、电容、电感等元件参数。
- 4、电路仿真，参数仿真；信号仿真；稳定性仿真
- 5、布线布板，设计布局，合理安排电路元件和信号走线；将布线设计转化为实际的电路板
- 6、实际测试，打板焊接，测试，优化调整

## 电阻选择步骤
- 1、电阻器选择主要考虑阻值、误差、额定功率以及极限电压。
  - 1.1 确定阻值，根据欧姆定律求出电阻的阻值
  - 1.2 确定误差，电路误差越小越好
  - 1.3 确定功率，根据功率计算公式可以计算出电阻功率，$P=I^2R$,为了使电阻使用时间更长需要功率为实际功率的2倍以上。
  - 1.4 确定极限电压，电阻的极限电压可以通过公式 $U=\sqrt{PR}$