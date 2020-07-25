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