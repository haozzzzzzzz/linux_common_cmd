# Nginx安装配置

[TOC]

## 配置（可选）

一些有利于管理和维护的配置。

### 创建sites-available和sites-enabled

在`/etc/nginx`里创建两个文件夹`sites-available`、`sites-enabled`

```shell
mkdir sites-available
mkdir sites-enabled
```

将`sites-enabled`的配置文件加入到`nginx.conf`，修改后如

```nginx
http {
	# ..
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}

```

**用法**

在`sites-available`创建可供访问的站点，然后将在`sites-enabled`添加链接。拿`/etc/nginx/sites-available/localtest.conf`为例，在`sites-enabled`中创建软连接。

```shell
ln -s ../sites-available/localtest.conf ./localtest.conf
```

