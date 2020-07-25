# centos7

```shell
rpm -qa subversion 			# 检查是否安装
yum remove subversion		# 卸载
yum -y install subversion	# 安装
svnserve --version			# 查看版本
cd /
mkdir /svn/
svnadmin create /svn/repo 	# 创建repo库
############# **修改配置** #############
vi /svn/repo/conf/authz		# 修改权限
[groups]
admin = user_name
[repo:/]
@admin = rw
vi /svn/repo/conf/svnserve.conf
[general]
anon-access = none     		# 匿名访问的权限，可以是read,write,none,默认为read
auth-access = write      	# 使授权用户有写权限
password-db = passwd  		# 密码数据库的路径
authz-db = authz        	# 访问控制文件
realm = /svn/repo       	# 认证命名空间，subversion会在认证提示里显示，并且作为凭证缓存的关键字
############# **启动** #############
svnserve -d -r /svn/ 		# 启动
killall svnserve			# 关闭
error: killall: command not found 
yum install psmisc			# fix killall command not found

svn checkout svn://127.0.0.1/repo
############# **开机启动** #############
vi /usr/lib/systemd/system/svnserve.service
...
ExecStart=/usr/bin/svnserve --daemon --pid-file=/run/svnserve/svnserve.pid -d -r /svn/ --listen-port 3690
...
:wq 
vi /etc/selinux/config		# 关闭selinux
SELINUX=disabled
:wq
systemctl enable svnserve.service
```

