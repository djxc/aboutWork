# GeoTIFF文件格式
[GeoTIFF文件格式](https://www.earthdata.nasa.gov/esdis/esco/standards-and-practices/geotiff)是基于TIFF格式的，是用来对地理信息的栅格影像传输的一种格式。当前的GeoTIFF版本为1.1，由OGC于2019年九月发布。本文翻译自NASA的ESDS(Earth Science Data Systems)组委会发布的[GeoTIFF文件格式](https://www.earthdata.nasa.gov/sites/default/files/imported/ESDS-RFC-040v1.1.pdf)(略有删减)。
- Ⅰ 摘要
该文件指定GeoTIFF文件格式作为NASA地球科学数据系统的一项标准.GeoTIFF在20世纪90年代初期开发的，是一个成熟的、设计优良并被广泛使用的文件格式。GeoTIFF在NASA内部被广泛使用。截止到目前，GeoTIFF没有最新的规范。开发地理空间信息联盟（OGC）于2019年九月份批准了OGC GeoTIFF Standard V1.1。V1.1版本兼容在1995年V1.0规范。另外，在NASA社区中构建其他科学的文件格式，比如：HDF5以及netCDF。尽管如此，GeoTIFF文件仍被持续关注与需要，主要为作为卫星或飞行器拍摄图像的发布一种格式，也应用在数字高程模型（DEM）数据，数字正交四边形以及关于人与环境交互的卫星衍生数据产品。

- [目录](#gis学习)
  - [1.简介](#1简介)
  - [2.GeoTIFF文件结构](#2GeoTIFF文件结构)
  - [3.使用时注意事项](#3使用时注意事项)
  - [4.该规范的未来版本](#4该规范的未来版本)
  - [5.参考文献](#5参考文献)
  
## 1简介
&emsp;&emsp;带标签的图像文件格式（TIFF）是由微软与Aldus共同开发在1996年。Aldus后来并入Adobe，是当前TIFF规范的版权持有者。GeoTIFF是建立在TIFF格式之上的一种文件格式，通过TIFF扩展机制将含有地理信息的标签添加到TIFF中，因此GeoTIFF名称为Geographic Tagged Image File Format.
  - 1.1 背景
GeoTIFF在1990年代初期投入使用，当时Intergraph Corp的Ed Grissom与JPL的制图应用组织分别独立开发一种将地理数据编码到TIFF格式中的方法。这种想法需要一种成熟的互操作并包含地理信息的文件格式。TIFF由于其无损失且可扩展性特性符合要求。在1994年九月SPOT图像公司提议了GeoTIFF结构，仅限于通用墨卡托投影。该提议经过Ritter与Ruth一年的增强与正式化在1995年十一月作为1.0版本，规范版本为1.8.2。欧洲石油勘探组织（EPSG）的Roger Lott博士一些提议为GeoTIFF项目带来了Epicenter模型的线性参数化以及FGDC元数据模型。这些规范在2019年之前一直保留在GeoTIFF官方规范中。
到2002年，开源软件libgeotiff已经到达1.1.4版本。该库还在持续开发中，且已成为人们理解原始GeoTIFF规范主要途径。GeoTIFF FAQ在2002年暗示过：
```javascript
虽然，GeoTIFF实现质量非常高，没有必要对生产的GeoTIFF文件做一致性检查。另一方面，尝试使复杂的投影使我们参数列表不断增长、清理单位类型问题以及其他问题都失败了。EPSG实现了很多关于潜在投影以及地球基准面列表的优化，但是是否能应用在GeoTIFF中还不清晰。只希望GeoTIFF2.0还没有完成。
```
在NASA内部与外部都做过很多尝试去开发GeoTIFF2.0规范。但都没有成功，直到2014年OGC的GeoTIFF规范工作组的形成。在2019年GeoTIFF标准工作组提出了新的OGC GeoTIFF标准V1.1文档，并在当年九月份通过、发布成为标准。版本号1.1表明新版本与之前的旧版本兼容。
  - 1.2 实现依据  
  - 1.2.1 GeoTIFF数据
    GeoTIFF格式的数据在NASA地球科学多个数据发布中心可以方便获取到。
    [ORNL DAAC](https://daac.ornl.gov), 提供了130个发布的数据集，包含25735个在线的GeoTIFF格式的小文件。此外，在ORNL空间数据访问工具
  - 1.2.2 GeoTIFF软件
  有很多开源软件实现了GeoTIFF，如[libgeotiff](https://github.com/OSGeo/libgeotiff)、[geotiff.js](https://github.com/geotiffjs/geotiff.js)以及[GeotiffParser.js](https://github.com/xlhomme/GeotiffParser.js)。
  + libgeotiff是用来提取并解析GeoTIFF中的key字典，也可以用来在新的GeoTIFF中定义key。libgeotiff作为很多软件的基础库用来读写GeoTIFF文件，如GDAL以及Esir产品簇。
  + geotiff.js可以读取不同TIFF文件类型的元数据以及原始的array数据。是纯javascript实现的。
  + geotiffparser.js该库已不再维护。
  - 1.2.3 相关工作
  大部分可以读写GeoTIFF的GIS与空间分析软件都使用libgeotiff库。
## 2GeoTIFF文件结构
## 3使用时注意事项
GeoTIFF格式并非适用于任何一种数据类型。在NASA社区中也包含其他的科学的文件格式，如：HDF5以及netCDF，这些数据格式都被NASA地球科学数据系统所采用。尽管如此，GeoTIFF文件仍被持续关注与需要，主要为作为卫星或飞行器拍摄图像的发布一种格式，也应用在数字高程模型（DEM）数据，数字正交四边形以及关于人与环境交互的卫星衍生数据产品。
## 4该规范的未来版本
GeoTIFF V1.1规范用户应当注意该规范还在由OGC持续的开发中。未来版本可能引入一些变化与当前使用不兼容。规范本身包含一些记录表明该部分在未来版本中可能被修订。  
推荐在GeoTIFF中谨慎使用规范版本号。新创建的GeoTIFF文件应该遵从修订版本1以及最小修订1.

## 5参考文献


