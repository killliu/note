# centos7

### 1. 查看环境

```shell
cat /etc/redhat-release
git --version
```

### 2. 下载gitlab包

> wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-12.10.11-ce.0.el7.x86_64.rpm

### 3. 安装依赖

> yum install -y policycoreutils-python

### 4. 安装 gitlab 及其相关软件包

> rpm -i gitlab-ce-12.10.11-ce.0.el7.x86_64.rpm

### 5. 修改端口

```shell
vi /etc/gitlab/gitlab.rb
...
external_url 'http://192.168.184.132:9527'   # ip:port的形式
...
:wq
gitlab-ctl reconfigure
```

### 6. 启动服务

> gitlab-ctl restart

### 7. 访问界面

http://192.168.184.132:9527 -> set password -> login -> root:password

### 8. 配置 ssh key

--> git bash

ssh-keygen -t rsa -C 'xxx@xxx.com'

然后一路回车(-C 参数是你的邮箱地址)﻿

  查看本机ssh公钥

cd ~/.ssh

ls

cat id_rsa.pub

多个密钥

\# ssh-keygen -t rsa -C "your_email@email.com" -f ~/.ssh/new_ssh_key_names

### 9. 配置 SourceTree

终端 ---> cd ~/.ssh/ ---> ssh-keygen -t rsa -C "your_username_or_email"

Enter file in which to save the key: your_new_key_name

将生成的文件用记事本打开并复制其所有内容 ~/.ssh/your_new_key_name.pub

打开网页添加信任（eg.）10.211.55.6 ---> 登陆后右上角 ---> settings ---> SSH Keys ---> 粘贴 ---> add key

将网站中库的http库路径复制 ---> source tree ---> 新建 ---> 从URL克隆

<font color="red"> error 提示想要使用储存在钥匙串...中的机密信息</font>

---> 打开钥匙串访问 ---> 双击对应的应用 eg. ... Access Key for  git ---> 访问控制 ---> 允许所有应用程序访问此项目 ---> 储存更改