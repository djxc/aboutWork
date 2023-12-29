# OGC标准
OGC（开放地理空间信息联盟）是一个非盈利的、国际化的、自愿协商的标准化组织，它的主要目的就是制定与空间信息、基于位置服务相关的标准。OGC标准较为权威，且不是强制性，但也存在一些缺点与不足。OGC标准是一些接口或编码的技术文档，其他厂家可以参考用来开放服务的接口、空间数据存储的编码、空间操作的方法等。OGC标准很多具体可以访问[OGC官网](https://www.ogc.org/standards/), 如：3dtiles、cityjson、geotiff、WMS以及WMTS等。

## 1 COG标准
COG(Cloud optimized GeoTIFF)即为适用于云的优化的tif，主要通过http的range参数读取tif中的部分内容。不需要把整个图像下载下来进行部分读取，仅需要请求部分数据即可。