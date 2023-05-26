# libgeotiff
libgeotiff是用来提取并解析GeoTIFF中的key字典，也可以用来在新的GeoTIFF中定义key。libgeotiff作为很多软件的基础库用来读写GeoTIFF文件，如GDAL以及Esir产品簇。

- 1、项目结构
libgeotiff源码地址：https://github.com/OSGeo/libgeotiff.git
根目录下主要有两个文件夹geotiff以及libgeotiff。geotiff文件夹内主要为libgeotiff的文档描述，libgeotiff文件夹为主要代码,当前目录下的.h于.c文件。libgeotiff基于tiff构建的因此libgeotiff依赖于libtiff，geotiff包含一些投影坐标系的操作，因此libgeotiff还需要依赖proj6.libgeotiff的构建
```sh
cd libgeotiff
./configure --with-proj=/contrib/proj-6 --with-libtiff=/usr/local
make 
```

- 2、功能接口
主要看geotiff.h文件，该文件开头定义了一些结构体、枚举以及变量。功能接口主要分为四部分：
  - 1、TIFF-level接口tiff的创建、释放以及写入keys
  - 2、geokey接口，获取并设置geokey
  - 3、metadata接口，获取tag名称以及值，打印以及获取key的编码等
  - 4、image于pcs之间的转换



## libgeotiff调试
在libgeotiff目录下的bin目录有若干个测试文件。首先需要编译proj，在libgeotiff的build下的CMakeCache.txt中设置PROJ_INCLUDE_DIR变量。即可调试bin下的程序。