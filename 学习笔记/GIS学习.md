# GIS学习
GIS主要围绕数据运行，主要为数据存储、数据分析、数据显示。
## GEOS
&emsp;&emsp;GEOS作为矢量数据相关的库，作为GDAL、qgis以及PostGIS等程序的依赖。GEOS主要用C++以及C进行开发，不过作为JTS的C++/C的实现，所有的算法都现在JTS中进行实现，然后再GEOS中进行扩展。阅读GEOS的源码了解矢量对象的构建以及算法的实现逻辑。从github上拉取[GEOS源码](https://github.com/libgeos/geos/ 'GEOS').   
&emsp;&emsp;简单分析下源码结构，源码结构如下图所示。  
![Alt text](../assets/GEOS-code.PNG)

- 1、benchmarks为性能基准测试
- 2、capi为c语言的接口
- 3、cmake的相关配置
- 4、创建debian相关的deb可执行文件
- 5、doxygen格式文档
- 6、examples为geos的使用示例
- 7、include与src一致，不清楚具体作用
- 8、src主要的代码，作为重点阅读
- 9、tests测试相关
- 10、tools为ci/cd相关工具
- 11、util主要为geosop，为geos的命令行工具
- 12、web为官网的页面

虽然有很多文件夹，我们主要需要阅读的仅为src下的文件。然后我们具体查看下src文件夹下的具体代码。  
![Alt text](../assets/GEOS-code-src.PNG)

- 1、algorithm，从名称看即为算法相关的代码，主要为基础的几何算法，包括角度计算、距离计算、面积计算以及最小外接圆的计算算法等。
- 2、coverge，为矢量数据的验证，验证几何是否有效，发现几何之间的缝隙等
- 3、deps，依赖，这里主要为ryu，ryu为浮点小数的最小表示
- 4、edgegraph，几何边相关操作，
- 5、geom，定义几何对象相关类，如：点、线、面、三角形、外接矩形等
- 6、geomgraph，图形相关类，节点、边、带方向的边等
- 7、index，几何索引，二叉树索引，kd树索引等
- 8、io，geojson以及wkb格式读写操作
- 9、linearref
- 10、math
- 11、noding
- 12、operation，几何操作，包括缓冲区分析、叠加分析、聚类分析等
- 13、planargraph
- 14、precision
- 15、shape
- 16、simplify
- 17、triangulate，
- 18、util，通用的工具

geos中定义的几何相关的类主要包括Coordinate、Geometry（子类：Point、LineString、Polygon等）、position、triangle、
# JTS