# logrotate配置

示例

配置目录在/etc/logrotate.d/下

```shell
/var/log/service/event_report/run.log {
    size 50M
    rotate 10
    notifempty
    copytruncate
    create 0644 root root
    dateformat -%Y%m%d.%s
}
```

