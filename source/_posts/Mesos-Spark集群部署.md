title: Mesos-Spark集群部署
date: 2015-12-13 22:51:21
## tags:
---


ubuntu 15

spark1.5.2

mesos 0.25

``` shell
sudo apt-get update

sudo apt-get instsall -y openjdk-7-jdk
sudo apt-get -y install build-essential python-dev python-boto libcurl4-nss-dev libsasl2-dev maven libapr1-dev libsvn-dev


```

可能还需要一个依赖,

``` 
sudo apt-get -y install libz-dev
```



解压下载好的压缩包, 创建一个build文件夹, 并且在自己现在安装mesos的路径创建一个名为mesos的文件夹. 进入build目录 执行configure 并且指定mesos安装路径 --prefix={mesos路径}

``` 
tar -zxvf mesos-0.25.0.tar.gz
cd /data/software/mesos-0.25.0
mkdir /data/software/mesos
/data/software/mesos-0.25.0/bootstrap
mkdir -p /data/software/mesos-0.25.0/build
cd /data/software/mesos-0.25.0/build
/data/software/mesos-0.25.0/configure   --prefix=/data/software/mesos
```

configure执行成功之后便开始make



