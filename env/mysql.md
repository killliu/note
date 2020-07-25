# centos7

### 卸载清除

> yum remove mysql-community-server	

找出 mysql 软件包

> rpm -pa|grep mysql

使用 rpm -e pacakge_name 或 yum remove package_name 依次删除

<font color="darkgray">eg.  rpm -e pacakge_name</font>

找出 mysql 配置文件

> find / -name mysql

使用 rm -rf 依次删除配置文件 config_path

<font color="darkgray">eg. rm -rf config_path</font>

找出 MariaDB 软件包

> rpm -pa|grep mariadb

使用 rpm -e mariadb_name

<font color="red">**error: 提示被 postfix 需要**</font>

使用强制删除

> rpm -e --nodeps mariadb_name

### 安装

```shell
mkdir /root/mysql
cd /root/mysql
tar -vxf mysql-8.0.18-1.el7.x86_64.rpm-bundle.tar
rpm -ivh mysql-community-common-8.0.16-2.el7.x86_64.rpm
rpm -ivh mysql-community-libs-8.0.16-2.el7.x86_64.rpm
rpm -ivh mysql-community-client-8.0.16-2.el7.x86_64.rpm
rpm -ivh mysql-community-server-8.0.16-2.el7.x86_64.rpm
```

<font color="red">**error: libcrypto.so.10 is needed**</font>

> dnf install compat-openssl10

<font color="red">**error: mariadb-libs is obsoleted by mysql-community-libs-8.0.18-1.el7.x86_64**</font>

<font color="darkgray">清除之前安装过的依赖</font>

> yum remove mysql-libs

<font color="red">**error: net-tools is needed by mysql-community-server-8.0.18-1.el7.x86_64**</font>

> yum install net-tools

<font color="red">**error: perl is needed ...**</font>

> yum install -y perl

### 开启服务
```shell
systemctl start mysqld.service 				# 初始密码
cat /var/log/mysqld.log | grep password		# 登录 mysql
mysql -u root -p							# 修改初始密码
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'Rts123456()';
```
### 开机启动
```shell
systemctl enable mysqld.service
systemctl restart mysqld.service
```
### 创建数据库
```mysql
create database your_database_name;
use your_database_name;
set names utf8;
source /root/xxx.sql;
```