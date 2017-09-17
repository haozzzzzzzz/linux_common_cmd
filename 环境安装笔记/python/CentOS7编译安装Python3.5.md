# CentOS7编译安装Python3.5

[TOC]

## 安装步骤

### 准备编译环境

```shell
 yum groupinstall 'Development Tools'
 yum install zlib-devel bzip2-devel openssl-devel ncurese-devel
```

### 下载Python3.5源码

去[Python下载页](Python下载页)选择指定版本源码，这里选择的是[Python3.5.3](https://www.python.org/downloads/release/python-353/)。

```shell
wget https://www.python.org/ftp/python/3.5.3/Python-3.5.3.tar.xz
```

### 编译

解压并进入源码目录后，执行一下编译命令

```shell
./configure --prefix=/usr/local/python3
make
make test
make install
```

