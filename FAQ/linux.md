[下载地址](http://mirrors.aliyun.com/centos/7.7.1908/isos/x86_64/CentOS-7-x86_64-Minimal-1908.iso)

### 将本地目录上传到服务器

​      打开secureCRT并连接服务器

​      atl + p 打开始 sftp 窗口，将要上传的文件或目录拖入窗口即可

​      此方法会将文件上传至服务器的根目录 <font color="red">/root/</font> 

### shell 脚本找不到命令或文件

<font color="darkgray">( 在 windows 下编辑的 .sh 文件的格式为 dos 格式，而 linux 只能执行格式为 unix 格式的脚本 )</font>

​    以 xxx.sh 为例

```shell
vi xxx.sh
# click Esc button
:set ff=unix 
# 或
:set fileformat=unix
# click Esc button
:wq
```

​    重新执行脚本就正常了

### GLIBC 2.14 not found

```shell
wget http://ftp.gnu.org/gnu/glibc/glibc-2.14.tar.gz
tar -xzvf glibc-2.14.tar.gz
cd glibc-2.14
mkdir build                   
cd build
../configure --prefix=/usr/local/glibc-2.14
make & make install
```

### install nodejs

[nodejs 下载地址](https://nodejs.org/dist/v12.16.2/node-v12.16.2-linux-x64.tar.xz)

```shell
tar xf node-v12.16.2-linux-x64.tar.xz
ln -s /root/node-v12.16.2-linux-x64/bin/node /usr/local/bin/
ln -s /root/node-v12.16.2-linux-x64/bin/npm /usr/local/bin/
node -v
```

### install cmake

[下载地址](https://codeload.github.com/Kitware/CMake/tar.gz/v3.2.2)

```shell
tar -zvxf CMake-3.2.2.tar.gz
cd CMake-3.2.2.tar.gz
./bootstrap
gmake
make install
cmake --version
```

<font color="red">error: ./bootstrap 时 cmake: /usr/lib64/libstdc++.so.6: version `GLIBCXX_3.4.15' not found</font>

检查版本 

> strings /usr/lib64/libstdc++.so.6|grep GLIBCXX

在当时make的目录下找到如下文件：

gcc-build-4.8.2/x86_64-unknown-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.18,

strings libstdc++.so.6.0.18| grep GLIBCXX

把该文件拷贝到了/usr/lib64下

```shell
cp libstdc++.so.6.0.18 /usr/lib64/libstdc++.so.6.0.18
cd /usr/lib64/
rm -f libstdc++.so.6
ln -s libstdc++.so.6.0.18 libstdc++.so.6
```

<font color="red">error: /usr/bin/ld: cannot find -lmysqlclient</font>

```shell
cp /root/libmysqlclient.a /usr/lib64/mysql/
cp /root/libmysqlservices.a /usr/lib64/mysql/
```

### install ffmpeg

**x264**

```shell
wget https://code.videolan.org/videolan/x264/-/archive/master/x264-master.tar.bz2
tar jxvf x264-master.tar.bz2
cd x264-master.tar.bz2
./configure --enable-shared
# error: Found no assembler
# error: Minimum version is nasm-2.13
cd ..
yum install -y yasm
wget https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/nasm-2.14.02.tar.gz
tar -zxvf nasm-2.14.02.tar.gz
cd nasm-2.14.02
./configure make && make install
cd ..
cd x264-master
make && make install
```

**[官网](http://www.ffmpeg.org/)**

```shell
wget https://ffmpeg.org/releases/ffmpeg-4.2.2.tar.bz2
tar jxvf ffmpeg-4.2.2.tar.bz2
# error: bzip2: Cannot exec: No such file or directory 原因是没有bzip2解压工具
yum -y install bzip2
./configure --enable-gpl --enable-libx264
# error: yasm/nasm not found or too old
yum -y install yasm
make
make install
# error while loading shared libraries: libx264.so.160: cannot open shared object file: No such file or directory
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
export CFLAGS=/usr/local/include:$CFLAGS
```

### install nginx & stmp

```shell
mkdir nginx
cd nginx
wget http://nginx.org/download/nginx-1.8.1.tar.gz
tar -zxvf nginx-1.8.1.tar.gz
wget https://github.com/arut/nginx-rtmp-module/archive/master.zip
unzip master.zip
./configure --with-http_ssl_module --add-module=../nginx-rtmp-module-master
make 
# error: configure: error: the HTTP rewrite module requires the PCRE library
# 安装pcre-devel与openssl-devel解决问题
yum -y install pcre-devel openssl openssl-devel
sudo make install
# 这时 nginx 将会被安装到目录：/usr/local/nginx directory，启动测试：
sudo /usr/local/nginx/sbin/nginx
# 在浏览器中输入当前服务器 IP，可以看到 Welcom to nginx! 页面
# 停止 nginx 服务
sudo /usr/local/nginx/sbin/nginx -s stop
# 增加 RTMP 模块
cd /usr/local/nginx/conf
vi nginx.conf
```

在最后加入如下配置：

```shell
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       80;
        server_name  localhost;
        location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
        # ------------ 以下为增加部分 ------------ 
        location /hls {
            types {
                application/vnd.apple.mpegurl m3u8; 
                video/mp2t ts;
            }
            alias /usr/local/nginx/hls/;
            expires -1;
            add_header Cache-Control no-cache;
        }
        #location /flv {
        #    flv_live on;
        #    chunked_transfer_encoding on;
        #}
    }
}

rtmp {
    server {
        listen 1935; #监听的端口
        chunk_size 4000;
        application live { #rtmp推流请求路径
            live on;
            record off;
            #publish_notify on; #推流验证，和下一行需一起使用
            #on_publish http://localhost/auth.php; #推流验证，具体可参照后文
            hls on;
            hls_path /usr/local/nginx/hls/test; #视频流hls切片文件目录(自己创建)
            hls_fragment 3s; #hls单个切片时长，会影响延迟
            hls_playlist_length 32s; #hls总缓存时间，会影响延迟
            meta on; #如果http-flv播放时首帧出现问题，可改为off
            #gop_cache on; #可以减少首帧延迟
        }
        application vod {
            play /usr/local/nginx/hls;
        }
    }
}
```

继续

```shell
cd ..
mkdir rtmp     # 这里存放媒体文件 flv mp4
sudo /usr/local/nginx/sbin/nginx
```

FFmpeg 推流

> ffmpeg -re -stream_loop -1 -i /usr/local/nginx/rtmp/end.mp4 -vcodec libx264 -acodec aac -f flv rtmp://localhost/live/test

test.html

```html
<html>
<body>
    <video autoplay webkit-playsinline controls>
        <source src="http://192.168.152.128/hls/test.m3u8" type="application/vnd.apple.mpegurl" />
        <p class="warning">Your browser does not support HTML5 video.</p>
    </video>
    <video controls>
        <source src="http://192.168.152.128/hls/test.m3u8" type="application/x-mpegURL">
    </video>
</body>
</html>
```

### 防火墙

```shell
systemctl stop firewalld.service
systemctl disable firewalld.service
```

### vmware 网络设置

```shell
cd /etc/sysconfig/network-scripts
vi ./ifcfg-ens33
...
ONBOOT=yes
...
:wq
init 6
```

<font color="darkgray">vmware -> 编辑 -> 虚拟网络编辑器 -> VMnet8 -> 子网IP -> 当前系统IP（eg. 10.15.119.0）</font>

### init

0：	关机

1：	进入单用户模式（系统可运行的最低软硬件配置）

2：	进入无网络服务的多用户模式

3：	进入控制台登录的多用户模式

4：	进入控制台登录的多用户模式

5：	 进入图形化登录的多用户模式

6：	重启

### yum

​      search                      查找

​      list                         列出要查找的列表

​      info                        信息

​      updates                    可更新的

​      installed                     已安装的软件包

​      extras                      已安装但不在 yum repository 内的包 

​      provides                    提供哪些文件

​      eg.  <font color="coral">yum info installed rpm</font>   列出已安装的 rpm 软件包相关的信息

### rpm

```shell
用法: rpm [选项...]
-a：查询所有套件；
-b<完成阶段><套件档>+或-t <完成阶段><套件档>+：设置包装套件的完成阶段，并指定套件档的文件名称；
-c：只列出组态配置文件，本参数需配合"-l"参数使用；
-d：只列出文本文件，本参数需配合"-l"参数使用；
-e<套件档>或--erase<套件档>：删除指定的套件；
-f<文件>+：查询拥有指定文件的套件；
-h或--hash：套件安装时列出标记；
-i：显示套件的相关信息；
-i<套件档>或--install<套件档>：安装指定的套件档；
-l：显示套件的文件列表；
-p<套件档>+：查询指定的RPM套件档；
-q：使用询问模式，当遇到任何问题时，rpm指令会先询问用户；
-R：显示套件的关联性信息；
-s：显示文件状态，本参数需配合"-l"参数使用；
-U<套件档>或--upgrade<套件档>：升级指定的套件档；
-v：显示指令执行过程；
-vv：详细显示指令执行过程，便于排错。
```

示例

```shell
rpm -qa                               # 列出所有安装过的包
rpm -ivh your-package                 # 直接安装
rpm --force -ivh your-package.rpm     # 忽略报错，强制安装
rpm -ql tree                          # 查询
rpm -e tree                           # 卸载
rpm -ql tree                          # 查询
```

### 各版本区别及下载地址

CentOS-xxxx-DVD.iso            		标准安装版
CentOS-xxxx-NetInstall.iso       	网络安装镜像
CentOS-xxxx-Everything.iso         对完整版安装盘的软件进行补充，集成所有软件。
CentOS-xxxx-GnomeLive.iso        GNOME桌面版
CentOS-xxxx-KdeLive.iso              KDE桌面版
CentOS-xxxx-livecd.iso           	  光盘上运行的系统，类拟于winpe

DVD ISO       	 ：标准安装盘，一般下载这个就可以了（4G左右）
Everything ISO ：对完整版安装盘的软件进行补充，集成所有软件（8G左右）
Minimal ISO      ：最小安装盘，只有必要的软件，自带的软件最少（1G左右）

CentOS-xxxx-LiveCD.ios      只能加载到内存里运行，不能安装 
CentOS-xxxx-bin-DVD.iso    镜像安装文件
CentOS-xxx-bin-DVD1.iso    镜像安装文件
CentOS-xxx-bin-DVD2.iso    一些软件

<font color="red">设置 yum 源</font>
备份： mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo_bak
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

下载地址：

​	https://www.centos.org/download/mirrors/
​       http://mirrors.aliyun.com/centos/             		      阿里
​       http://mirrors.cqu.edu.cn/CentOS/            		     重庆大学
​       https://mirrors.cqu.edu.cn/CentOS/           		    重庆大学
​       http://mirrors.cn99.com/centos/              		       cn99
​       http://mirrors.neusoft.edu.cn/centos/         		   大连东软信息学院
​       http://mirrors.huaweicloud.com/centos/      	     华为
​       https://mirrors.huaweicloud.com/centos/     	    华为
​       http://mirror.jdcloud.com/centos/            		      京东云
​       https://mirror.jdcloud.com/centos/
​       http://mirror.lzu.edu.cn/centos/              		        兰州大学开源协会
​       http://mirrors.nju.edu.cn/centos/             		       南京大学
​        http://mirrors.njupt.edu.cn/centos/           		    南京邮电大学       
​       https://mirrors.njupt.edu.cn/centos/
​       http://mirrors.163.com/centos/               		        163
​       http://ftp.sjtu.edu.cn/centos/                 			      上海交通大学
​       https://ftp.sjtu.edu.cn/centos/
​       http://ap.stykers.moe/centos/                 			    SysKiller Dev
​       https://ap.stykers.moe/centos/
​       http://mirrors.tuna.tsinghua.edu.cn/centos/    	清华大学
​       https://mirrors.tuna.tsinghua.edu.cn/centos/
​       [rsync://mirrors.tuna.tsinghua.edu.cn/centos/](rsync://mirrors.tuna.tsinghua.edu.cn/centos/)
​       http://centos.ustc.edu.cn/centos/              			中国科技大学
​       http://mirrors.zju.edu.cn/centos/               		    浙江大学
​       https://mirrors.zju.edu.cn/centos/    
