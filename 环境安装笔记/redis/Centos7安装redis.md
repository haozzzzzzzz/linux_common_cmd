# Centos7 安装Redis

[TOC]

## 使用源进行安装

已安装`EPEL`的，可以使用以下命令进行安装

````shell
yum -y install redis
````



### 配置远程访问

```shell
whereis redis #查找redis.conf文件
在redis.conf后将127.0.0.1改为0.0.0.0访问
```

