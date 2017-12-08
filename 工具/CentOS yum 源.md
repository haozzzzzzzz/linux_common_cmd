## EPEL

EPEL是企业版 Linux 附加软件包的简称，EPEL是一个由[Fedora](http://www.linuxidc.com/topicnews.aspx?tid=5)特别兴趣小组创建、维护并管理的，针对 红帽企业版 Linux(RHEL)及其衍生发行版(比如 [CentOS](http://www.linuxidc.com/topicnews.aspx?tid=14)、Scientific Linux、[Oracle](http://www.linuxidc.com/topicnews.aspx?tid=12) Enterprise Linux)的一个高质量附加软件包项目。

EPEL 的软件包通常不会与企业版 Linux 官方源中的软件包发生冲突，或者互相替换文件。EPEL 项目与 Fedora 基本一致，包含完整的构建系统、升级管理器、镜像管理器等等。



**下载并安装EPEL**

[root@localhost ~]# wget http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

[root@localhost ~]# rpm -ivh epel-release-latest-7.noarch.rpm

[root@localhost ~]# yum repolist      ##检查是否已添加至源列表