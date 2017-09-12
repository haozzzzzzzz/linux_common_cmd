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



## 资料

* [systemd.service 中文手册](http://www.jinbuguo.com/systemd/systemd.service.html)