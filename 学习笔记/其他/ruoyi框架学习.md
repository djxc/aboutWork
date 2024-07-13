## ruoyi框架学习

ruoyi框架是基于springboot + vue搭建的后台管理系统。

### ruoyi-cloud版本
- 1、服务端启动项目：ruoyi-cloud采用nacos进行配置的管理，springboot的主要配置文件都放在数据库中ry-config表中，因此需要先创建ry-config和ry-cloud两个数据库，将数据（项目sql文件夹下的）；然后启动nacos，项目会通过nacos请求配置参数。如果nacos启动了，在修改数据库则可能获取的是旧的数据nacos可能对数据进行了缓存，因此需要重新启动nacos；最后, 由于rouyi-cloud采用微服务模式，需要启动各个模块，必须启动的模块是ruoyi-gateway、ruoyi-auth以及rouyi-system模块。
- 2、前端项目启动，首先安装依赖，然后`yarn run dev`启动即可
- 3、spring cloud gateway,作为微服务的网关，所有的请求都先到网关，网关在根据所配置的路由将请求转发给具体的服务。网关可以统一进行鉴权、限流等操作。gateway中的yml文件配置了routes参数，其中uri:lb://xxxx表示动态路由，需要gateway去服务注册中心（nacos）取具体的服务地址，
- 4、nacos，服务发现与服务管理，ui地址为：`http://localhost:8848/nacos/index.html`,方便用户管理。nacos ui主要包括配置管理、服务管理、命名空间以及集群管理。ruoyi-cloud中各个模块的yml文件配置都在配置管理中进行管理。感觉nacos以及springcloudgateway可以用k8s作为替换的解决方案。