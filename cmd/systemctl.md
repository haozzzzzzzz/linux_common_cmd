# systemctl

## 命令

```shell
systemctl start php-fpm.service # 开启
systemctl stop php-fpm.service # 结束
systemctl status php-fpm # 运行状态
systemctl enable php-fpm.service # 开机启动

systemctl start nginx.service              #启动nginx服务
systemctl enable nginx.service             #设置开机自启动
systemctl disable nginx.service            #停止开机自启动
systemctl status nginx.service             #查看服务当前状态
systemctl restart nginx.service　          #重新启动服务
systemctl list-units --type=service        #查看所有已启动的服务

systemctl --system daemon-reload # 更改配置后，重新加载配置
```



## service配置

* [systemd.service 中文手册](http://www.jinbuguo.com/systemd/systemd.service.html)

`.service`文件存放在`/lib/systemd/system`目录下。

例如，php-fpm.service

```ini
[Unit]
Description=php-fpm 7
After=network.target

[Service]f
Type=forking
PIDFile=/usr/local/php7/var/run/php-fpm.pid
ExecStart=/usr/local/php7/sbin/php-fpm
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s TERM $MAINPID

[Install]
WantedBy=multi-user.target
```

然后可以使用systemctl进行管理。