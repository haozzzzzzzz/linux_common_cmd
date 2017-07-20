# kill命令

信号：

INT, TERM 立刻终止
QUIT 平滑终止
USR1 重新打开日志文件
USR2 平滑重载所有worker进程并重新载入配置和二进制模块

示例：

```shell
kill -INT `cat /usr/local/php/var/run/php-fpm.pid` # 关闭
kill -USR2 `cat path/to/file.pid` # 重启
```

