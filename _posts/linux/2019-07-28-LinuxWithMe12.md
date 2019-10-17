---
layout: post
title: "跟我学Linux——第十二日"
subtitle: "第10章 使用 Apache 服务部署静态网站"
author: "Twany"
header-img: "img/bg-art/post-bg-ele.jpg"
header-mask: 0.4
catalog: true
tags:
  - Linux
---  

## 网站服务程序
#### 一个简单的实例
1.  挂载镜像，使用Yum下载 httpd `yum install httpd`
2.  启用`httpd`服务程序并添加到开机项   `systemctl start httpd` `systemctl enable httpd`
3.  测试：打开浏览器，输入`127.0.0.1`即可看到（文件路径：`/var/www`）

#### 配置服务参数
![](https://i.loli.net/2019/08/03/NVzn54KaMyJYo8D.png)

详细配置👉 [配置服务文件参数](https://www.linuxprobe.com/chapter-10.html#102)

## SELinux 安全子系统
> SELinux 安全子系统能够从多方面监控违法行为：对服务程序的功能进行限制（SELinux域限制可以确保服务程序做不了出格的事情）；对文件资源的访问限制（SELinux安全上下文确保文件资源只能被其所属的服务程序进行访问）。

由概述可知，“SELinux 域”和“SELinux 安全上下文”是Linux系统的双保险。

当我们将网站默认目录修改到其他目录下，那么就会出现无法访问的情况。我们可以通过修改当前目录的 `SELinux 安全上下文` 和原始目录一样即可

详见👉 [SELinux安全子系统](https://www.linuxprobe.com/chapter-10.html#103SELinux)

## 虚拟主机功能
虚拟主机可以理解为一台服务器划分为多个虚拟主机，每个是独立的，可存放各自的文件。

Apache的虚拟主机功能是服务器基于用户请求的不同**IP地址、主机域名或端口号**，实现提供多个网站同时为外部提供访问服务的技术。

![](https://www.linuxprobe.com/wp-content/uploads/2015/05/Apache%E8%99%9A%E6%8B%9F%E4%B8%BB%E6%9C%BA%E5%8A%9F%E8%83%BD%E6%8B%93%E6%89%91.png)
#### 基于主机域名
我们以基于主机域名为例（实际情况用到较多），来演示在虚拟机上如何创建三个不同的网站

1. 分别在`/home/wwwroot`中创建三个文件夹，来保存对应网站数据，并分别输入数据来观看结果
   ```shell
    [root@linuxprobe ~]# mkdir -p /home/wwwroot/www
    [root@linuxprobe ~]# mkdir -p /home/wwwroot/bbs
    [root@linuxprobe ~]# mkdir -p /home/wwwroot/tech    //创建三个文件
    [root@linuxprobe ~]# echo "WWW.linuxprobe.com" > /home/wwwroot/www/index.html
    [root@linuxprobe ~]# echo "BBS.linuxprobe.com" > /home/wwwroot/bbs/index.html
    [root@linuxprobe ~]# echo "TECH.linuxprobe.com" > /home/wwwroot/tech/index.html //输入数据至首页
   ```
2. 配置`httpd服务`，追加三个基于主机名的虚拟主机参数
   ```shell
    [root@linuxprobe ~]# vim /etc/httpd/conf/httpd.conf
    ………………省略部分输出信息………………
    113 <VirtualHost 192.168.10.10>
    114 DocumentRoot "/home/wwwroot/www"    //网站资源路径
    115 ServerName "www.linuxprobe.com"     //网站地址
    116 <Directory "/home/wwwroot/www">     //网站资源路径
    117 AllowOverride None
    118 Require all granted
    119 </directory> 
    120 </VirtualHost>

    121 <VirtualHost 192.168.10.10>
    122 DocumentRoot "/home/wwwroot/bbs"
    123 ServerName "bbs.linuxprobe.com"
    124 <Directory "/home/wwwroot/bbs">
    125 AllowOverride None
    126 Require all granted
    127 </Directory>
    128 </VirtualHost>

    129 <VirtualHost 192.168.10.10>
    130 DocumentRoot "/home/wwwroot/tech"
    131 ServerName "tech.linuxprobe.com"
    132 <Directory "/home/wwwroot/tech">
    133 AllowOverride None
    134 Require all granted
    135 </directory>
    136 </VirtualHost>
    ………………省略部分输出信息………………
   ```
3. 修改好配置文件，保存并退出，**重启 `httpd` 服务**
3. 因为我们将网站资源路径从默认地址转移，所以应该修改正确的`SELinux安全上下文`
   ```shell
    [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot
    [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/www
    [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/www/*
    [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/bbs
    [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/bbs/*
    [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/tech
    [root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/tech/*
   ```
4. 使新配置的 `SELinux安全上下文`立即生效 `restorecon -Rv /home/wwwroot`
5. 测试，成功!

其他两种详见👉 [配置虚拟主机](https://www.linuxprobe.com/chapter-10.html#105)

## Apache 的访问控制
```shell
[root@linuxprobe ~]# vim /etc/httpd/conf/httpd.conf
………………省略部分输出信息………………
129 <Directory "/var/www/html/server">
130 SetEnvIf User-Agent "Firefox" ff=1
131 Order allow,deny
132 Allow from env=ff
133 </Directory>
………………省略部分输出信息………………
[root@linuxprobe ~]# systemctl restart httpd
```
进行测试，我们会发现 Firefox 浏览器无法访问 127.0.0.1，其他浏览器均可以。

## 图片笔记
![](https://i.loli.net/2019/08/03/BxN4iwog1K8fydW.jpg)
![](https://i.loli.net/2019/08/03/O1PN4C8jlQLcpHq.jpg)
