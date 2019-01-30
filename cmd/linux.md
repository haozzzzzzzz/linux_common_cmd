### 修改ulimit

临时`ulimit -n 1024`

永久在`/etc/security/limits.conf`中添加

```conf
* soft nofile 10240
* hard nofile 10240
```

