# <center>Java相关学习</center>
- [Java相关学习](#java相关学习)
  - [一、Kafka学习](#一kafka学习)
    - [1.1、功能](#11功能)
    - [1.2、组成](#12组成)
    - [1.3、运行](#13运行)
    - [1.4、创建topic](#14创建topic)
    - [1.5、接收指定topic消息](#15接收指定topic消息)
    - [1.6、向指定的topic发送消息](#16向指定的topic发送消息)
    - [1.7、操作kafka](#17操作kafka)
  - [二、arthas学习](#二arthas学习)
  - [三、dubbo学习](#三dubbo学习)
  - [四、SPI（service provider interface）](#四spiservice-provider-interface)
  - [五、netty学习](#五netty学习)
  - [六、java线程池](#六java线程池)
  - [七、RocketMq学习](#七rocketmq学习)
    - [7.1、从源码编译安装包](#71从源码编译安装包)
    - [7.2、创建topic](#72创建topic)
    - [7.3、nameserver](#73nameserver)
  - [八、springcloud学习](#八springcloud学习)
    - [8.1、服务注册到注册中心](#81服务注册到注册中心)
    - [8.2、使用openfegin进行服务调用](#82使用openfegin进行服务调用)
  - [九、Zookeeper学习](#九zookeeper学习)
    - [9.1、Zookeeper目的](#91zookeeper目的)
  - [十、Maven学习](#十maven学习)
  - [十一、spring学习](#十一spring学习)
  - [十二、测试](#十二测试)
  - [十三、GRPC](#十三grpc)
  - [十四、HotSpot编译](#十四hotspot编译)
  - [java内存区域](#java内存区域)
  - [java垃圾收集](#java垃圾收集)
  - [java并发](#java并发)
  - [jvm虚拟机](#jvm虚拟机)
  - [JVM性能监控和故障处理工具](#jvm性能监控和故障处理工具)

## 一、Kafka学习
### 1.1、功能
&emsp;&emsp;发布与订阅消息、记录消息流、处理消息流。适用于数据吞吐量大，但对于消息重复、丢失不敏感情景。如果对消息正确性要求高但对数据吞吐量要求不严格可以采用rabbitmq。
### 1.2、组成
&emsp;&emsp;生产者（producer）、topic（主题）、consumer（消费者）
消息存储按照分区存储，不同分区的消息都是按照时间顺序存储的。不同分区的数据都包含一个offset属性，用以区分该消息的在所有的消息的时间顺序。Kafka可以先把消息保存下来，等有消费者消费时从文件中取出消息，发给消费者。消息的发布有两种模式，队列模式以及发布-订阅模式。队列模式中每个消息仅可被一个消费者消费；而发布-订阅则把消息广播给所有的消费者。
### 1.3、运行
&emsp;&emsp;新版的kafka内置了zookeeper，首先启动zookeeper，bin/zookeeper-server-start.sh config/zookeeper.properties；
然后启动kafka，bin/kafka-server-start.sh config/server.properties
### 1.4、创建topic
&emsp;&emsp;bin/kafka-topics.sh --create --topic topicName --bootstrap-server localhost:9092
### 1.5、接收指定topic消息
&emsp;&emsp;仅需给方法添加注解即可，@KafkaListener(topics = {"djTest"})，在方法中解析topic中的数据。
### 1.6、向指定的topic发送消息
&emsp;&emsp;调用kafkaTemplate.send(TOPIC_NAME,  "key", msg);即可，其中msg为消息内容。TOPIC_NAME为topic值
### 1.7、操作kafka
&emsp;&emsp;通过添加kafka-clients依赖，创建AdminClient对象，即可通过adminClient对象操作kafka。创建topic，查看topic以及删除topic

## 二、arthas学习
&emsp;&emsp;Arthas为阿里开源的java诊断程序，这里仅利用其查看具体方法执行的耗时分析。具体使用为运行java -jar arthas-boot.jar，然后选中带分析的java进程。要分析具体方法耗时，采用trace demo.mathGame run,其中demo.mathGame为具体的类名具体路径，run为该类下的方法。

## 三、dubbo学习
&emsp;&emsp;首先需要安装zookeeper，启动运行zkServer.cmd即可。创建api模块，定义接口，api模块不需要引入其他依赖。   
&emsp;&emsp;创建provider模块，引入api模块以及dubbo以及zookeeper依赖，实现api接口。在yaml文件中配置dubbo配置，包括应用名称、服务类包扫描、对外协议端口以及注册中心等。在实现类上添加dubbo的Service注解。可以创建多个provider，进行负载均衡。   
&emsp;&emsp;创建consumer模块，同provider类似引入api模块以及dubbo依赖，并在yaml文件中配置dubbo配置，主要为应用名称、注册中心以及标志为消费者。创建controller类，其中需要引入接口添加Reference，可以在里面添加负载均衡配置。  
&emsp;&emsp;Dubbo在spring启动的时候增加了监听ApplicationContextInitializer事件，当服务启动的时候会监听到该事件然后调用DubboBootstrap下的start方法。
使用dubbo需要在接口实现服务类入口处添加@EnableDubbo注解，该注解有依赖了DubboComponentScan以及EnableDubboConfig这两个注解。DubboComponentScan注解import了DubboComponentScanRegistrar类，而EnableDubboConfig注解import了DubboConfigConfigurationRegistrar类。这两个类都继承了ImportBeanDefinitionRegistrar，因此会调用registerBeanDefinitions方法。在registerBeanDefinitions方法中都有DubboSpringInitializer.initialize(registry)代码，
## 四、SPI（service provider interface）
&emsp;&emsp;Api接口与实现绑定在一起，spi接口与实现是分离的，可以通过配置文件具体加载哪个实现类，方便扩展。Api为服务实现方与接口定义在一起，调用方引入实现方api即可调用实现方的具体实现；spi接口定义存在调用方，实现方根据接口定义给出不同的实现。Spi与api都属于接口。

## 五、netty学习
&emsp;&emsp;Netty提供异步的、事件驱动的网络应用程序框架和工具，用以快速开发高性能、高可靠性的网络服务器和客户端程序。  
&emsp;&emsp;Netty通过添加handler进行消息处理，handler分为InboundHandler以及outboundHandler，多个handler组成一个链表，当接收消息时按照代码编写的添加inboundhandler顺序依次执行；在发送消息时按照outboundhandler顺序逆序执行。一个handler处理完成之后需要交给下一个handler去处理需要调用ctx. fireChannelRead方法。

## 六、java线程池
&emsp;&emsp;Java的web编程通过socket连接一个客户端，服务器与客户端为1对多的关系。当新的客户端连接时javaBIO会创建一个线程处理当前的连接。因此有多少连接即会创建多少线程，如果连接数不断增加，则会出现OOM问题。可以创建线程池来限制线程的创建，通过规定线程池大小，可以允许当前活跃线程个数。线程池当中有常驻线程，临时线程以及队列。如果当前正在执行的任务个数超过常驻线程数，则将其放在队列中，如果队列也满了则创建新的线程，线程总数不可超过设置大小。如果任务个数超过线程总数大小，则拒绝请求。在队列中的任务一直在等待。只有线程中处理完其他任务之后才去队列中领取任务。

## 七、RocketMq学习
### 7.1、从源码编译安装包
&emsp;&emsp;在github上下载下源码，然后执行mvn -Prelease-all -DskipTests clean install -U命令，即可在rocketmq\distribution\target生成安装包
### 7.2、创建topic
&emsp;&emsp;执行命令：mqadmin updateTopic -n localhost:9876 -b localhost:10911 -t RealBizTopic
其中-n为nameserver -b broker -t为topic，即需要指定nameserver在具体的broker上创建topic
### 7.3、nameserver
&emsp;&emsp;Rocketmq中nameserver作为注册中心，管理所有的broker，当发布者需要发布topic时需要从nameserver获取该topic所在的broker。消费者也是一样当消费某个topic是需要从nameserver获取broker地址。Nameserver中的namesrvController为所有关于nameserver操作方法。其中包括routeInfomanager，即为存储topic与broker对应关系、broker集群信息、存活的broker列表等信息。namesrvController还包括一些配置信息、定时任务以及remoteServer。其中定时任务主要包括配置信息的更新以及检查不存活的broker；remoteServer为具体的netty创建服务处理连接请求。

## 八、springcloud学习
&emsp;&emsp;基本定义：Spring Cloud基于Spring Boot框架开发应用，为微服务开发中的架构问题提供了一整套的解决方案:如服务注册与发现、服务消费、服务容错、API网关、分布式调用追踪和分布式配置管理等。
### 8.1、服务注册到注册中心
&emsp;&emsp;为了让其他服务可以找到该服务并且调用，需要将服务注册到注册中心中。这里以consul为例，在创建的springboot工程中pom.xml添加spring-cloud-starter-consul-discovery依赖以及spring-boot-starter-actuator检查服务状态的依赖。然后在yml文件中配置consul地址以及服务名称等。
```yml
spring:
  application:
    name: student-provider
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: student-provider #注册到 Consul 的服务名称，客户端根据这个名称来进行服务调用。
```

### 8.2、使用openfegin进行服务调用
&emsp;&emsp;Spring Cloud OpenFeign 对 Feign 进行了二次封装，使得在 Spring Cloud 中使用 Feign 的时候，可以做到使用 HTTP 请求访问远程服务，就像调用本地方法一样的，开发者完全感知不到这是在调用远程访问，更感知不到在访问 HTTP 请求。Spring Cloud OpenFeign 增强了 Feign 的功能，使 Feign 有限支持 Spring MVC 的注解，如 @RequestMapping 等。OpenFeign 的 @FeignClient 注解可以解析 Spring MVC 的 @RequestMapping 注解下的接口，并通过动态代理的方式产生实现类，在实现类中做负载均衡并调用其他服务，默认集成了 Ribbon 与 Hystrix。  
&emsp;&emsp;Openfeign通过http形式进行rpc，首先创建接口模块，添加spring-cloud-starter-openfeign依赖。接口模块定义了接口，并在接口上添加@FeignClient(“xxx”)注解，其中xxx为服务的名称。在具体方法中添加@RequestMapping注解，指定目标服务的url地址。即通过服务名称加服务url地址进行远程服务调用。  
&emsp;&emsp;然后创建服务提供者模块，引入接口模块。创建服务类实现接口，在该类中实现具体的业务。然后需要将服务提供模块注册到注册中心以便消费者发现，这是使用的consul注册中心，因此需要添加spring-cloud-starter-consul-discovery依赖。还可添加spring-boot-starter-actuator进行服务检查。  
&emsp;&emsp;最后创建服务消费者，引入接口模块。在具体类中注入接口，调用接口的方法，虽然调用了接口方法但是实际上调用了服务提供者模块实现的方法。服务消费者需要接入注册中心，这样才可以从注册中心根据服务名称获取服务提供者地址。添加spring-cloud-starter-consul-discovery依赖即可。还需要在启动类中添加@EnableFeignClients注解。


## 九、Zookeeper学习
### 9.1、Zookeeper目的
&emsp;&emsp;Zookeeper用来协调管理分布式的服务，具有简单、可靠、顺序以及快速优点。Zookeeper提供了类似文件系统的znode，不同的是znode可以携带少量数据，用户可以创建、修改、删除znode等操作。当znode被修改之后，Zookeeper可以通知所有监听该znode的用户。Zookeeper用途：数据发布与订阅、负载均衡、命名服务，配置管理，集群管理，分布式锁，队列管理。

## 十、Maven学习
在springboot项目pom.xml文件中，可以添加外部库的依赖，如果一个springboot项目中包含多个模块，不同模块引用了不同版本的相同依赖则可能会产生不必要的冲突。因此需要在项目根目录下的pom.xml文件中添加dependencyManagement模块，用来指定需要的依赖以及版本，其他子模块使用该依赖时不需要指定版本号，因此实现依赖版本的统一管理。

## 十一、spring学习
- 1、AbstractController，采用模板设计模式创建的、方便controller实现的父类。其中定义了handleRequest以及handleRequestInternal两个方法，handleRequestInternal是AbstractController子类必须要实现的方法，其参数为请求以及相应对象。
- 2、ApplicationContext是Spring提供的核心接口，可以用来创建、获取以及管理bean。
## 十二、测试
- web请求性能测试`ab -c 500 -n 10000 http://192.168.31.83/index.html`

## 十三、GRPC
grpc为google开发的一种grpc协议，采用proto文件定义接口与接口需要的参数，与语言无关，因此可以实现不同语言之间的调用。在不同语言中需要实现proto到该语言的转换，需要安装protoc工具。
- python创建grpc服务器   
安装需要的依赖
```python
pip install grpcio #gRPC 的安装 
pip install protobuf  #ProtoBuf 相关的 python 依赖库
pip install grpcio-tools   #python grpc 的 protobuf 编译工具
```

然后编写proto文件，运行一下命令解析proto文件，生成py文件
`python -m grpc_tools.protoc -I. --python_out=./base_package --grpc_python_out=./base_package ./data.proto`
- java编写grpc客户端调用python服务的方法   
添加依赖
```xml
   <groupId>net.devh</groupId>
   <artifactId>grpc-spring-boot-starter</artifactId>
```
复制proto文件，需要与服务端的一致。通过maven的proto插件解析为java文件。在yml中配置grpc：
```yml
grpc:
  client:
    userClient:
      negotiationType: PLAINTEXT
      address: static://localhost:9091
  server:
    port: 9092
```


## 十四、HotSpot编译
&emsp;&emsp;hotspot在openjdk中，因此需要从github中获取openjdk源码,如`https://github.com/openjdk/jdk22`，根据源码中的doc下的building文档进行编译，在wsl中编译需要特别主要，默认编译的是windows系统的，需要configure配置`--build=x86_64-unknown-linux-gnu --openjdk-target=x86_64-unknown-linux-gnu`.编译openjdk即可获取hotspot。安装所需的依赖。hotspot是最流行的java虚拟机，新的虚拟机还有graalvm，可以支持多语言。

## java内存区域
- 1、运行时数据区
方法区、虚拟机栈、本地方法栈、堆、程序计数器。其中方法区以及堆是所有线程共享的区域，其他的为线程私有。
  - 1、程序计数器是每个线程私有的，为了记录当前线程执行的位置，便于下次线程获取cpu时间时继续运行。
  - 2、虚拟机栈也是线程私有，随着线程创建而分配，线程销毁，虚拟机栈也被回收。虚拟机栈存放线程的局部变量表、操作数栈以及动态链接等。
  - 3、本地方法栈，虚拟机栈是为了执行java方法而服务的，本地方法栈为运行本地方法服务的。
  - 4、堆，是所有线程共享区域，随着虚拟机启动时创建。堆中主要存放对象实例，是垃圾回收的主要区域。
  - 5、方法区，也是线程共享区域，主要存储虚拟机加载的类型信息、常量、静态变量、代码缓存等数据。
  
- 2、对象的创建  
&emsp;&emsp;对象的创建大体可以分四个步骤：
  - 1）类加载：虚拟机在执行new指令时需要先检查new之后的参数在常量池中是否有一个类符号引用，然后根据类符号引用判断是否被加载，如果没有加载需要先加载类。
  - 2）内存分配：内存分配需要考虑两个问题：哪些内存块是空闲的可以分配；如果保证内存块的线程安全。虚拟机为新对象分配内存，对象占用内存在类加载完成之后即可确定。内存的分配又包括两种类型：指针碰撞以及空闲列表。具体采用那种类型取决于采用的垃圾回收器。内存分配过程中还需要注意线程安全性,可以采用同步操作CAS加失败重试；本地线程分配缓存。
  - 3）对象内存空间初始化，内存分配完成之后虚拟机需要将其初始化为0.
  - 4）对象设置：虚拟机需要设置对象头。
  - 5）最后，根据用户代码初始化对象值，执行对象的构造函数`<init>`方法.至此一个对象创建完成。

- 3、对象内存布局  
&emsp;&emsp;对象在堆中存储结构可以分为三部分：对象头、实例数据以及对齐填充。对象头包括两类：存储运行时数据以及类型指针。运行时数据包括哈希码、GC分代年龄以及锁状态标志等。类型指针表示当前对象是哪个类的实例。实例数据是对象真正存储的有效信息，即定义的各种字段内容。对齐填充，不是必须存在的，hotspot虚拟机定义对象存储大小必须为8的整数倍，如果不满则进行填充。

- 4、对象的访问  
&emsp;&emsp;对象的访问是通过栈上的reference数据来操作堆上的具体对象。对象访问分为两种句柄访问以及指针访问。句柄访问在堆中有一块句柄池用来存放具体的对象地址，栈上的引用指向句柄池的具体的位置。指针访问栈上存储的为对象地址。整体来说指针访问速度比句柄更快，hotspot即采用这种访问方式。
- 5、OutOfMemoryError异常
&emsp;&emsp;java程序编译 `javac xxx.java`，javac编译时将源码中的每个对象生成一个class文件；java程序执行`java xxx`，在执行命令可以设置堆栈大小，-Xss2m即为设置线程堆栈2m大小；-Xms设置初始堆大小；-Xmx设置最大堆大小。`-XX:+HeapDumpOnOutOfMemoryError`可以在jvm出现outofmemory错误时保存内存堆快照。java参数需要在运行的class前面设置。可以通过jvisualvm工具加载内存快照查看哪些对象占用内存过大。内存泄漏是指用完的对象没有回收一直占用内存，导致内存不能分配；内存溢出是指程序运行时申请的内存大于系统可以提供的内存。


## java垃圾收集
- 1、引用计数法  
&emsp;&emsp;简单，需要额外计数，但是不能解决两个类相互引用问题。rust没有垃圾收集，取而代之的则是`所有权`,其在编译过程中会禁止相互引用产生。

- 2、可达性分析  
&emsp;&emsp;类似一个树状结构的引用关系，根节点为GC Roots，如果一个对象与GC Roots之间没有任何引用链相连，则认为该对象不可达，不再被使用。GC Roots不是一个，而是一类集合。

- 3、引用  
&emsp;&emsp;如果一个变量中存储的是另外一块内存区域的开始地址，则称该变量为这块内存的引用。在java中应用按照引用强度分为四种：强引用、软引用、弱应用以及虚引用。强引用是普遍的引用赋值，只要强引用存在则垃圾收集器不会回收。软引用，一些还有用但是非必须的对象，当内存不足时软引用会列为第二次内存回收，如果还是内存不足则会报错。弱引用比软引用更弱，在第一次垃圾回收就会将其回收。虚引用是最弱的引用，无法通过虚引用获取一个对象实例，其作用是该引用被回收时收到一个系统通知。

- 4、垃圾收集算法  
分代收集理论，将内存分为不同的代，针对不同的代进行不同的垃圾收集。标记-清除算法，先将要保留或要删除的对象进行标记，然后将其他未标记的进行保留或删除。会产生大量的碎片化内存，如果标记的是需要保留的而需要保留的对象又占大多数则会导致效率低下。标记-复制算法，将内存划分为两块，一部分进行使用，将需要保留的进行标记，进行垃圾回收时将其复制到未使用的内存块中，然后将另一半整体清空。有利于解决内存碎片化问题，但会导致内存使用率不高。大多对象存在朝生夕灭特点因此可以不按1：1划分内存，已充分利用内存。标记-整理算法，标记过程和标记-清楚算法一致，不过最后不是直接清除对象，而是将标记的移动到内存一端，按照边界清除掉边界外的对象。
&emsp;&emsp;安全点，在进行垃圾回收过程中，需要将用户线程停顿，需要将用户线程停顿的位置称为安全点。在安全点对象之间的引用关系不会发生变化，便于进行垃圾回收。
&emsp;&emsp;分代收集理论，将内存分为不同的代，针对不同的代进行不同的垃圾收集。标记-清除算法，先将要保留或要删除的对象进行标记，然后将其他未标记的进行保留或删除。会产生大量的碎片化内存，如果标记的是需要保留的而需要保留的对象又占大多数则会导致效率低下。标记-复制算法，将内存划分为两块，一部分进行使用，将需要保留的进行标记，进行垃圾回收时将其复制到未使用的内存块中，然后将另一半整体清空。有利于解决内存碎片化问题，但会导致内存使用率不高。大多对象存在朝生夕灭特点因此可以不按1：1划分内存，已充分利用内存。标记-整理算法，标记过程和标记-清楚算法一致，不过最后不是直接清除对象，而是将标记的移动到内存一端，按照边界清除掉边界外的对象。



## java并发
- 1、并发编程挑战  
  - 1.上下文切换：cpu时间片分配算法执行多线程，每个时间片大约几十毫秒，在线程之间切换时会先保存当前任务的状态，然后加载下一个任务状态，即为上下文切换。如果程序执行不耗时则并发可能会比串行要慢，因为存在上下文切换的开销。用lmbench可以测量上下文切换的时长；vmstat测量上下文切换的次数。  
内存解读：系统性能收到空闲内存影响，如果空闲内存较小，系统性能会降低；还需要查看si（磁盘到内存的交换页数量）与so（内存到磁盘的交换页数量），如果物理内存够系统用即使free内存很小系统还是正常运行，如果内存不够用则会用磁盘换内存，导致si与so交换频繁，系统性能降低。CPU解读：cpu主要关注运行队列、cpu使用率以及上下文切换。r为运行队列中等待的任务数，如果CPU过载会导致运行队列等待的任务数越来越多。cpu使用率包括用户进程、系统进程、等待io以及空闲。如果cpu空闲率小于10则表示cpu资源较为紧张。  
减少上下文切换：无锁并发、CAS算法、使用最少线程、使用协程。
  - 2.资源限制  

- 2、java并发原理
synchronized与volatile，volatile是轻量级的synchronized。如果一个变量为volatile则jvm中所有的线程看到该变量是一致的。volatile在编译为汇编时添加了lock指令，即cpu修改缓存中值时会锁住缓存，不会受到其他线程影响，cpu修改之后将其写回内存。cpu采用MESI控制协议使其他线程内存值失效.
  - 2.1 原子操作原理
    处理器通过总线锁实现原子操作，即通过LOCK指令，使得一个处理器独占共享内存。总线锁会把整个共享内存锁住，而实际大部分需要仅为锁住某个内存地址。

## jvm虚拟机
- 1、在jvm中运行时数据区分为两类：所有线程共享以及线程隔离数据区。所有线程共享的数据区有方法区、堆。线程隔离的数据区有虚拟机栈、本地方法栈、程序计数器。
![JVM数据区域](../../assets/JVM数据区域.JPG 'JVM数据区域') 
- 2、程序计数器，可以看作当前线程所执行的籽伽玛的行号指示器。字节码解释器需要根据当前的程序计数器来选取下一条需要执行的字节码指令。java可以多线程执行，因此每个线程单独拥有自己的程序计数器。
- 3、虚拟机栈：是描述java方法执行的线程内存模型：每个线程都会创建一个栈帧用来存储该线程用到的变量、方法、动态连接等。内存主要分为堆和栈，在java中栈主要为虚拟机栈。如果线程请求栈深度大于虚拟机允许的深度则会抛出StackOverflowError异常，如果java虚拟机栈容量动态扩展，扩展不发申请足够内存会抛出OutOfMemoryError异常。
- 4、本地方法栈，虚拟机用到的本地方法服务。
- 5、java堆：是所有线程共享的内存区域，在虚拟机启动时创建。该区域存放几乎所有的对象实例以及数据。java堆是垃圾收集器管理的内存区域，一些jvm的实现将堆分为不同的代（新生代、老年代以及永久代等）目的是为了更快的分配内存和回收内存。
- 6、方法区：也是所有线程共享的区域，用于存储被虚拟机加载的类型信息、常量、静态变量以及即时编译代码缓存等。运行时常量池是方法区的一部分，用来存储编译期生成的各种字面量与符号引用，在类加载后存放到方法区的运行时常量池中。
- 7、直接内存：直接内存不是虚拟机运行数据区的一部分，是有本地方法申请的内存，在一些场景中避免了本地方法与java堆数据转移，可以提高效率。直接内存不受java堆大小的限制。

## JVM性能监控和故障处理工具
- 1、jps，列出正在运行的虚拟机进程，类似于linux中的ps命令。jps通过uri来查找本机或远程主机的临时文件，格式如：{tmpdir}/hsperfdata_{any_user_name}/[0-9]+,每个文件都是一个虚拟机产生的，从该文件中可以读取虚拟机的进程id。
- 2、jstat用于监视本地或远程虚拟机各种运行状态信息，如类的加载、内存、垃圾收集、及时编译等数据。
- 3、jinfo实时查看和调整虚拟机各项参数。
- 4、jmap用于生成堆转存储快照。
- 5、jstack用于生成虚拟机当前时刻的线程快照。线程快照就是当前虚拟机每条线程正在执行的方法堆栈的集合。


