---
layout: post
title: "Swoole初尝鲜"
subtitle: "第一次与Swoole的接触"
author: "Twany"
header-img: "img/bg-person/post-bg-rwd.jpg"
header-mask: 0.4
catalog: true
tags:
  - Swoole
---

其实，很久之前就听说Swoole的大名了。自己也曾试着去学习了一下，但当时的自己是真的菜，一听说是要在linux上才能玩得转，也就退却了。

直到环院的项目开始动工，自己也就“被迫”开始学习了

最开始想的是很简单，觉得无非就是一个简单的接受发数据嘛，可当操作起来，完全懵逼。现在也没有学过计算机网络，也就不知道网络协议的区别，第一次用到Tcp真的以一头雾水，哎。。。

生产环境：
- 硬件：**esp8266**
- 软件：
  - 系统环境：CentOS 7
  - 运行环境：PHP 7
  - Swoole：swoole 4.4

## 起源
最开始自己使用的是`WorkMan`，结果连接不成功，现在看来不是`WorkMan`使用不当，而是服务器的问题。

## 解决方法
#### step1 swoole安装
官网下载swoole，并编译到服务器上。

此处有几个坑
- swoole不是安装到项目目录，而是server目录
- 注意PHP版本，4.4就已经要求是php7.2版本了
- 宝塔界面有swoole的扩展，应该可以直接安装
- 下载新的PHP版本，要在网站界面去切换PHP版本
  
具体可参考：[swoole安装详细步骤](https://blog.csdn.net/z13615480737/article/details/83857055)

#### step2 代码编写
网上找了几个demo，因为自己只需要实现数据的接发，所以很简单
```php
<?php 

// 创建serv 并且全网广播 监听2350端口
$serv = new swoole_server("0.0.0.0 ", 2350);

// 进入监听事件
$serv->on('connect',function($serv,$fd){

echo "进入监听 :进入用户".$fd."\n";

});

// 监听数据接受事件
$serv->on('receive',function($serv,$fd,$from_id,$data){
$serv->send($fd,"用户：".$fd."---".$data);
});

//离开监听
$serv->on('close',function($serv,$fd){

echo "用户离开用户:".$fd."\n";

});

//启动
$serv->start();
?>
```
上面出现的ip注意哟填写成`0.0.0.0`，端口号随意，我这里填写的是2350

#### step3 运行
打开Linux命令行，**进入到项目目录**，输入`php data.php`，运行服务

结果显示 **connections refuesd**，为什么呢？

忘了安全组配置！！！

#### step4 安全组配置
因为我是阿里云的服务器，所以登录阿里云控制台

添加安全组规则（出入方向都要添加）
![](https://i.loli.net/2019/07/16/5d2ddca04275530790.png)
![](https://i.loli.net/2019/07/16/5d2ddcacada9f23101.png)

本以为一切都已经over，可是再次测试———仍然报同样的错误！

百度了一大堆，都在说是防火墙的问题，可是自己明明已经设置安全组规则啊！

#### step5 找bug
找到一篇博客，博主不是通过在控制台设置安全组来设置防火墙，而是直接在Linux设置

自己也就照样敲了一遍命令，结果，不可思议的一幕出现了

- `firewall-cmd --list-ports` 查看开放端口
![](https://i.loli.net/2019/07/16/5d2dddda7e51420725.png)
我们可以看到，并没有我们之前设置的2235端口，奇怪了

- `firewall-cmd --zone=public --add-port=2235/tcp --permanent`  添加2350开放端口

- `systemctl restart firewalld.service` 重启服务生效

这样，当我们再次查看端口时，2350成功出现
![](https://i.loli.net/2019/07/16/5d2dde6b1e87795719.png)

#### step6 成功
其实这里有个小插曲，完成第五步之后并没有成功，于是重启服务器，再次进入到项目目录，`php date.php`运行服务文件

再次访问，成功！
![](https://i.loli.net/2019/07/16/5d2ddf97d2ba897429.png)
![](https://i.loli.net/2019/07/16/5d2ddfaba15d343551.jpg)

<hr>

## 总结
其实，这一切的问题都是自己基础知识的缺失，完全就是没头的苍蝇乱飞。

另外，设置Linux服务器时，最好自己亲自用命令去设置一下，否则出现上述的bug就真的恶心死了，从下午一点到晚上八点，菜是原罪啊！

不得不说，Swoole是真的牛逼！简直碉堡了有没有

最有效的方法，**重启试试**！

[致谢](https://www.qinziheng.com/swoole/7472.htm)


