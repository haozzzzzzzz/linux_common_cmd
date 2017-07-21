# Nginx内建状态页启用

[TOC]

## 在站点配置中添加location
```nginx
server{
    location /nginx_status{
        stub_status on;
        access_log off;
    }
}
```
##测试
````shell
curl 127.0.0.1/nginx_status
````
返回结果
````shell
[root@localhost conf.d]# curl 127.0.0.1/nginx_status
Active connections: 1 
server accepts handled requests
 1 1 1 
Reading: 0 Writing: 1 Waiting: 0
````
**参数解析**
active connections – 活跃的连接数量
server accepts handled requests — 总共处理了11989个连接 , 成功创建11989次握手, 总共处理了11991个请求
reading — 读取客户端的连接数.
writing — 响应数据到客户端的数量
waiting — 开启 keep-alive 的情况下,这个值等于 active – (reading+writing), 意思就是 Nginx 已经处理完正在等候下一次请求指令的驻留连接.

