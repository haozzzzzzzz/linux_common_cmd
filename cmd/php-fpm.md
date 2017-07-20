# php-fpm

查看进程数: 

```shell
ps aux | grep -c php-fpm
```

查看进程

```shel
ps aux | grep php-fpm
```

杀死进程

```shell
kill -INT `cat /var/run/php-fpm/php-fpm.pid` # 关闭
kill -9 <PID>
kill -USR2 `cat path/to/php-fpm.pid` # 重启
```

