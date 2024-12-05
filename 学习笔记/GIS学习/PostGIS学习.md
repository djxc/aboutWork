# PostGIS in actino (3th)
- 1、PostGIS是PostgresSQL空间数据库的扩展。PostGIS支持OGC标准以及SQL标准。OGC是开放地理信息联盟宗旨是设计地理信息的API标准；OSGeo是开放地理空间基金会，主旨是设计开源GIS工具与软件。PostGIS提供了空间操作、空间方法、空间数据类型以及空间索引。
- 2、什么是空间数据，具有空间类型数据的字段。空间数据库会通过SQL利用提供的方法和索引来查询和操作空间数据。空间数据库类型：1）Geometry，平面类型，是其他数据类型的基础；2）Geography，球面大地测量类型，线、面都是贴在地球表面曲线而不是直线；3）Raster，多波段单元类型；4）Topology，关系模型类型。