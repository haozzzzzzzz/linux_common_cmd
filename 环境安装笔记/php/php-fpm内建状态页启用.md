# php-fpm内建状态页启用

[TOC]

## 启用php-fpm状态功能

打开php-fpm的`www.conf`文件，找到`pm.status_path`配置项，设置状态页的路径。

````ini
pm.status_path = /status
````

然后重启php-fpm

````shell
systemctl restart php-fpm
````

## 配置nginx

打开站点配置文件，添加状态页路径。

````nginx
server{
    # (optional) php-fpm status page
    location ~ ^/(status|ping)$ {
        include fastcgi_params;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_param SCRIPT_FILENAME $fastcgi_script_name;
    }
}
````

重启nginx

````shell
nginx -t # 测试配置文件
systemctl restart nginx
````

## 测试

````shell
curl 127.0.0.1/status
````

返回结果

````shell
pool:                 www
process manager:      dynamic
start time:           21/Jul/2017:14:23:40 +0800
start since:          76
accepted conn:        1
listen queue:         0
max listen queue:     0
listen queue len:     128
idle processes:       1
active processes:     1
total processes:      2
max active processes: 1
max children reached: 0
slow requests:        0
````

**参数说明**

pool – fpm池子名称，大多数为www
process manager – 进程管理方式,值：static, dynamic or ondemand. dynamic
start time – 启动日期,如果reload了php-fpm，时间会更新
start since – 运行时长
accepted conn – 当前池子接受的请求数
listen queue – 请求等待队列，如果这个值不为0，那么要增加FPM的进程数量
max listen queue – 请求等待队列最高的数量
listen queue len – socket等待队列长度
idle processes – 空闲进程数量
active processes – 活跃进程数量
total processes – 总进程数量
max active processes – 最大的活跃进程数量（FPM启动开始算）
max children reached - 大道进程最大数量限制的次数，如果这个数量不为0，那说明你的最大进程数量太小了，请改大一点。
slow requests – 启用了php-fpm slow-log，缓慢请求的数量

更多返回格式请参考：

* [启用php-fpm状态详解](http://www.ttlsa.com/php/use-php-fpm-status-page-detail/)

