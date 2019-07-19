# Centos查看和修改ulimit最大进程数和最大文件打开数

```
ulimit -n 每个进程可以同时打开的最大文件数
ulimit -u 可以运行的最大并发进程数
```



使用ulimit -n 204800修改只对当前登陆有效。

永久修改limits，修改/etc/security/limits.conf

```
* soft nofile 204800
* hard nofile 204800
* soft nproc 204800
* hard nproc 204800
 
*             代表针对所有用户 
noproc     是代表最大进程数 
nofile     是代表最大文件打开数
--------------------- 
作者：xzw_123 
来源：CSDN 
原文：https://blog.csdn.net/xzw_123/article/details/46878459 
版权声明：本文为博主原创文章，转载请附上博文链接！
```



## systemd.service

systemd的最大打开文件数和最大进程数是独立设置的，所以定义的systemd.service需要设置最大打开文件数和最大进程数。

- systemctl status 查看进程的PID
- cat /proc/<PID>/limits查看最大打开进程数和最大文件数



修改systemd配置文件,/etc/systemd/system.conf

设置

```
DefaultLimitCORE=infinity
DefaultLimitNOFILE=100000
DefaultLimitNPROC=100000
```
重启系统后生效



也可以只对systemd.service进行修改

```
[Service]
LimitCORE=infinity
LimitNOFILE=100000
LimitNPROC=100000
```

重启进程即可