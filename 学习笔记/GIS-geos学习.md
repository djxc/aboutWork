# GEOS学习
geos是C++处理地理矢量数据的库，始于java的JTS项目。地理数据包括几何数据以及属性数据，在geos中主要的几何类为Geometry，其他的具体类型的几何像点、线以及面等都是继承于Geometry。
- 1、Geometry
Geometry为抽象的数据类型，具体数据类型包括POINT、LINESTRING、LINERRING、POLYGON、MULTIPOINT、MULTILINESTRING、MULTIPOLYGON以及GEOMETRYCOLLECTION其中类型，这七种类型具有指定的顺序0-7.在geos中Geometry创建都是由GeometryFactory创建或进行销毁。
- 1.1 userData：Geometry用userData来保存属性数据。
- 1.2 SRID用来标识Geometry采用投影类型。
- 1.3 Coordinate，geometry顶点，任何类型的geometry都包含coordinate
- 1.4 geometryType，类型
- 1.5 simple，当前geometry是否为简单几何
- 1.6 dimension,维度，点为0维、线为一维、面为二维
- 1.7 boundary, 边界，是一系列更低维度的geometry几何
- 1.8 envelope, 外接矩形  
一些空间关系（DE-9IM框架和最小谓词集都是完备的、唯一的，可以较细的区分人类可辨识的空间拓扑关系。）  
- 1.9 disjoint，判断两个几何是否分离，是intersect对立面
- 1.10 touches，两个几何相互接触
- 1.11 crosses，两个几何具有共同点，但不是包含关系
- 1.12 within，几何在另一个几何内部
- 1.13 contains，同within
- 1.14 overlaps
- 1.15 relate，两个geometry是否intersect
- 1.16 equals，两个几何相等
- 1.17 covers，所有geometry点都在另一个geometry中  
一些空间分析方法
- 1.18 buffer，缓冲区分析
- 1.19 convexhull，返回最小的凸多边形
- 1.20 reverse，将geometry中的coordinate调转顺序。
- 1.21 intersection，相交分析
- 1.22 union，联合分析
- 1.23 difference，类似于裁剪分析
- 1.24 





