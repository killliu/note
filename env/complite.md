[centos-6.10-x86_64-minimal.iso﻿](https://pan.baidu.com/s/1jPic6wMuAYUn4c4XS2LF0A)   密码:  0gbw
[gcc-4.9.4.tar.gz](https://pan.baidu.com/s/14I1DVRv9H_RnPqeTfqOMuw)   密码：06xe

### 1. 设置网络
```shell
vi /etc/sysconfig/network-scripts/ifcfg-eth0
...
ONBOOT=yes
...
:wq
Init 6
```
### 2. 安装工具集
```shell
yum groupinstall "Development Tools"
yum -y install wget
yum -y install mysql-server
yum -y install mysql-devel
tar zvxf gcc-4.9.4.tar.gz
cd gcc-4.9.4
./contrib/download_prerequisites
# 编译安装
./configure --prefix=/usr/local/gcc --enable-checking=release --enable-languages=c,c++ --disable-multilib

make -j4
make install
ln -bfsv /usr/local/gcc/bin/gcc /usr/bin/gcc
ln -bfsv /usr/local/gcc/bin/g++ /usr/bin/g++
vi ~/.bashrc	# 追加
...
LD_LIBRARY_PATH=/usr/local/gcc/lib64:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH
...
:wq
```
### 4. 编译
注意权限问题 