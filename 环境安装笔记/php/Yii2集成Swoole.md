# Yii2集成Swoole

将swoole websocket server集成到yii2的console应用中。

配置参数:

````php
<?php

$swoole_websocket_params = [
    'daemonize' => 1, //守护进程
    'pid_file' => '/var/run/php-swoole-websocket.pid', //进程id文件

    'worker_num' => 2,// worker 进程数
    'max_request' => 3, // 单个worker进程处理请求数
    'max_con' => 10000, //服务器程序最大连接数
    'log_file' => Yii::getAlias('@runtime/log/websocket.log'), //日志文件
    'log_level' => 2, //日志打印级别 INFO

];

//区分环境进行配置
if( RUNNING_INNER_NET ){
    $swoole_websocket_params['log_level'] = 0; // DEBUG

}else{
    //..

}

return $swoole_websocket_params;
````



````php
<?php
/**
 * Created by PhpStorm.
 * User: Administrator
 * Date: 2017/7/24 0024
 * Time: 13:49
 */
namespace console\controllers;

use Swoole\WebSocket\Server;
use yii\console\Controller;

/**
 * websocket服务器
 * Class WebsocketController
 * @package console\controllers
 */
class WebsocketController extends Controller{

    protected $setting = [];
    public $defaultAction = 'runserver';

    public function __construct($id, $module, $config = []){
        parent::__construct($id, $module, $config);

        $this->setting = require(\Yii::getAlias('@common/config/swoole/websocket.php'));

    }

    /**
     * 运行服务器
     */
    public function actionRunserver($host="0.0.0.0", $port=9502){
        $ws = new Server($host, $port);

        $ws->set($this->setting);

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

        echo "Websocket server is running on $host:$port.\n";

        // 启动
        $ws->start();

    }

}

````

创建系统服务php-swoole-websocket.service。

````ini
[Unit]
Description=php swoole websocket.
After=network.target

[Service]
Type=forking
PIDFile=/var/run/php-swoole-websocket.pid
ExecStart=/usr/local/php7/bin/php /mnt/hgfs/htdocs/morefans/web/game/web/branch/v1.0-2048/yii websocket/runserver
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s TERM $MAINPID

[Install]
WantedBy=multi-user.target
````

这样既可以通过`php yii websocket/runserver`启动websocket服务，也可以通过`systemctl start php-swoole-websocket`启动。

##测试
````html
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
        var wsServer = 'ws://10.5.1.110:9502';
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
            var data = {
                'type': '123',
                'data': 100
            };
            websocket.send(JSON.stringify(data));

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

