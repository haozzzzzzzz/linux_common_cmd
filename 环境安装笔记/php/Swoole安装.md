# Swoole安装配置

[TOC]

# 相关链接

* [github](https://github.com/swoole/swoole-src)

# 安装

可以通过`pecl`和源码安装，为了方便，这里通过`pecl`进行安装。

```bash
pecl install swoole
```

安装完成后，修改php.ini文件添加`extension=swoole.so`，然后执行重启php-fpm

```shell
systemctl restart php-fpm
```

## 测试

创建一个websocket服务，并测试。

**websocket_server.php**

````php
<?php
/**
 * Created by PhpStorm.
 * User: Administrator
 * Date: 2017/7/21 0021
 * Time: 16:48
 */
//创建websocket服务器对象，监听0.0.0.0:9502端口
$ws = new swoole_websocket_server("0.0.0.0", 9502);

//监听WebSocket连接打开事件
$ws->on('open', function ($ws, $request) {
    var_dump($request);
    $ws->push($request->fd, "hello, welcome\n");
});

//监听WebSocket消息事件
$ws->on('message', function ($ws, $frame) {
    echo "Message: {$frame->data}\n";
    $data = $frame->data;
    var_dump($frame);
    $ws->push($frame->fd, "server: $data");
});

//监听WebSocket连接关闭事件
$ws->on('close', function ($ws, $fd) {
    echo "client-{$fd} is closed\n";
});

$ws->start();
````

**websocket_client.html**

````php
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>websocket测试</title>
</head>
<body>
    <button id="btn-send">发送消息</button>
    <button id="btn-close">关闭链接</button>

    <script>
        var wsServer = 'ws://10.5.1.110:9502/12312345646';
        var websocket = new WebSocket(wsServer);

        websocket.onopen = function (evt) {
            console.log("Connected to WebSocket server.");

        };

        websocket.onclose = function (evt) {
            console.log("Disconnected");

        };

        websocket.onmessage = function (evt) {
            console.log('Retrieved data from server: ' + evt.data);

        };

        websocket.onerror = function (evt, e) {
            console.log('Error occured: ' + evt.data);

        };

        // 按钮
        var btnSend = document.getElementById('btn-send');
        var btnClose = document.getElementById('btn-close');

        // 发送消息
        btnSend.addEventListener('click', function(){
            console.log('To server: Hello');
            websocket.send('Hello');

        });

        // 关闭链接
        btnClose.addEventListener('click', function(){
            console.log('Close connection.');
            websocket.close();

        });

    </script>

</body>
</html>
````

使用php启动websocket_server，然后打开websocket_client，查看server和client的输出。

````shell
php websocket_server.php
````

