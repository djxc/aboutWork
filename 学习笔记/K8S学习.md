# K8S学习

k8s是一种容器编排工具。用于自动化部署、扩展以及管理容器。
- 1、节点  
运行pod的物理机
- 2、pods  
是k8s管理的最小单位，每个pod有一个分配给pod中的所有容器的单独的IP 地址。在pod中的容器内存和存储资源是共享的。当应用程序只有一个进程时，pod也可以有一个容器。pod管理容器。
- 3、replicaSet  
目的是维护一组在任何时候都处于运行状态的 Pod 副本的稳定集合。
- 4、label  
Label以key/value键值对的形式附加到各种对象上，如Pod、Service、RC、Node等。
Label定义了这些对象的可识别属性，用来对它们进行管理和选择。Label可以在创建时附加到对象上，也可以在对象创建后通过API进行管理。
- 5、Deployment    
用于管理Pod、ReplicaSet，可实现滚动升级和回滚应用、扩容和缩容。
- 6、service  
service定义了服务访问入口，pod会随着需求新建摧毁，service会找到其绑定的pod，客户端请求时先到service，然后发送给pod。