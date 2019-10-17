---
layout: post
title: "使用GatewayWorker实现“三端互联"
subtitle: "使用GatewayWorker实现“三端互联”"
author: "Twany"
header-img: "img/bg-tec/post-bg-universe.jpg"
header-mask: 0.4
catalog: true
tags:
  - Workerman
  - GatewayWorker
---

##### 之前使用swoole 和workman，因水平有限，并为实现需求。经过一番百度之后，发现了基于workerman的 GatewayWorker，以下为 GatewayWorker 的简介：
> GatewayWorker基于Workerman开发的一个项目框架，用于快速开发TCP长连接应用，例如app推送服务端、即时IM服务端、游戏服务端、物联网、智能家居等等

详细可查看 [GatewayWorker 官网](https://www.workerman.net/doc)


## 需求分析
![](https://i.loli.net/2019/07/22/5d358ffba00ef70244.png)
如图，需要做到：
- 硬件可以给服务器发送数据
- 微信小程序控制硬件

较容易实现的是第一个，直接使用tcp就可以。但是第二个问题呢？微信小程序如何连接到指定硬件设备

## 解决方案
##### 从GatewayWorker官网下载一个简易的聊天室Demo，使用了tcp协议，功能是多个telnet可进行聊天，消息实时共享。我们在此基础上改进。
#### 目录结构
把YourAPP复制一份，并命名为`WSServer`
![](https://i.loli.net/2019/07/22/5d359e976eec692157.png)

#### `YourApp` 文件夹
在`YourApp`中，也就是`TcpServer`,Tcp端口使用`8282`，服务注册端口为`1238`

Gateway类型为tcp:

`$gateway = new Gateway("tcp://0.0.0.0:8282");`

#### `WSServer`文件夹
在`WSServer`文件夹中，WebSocket端口使用`8283`,服务注册端口**依旧**为`1238`

Gateway类型为websocket:

`$gateway = new Gateway("websocket://0.0.0.0:8283");`

#### 数据共享
我们建立了两个服务器，一个Tcp，一个WebSocket。那么如果实现通信呢？

解决方案就是**在WebSocket服务器内创建一个tcp 客户端**即可，代码如下：
```php
// 文件路径 /WSServer/Events.php
public static function onMessage($client_id, $message)
{
    var_dump($message);
    // 向所有人发送
    $host = "127.0.0.1";
    $port = 8282;
    global $socket;
    if (empty($socket)) {
        $socket = TcpClient::getInstace()->connect($host, $port);
    Gateway::sendToClient($client_id, "$client_id welcome\n\r");
    }
}
```

##### 注意：WebServer服务器在次处只是充当一个中转站，其他不起任何作用。包括`onMessage`方法都是使用的`TcpServer`的Events.php

##### 测试
1.  启动 `telnet 127.0.0.1 8282`,`telnet 127.0.0.1 8283`

2.  打开浏览器，F12，console处输入下方代码
    ```javascirpt
    ws = new WebSocket("ws://127.0.0.1:8283");
    ws.onopen = function() {
        alert("连接成功");
        ws.send('tom');
        alert("给服务端发送一个字符串：tom");
    };
    ws.onmessage = function(e) {
        alert("收到服务端的消息：" + e.data);
    };
    ƒ (e) {
        alert("收到服务端的消息：" + e.data);
    }
    ```
此时，Tcp服务器接收到数据 tom：
![](https://i.loli.net/2019/07/22/5d35a6c83e8cf10757.png)
在Tcp发送数据 1，浏览器显示：
![](https://i.loli.net/2019/07/22/5d35a6f7ee51014238.png)

<hr>

至此，服务器和硬件的双向通行已完成。至于后续的微信小程序控制硬件，则就是微信小程序直接调用WebSocket即可。

参考文章：[基于Web实现远程与硬件交互](https://blog.csdn.net/kangle0228/article/details/83109820)