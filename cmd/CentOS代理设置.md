# CentOS设置代理服务器

编辑`/etc/profile`，设置参数

```shell
all_proxy=http://xxx:xxx
ftp_proxy=http://xxx:xxx
http_proxy=http://xxx:xxx
https_proxy=http://xxx:xxx
no_proxy=localhost,127.0.0.1
export all_proxy
..
```



使配置生效

```shell
source /etc/profile
```



## yum代理服务器配置

编辑`/etc/yum.conf`添加`proxy=socks5://xxx:xx`或`proxy=http://xxx:xxx`