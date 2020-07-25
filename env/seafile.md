# centos7
**https://github.com/haiwen/seafile**
```shell
yum install -y wget vim
systemctl stop firewalld
systemctl disable firewalld
######### 安装 python mysql mariadb #########
yum install python python-setuptools python-imaging python-ldap python-memcached MySQL-python mariadb mariadb-server
######### 启动 MariaDB 服务 #########
sudo systemctl start mariadb.service sudo systemctl enable mariadb.service
/usr/bin/mysql_secure_installation				# 配置 MySQL
回车
Password
Password
一直回车直到结束
######### 安装 SeaFile #########
wget https://mc.qcloudimg.com/static/archive/3d8addbe52be88df4f6139ec7e35b453/seafile-server_5.1.4_x86-64.tar.gz
tar -zxvf seafile-server_5.1.4_x86-64.tar.gz
sudo mkdir -p /opt/seafile/installed
sudo mv seafile-server_5.1.4_x86-64.tar.gz /opt/seafile/installed
sudo mv seafile-server-5.1.4/ /opt/seafile
cd /opt/seafile/seafile-server-5.1.4
sudo ./setup-seafile-mysql.sh
# 按提示输入，选择默认直接回车进入下一步， seafile 安装路径：/opt/seafile/seafile-server-5.1.4
sudo ./seafile.sh start
sudo ./seahub.sh start
[ admin email ] [killliu@seafile.com](mailto:killliu@seafile.com)
[ admin password ] ***********
```
http://192.168.147.128:8000/