# minikube
minikube是本地k8s引擎，可以快速搭建单机的k8s环境，支持所有的k8s功能。
## 通过minikube部署k8s
- 1、安装docker，用docker作为k8s的容器运行时，当然也可以用containerd
- 2、启动minikube`minikube start --driver='docker'  --image-mirror-country='cn'`,这里指定了国内镜像，以dokcer作为容器运行时

Linux中容器最基础的技术：Namespace和Cgroups。容器即一个进程通过约束和修改进程的动态表现，创造一个边界，在linux中主要采用Cgroups技术实现资源限制；Namespace用来修改进程视图。容器的资源隔离相对于虚拟机是不彻底的，容器实例之间共同使用宿主机的操作系统内核；很多资源和对象是不能被Namespace化的。