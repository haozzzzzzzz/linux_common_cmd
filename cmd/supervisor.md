###supervisord
```
sudo supervisorcd -c /etc/supervisor/supervisord.conf
```
###supervisorctl
```
sudo supervisorctl update #更新配置，不重启已有进程
sudo sueprvisorctl reload #重新加载配置并重启所有进程
sudo supervisorctl start clrsrvhttp
```
- start 启动
- stop 关闭
- status 状态
- tail 日志
- reload 重新加载配置



示例

```
[program:2048-web]
command         = /home/www/2048/prod/web/web --config=prod.yaml
directory       = /home/www/2048/prod/web/
startsecs       = 3

autostart       = true
autorestart     = true

redirect_stderr                 = true
stdout_logfile_maxbytes = 50MB
stdout_logfile_backups  = 10
stdout_logfile                  = /home/www/2048/prod/web/logs/web.log
```

