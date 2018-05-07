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
- reload 重新加载配置re