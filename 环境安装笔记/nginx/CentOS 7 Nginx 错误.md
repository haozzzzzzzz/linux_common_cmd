# CentOS 7 Nginx 错误

[TOC]

## 使用systemctl启动时报错

## /var/run/nginx.pid

```shell
[root@localhost system]# systemctl status nginx.service
● nginx.service - nginx - high performance web server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; vendor preset: disabled)
   Active: failed (Result: exit-code) since Fri 2017-07-21 09:36:55 CST; 28min ago
     Docs: http://nginx.org/en/docs/
  Process: 2979 ExecStartPre=/usr/sbin/nginx -t -c /etc/nginx/nginx.conf (code=exited, status=1/FAILURE)

Jul 21 09:36:54 localhost.localdomain systemd[1]: Starting nginx - high performance web server...
Jul 21 09:36:54 localhost.localdomain nginx[2979]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
Jul 21 09:36:54 localhost.localdomain nginx[2979]: nginx: [emerg] open() "/var/run/nginx.pid" failed (13: Permission denied)
Jul 21 09:36:54 localhost.localdomain nginx[2979]: nginx: configuration file /etc/nginx/nginx.conf test failed
Jul 21 09:36:55 localhost.localdomain systemd[1]: nginx.service: control process exited, code=exited status=1
Jul 21 09:36:55 localhost.localdomain systemd[1]: Failed to start nginx - high performance web server.
Jul 21 09:36:55 localhost.localdomain systemd[1]: Unit nginx.service entered failed state.
Jul 21 09:36:55 localhost.localdomain systemd[1]: nginx.service failed.

```

**原因**

/var/run/nginx.pid的权限不足

**解决方法**

```shell
touch /var/run/nginx.pid # 如果没有则先创建一个
chown nginx:nginx /var/run/nginx.pid # 将文件的所属者改为 nginx:nginx
```

重启后，nginx.service会自动启动

