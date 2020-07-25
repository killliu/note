# centos7
### 1. 准备资源
安  装  包：golang、leanote、mongodb
下载地址：[百度云](https://pan.baidu.com/s/1moX0eGbL17qG4KEHpZUBPA)   密码：jhrw
/home/go1.12.5.linux-amd64.tar.gz
/home/leanote-all-master
/home/mongodb-linux-x86_64-3.0.1.tgz
### 2. 安装 go
```shell
cd /home
tar -zvxf  go1.12.5.linux-amd64.tar.gz
mkdir go_install
vi /etc/profile						# 设置环境变量
...
exportGO111MODULE=on
exportGOPROXY=https://goproxy.cn
#orexportGOPROXY=https://mirrors.aliyun.com/goproxy/
exportGOROOT=/home/go
exportGOPATH=/home/go_install
exportPATH=$PATH:$GOROOT/bin:$GOPATH/bin
...
:wq
source /etc/profile
go version
```
### 3. leanote
```shell
yum -y install unzip
unzip leanote-all-master.zip
cp -r /home/leanote-all-master/src /home/go_install
go install github.com/revel/cmd/revel
```
### 4. mongodb
```shell
tar -zvxf mongodb-linux-x86_64-3.0.1.tgz
vi /etc/profile					# 设置环境变量
...
export PATH=$PATH:/home/mongodb-linux-x86_64-3.0.1/bin
...
:wq
source /etc/profile
mkdir /home/data				# 新建目录 /home/data 存放数据
# 后台启动
mongod --dbpath /home/data --logpath /home/leanote.log --logappend --fork --port 27017
# 导入数据
mongorestore -h localhost -d leanote --dir /home/go_install/src/github.com/leanote/leanote/mongodb_backup/leanote_install_data 
# 查看是否导入成功
mongo
> show dbs
# 修改 leanote 配置文件 app.conf
# 请务必修改`app.secret`一项, 在若干个随机位置处，将字符修改成一个其他的值, 否则会有安全隐患!
```
### 5. 启动
> revel run github.com/leanote/leanote

[参考网站：Leanote 二进制版详细安装教程 Mac and Linux](https://github.com/leanote/leanote/wiki/Leanote-二进制版详细安装教程----Mac-and-Linux) 