# Swoole安装配置

[TOC]

# 相关链接

* [github](https://github.com/swoole/swoole-src)

# 安装

可以通过`pecl`和源码安装，为了方便，这里通过`pecl`进行安装。

```bash
pecl install swoole
```

安装完成后，修改php.ini文件添加`extension=swoole.so`，然后执行重启php-fpm

```shell
systemctl restart php-fpm
```

