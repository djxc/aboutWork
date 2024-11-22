# OSGEarth学习
- 1、源码编译，在windows平台上参考`https://docs.osgearth.org/en/latest/build.html#building-with-vcpkg`官方文档进行源码编译。OSGEarth所依赖的文件较多，如gdal、geos、opengl、blend2d以及osg等，因此下载依赖较慢。官方推荐通过vcpkg进行安装。编译动态链接库在`build\vcpkg_installed\x64-windows`下。
- 2、osgEarth是一个三维地图SDK，采用OpenSceneGraph三维引擎（底层基于OpenGL）。