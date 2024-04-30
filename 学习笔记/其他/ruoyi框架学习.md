## ruoyi框架学习

ruoyi框架是基于springboot + vue搭建的后台管理系统。

### ruoyi-cloud版本
- 1、服务端启动项目：ruoyi-cloud采用nacos进行配置的管理，springboot的主要配置文件都放在数据库中ry-config表中，因此需要先创建ry-config和ry-cloud两个数据库，将数据（项目sql文件夹下的）；然后启动nacos，项目会通过nacos请求配置参数。如果nacos启动了，在修改数据库则可能获取的是旧的数据nacos可能对数据进行了缓存，因此需要重新启动nacos；最后, 由于rouyi-cloud采用微服务模式，需要启动各个模块，必须启动的模块是ruoyi-gateway、ruoyi-auth以及rouyi-system模块。
- 2、前端项目启动，首先安装依赖，然后`yarn run dev`启动即可