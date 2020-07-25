# centos7
### 1. 安装
```shell
yum install vsftpd -y﻿
vi /etc/vsftpd/vsftpd.conf
...
anonymous_enable=NO
anon_upload_enable=YES
anon_mkdir_write_enable=YES
...
:wq
```
### 2. 添加用户
```
useradd klftp -g ftp
echo "123" | passwd --stdin klftp
# 禁用登陆权限，设置 ftp 路径
usermod -s /sbin/nologin klftp
chown klftp:ftp -R /home/klftp
```
<font color="darkgray"> 删除用户 userdel your_user_name 删除用户及用户组 userdel -f your_user_name</font>
### 3. 启动
```
systemctl enable vsftpd.service
systemctl start vsftpd.service
systemctl status vsftpd.service
```
### 3. 验证
资源管理器访问 ftp://192.168.1.134
### 4. 无法登录
```shell
vi /etc/pam.d/vsftpd
...
# 注释掉 auth required pam_shells.so
...
```
<font color="red">没有权限， 提示 200 227</font>
原因是227进入被动模式后面接的IP是ftp服务器的内网IP
<font color="darkgray">（eg. 172, 19, 28, 109, 39, 70 前四位是内网IP，39*256 + 70 = 10054 这个10054是ftp服务器被动模式下的等待端口）</font>
解决办法：

```shell
vi /etc/vsftpd/vsftpd.conf
...
listen=YES
listen_ipv6=NO
pasv_address=外网IP
...
:wq
```
<font color="red">阿里云 用户登陆后提示无权限</font>
规则添加 **1024/65535** 端口通行，或者
```
vi /etc/vsftpd/vsftpd.conf
...
pasv_enable=YES
allow_writeable_chroot=YES
pasv_address=外网IP
pasv_min_port=63521   # 自定义最小端口号
pasv_max_port=63531   # 自定义最大端口号
...
:wq
```
### 5. 删除win10本地ftp账户记录
cmd ---> regedit ---> HKEY_CURRENT_USER ---> Software ---> Microsoft ---> FTP
<font color="darkgray"> 删除对应的记录即可</font>