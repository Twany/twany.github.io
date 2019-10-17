---
layout: post
title: "跟我学Linux——第十日"
subtitle: "第8章 Iptables与Firewalld防火墙"
author: "Twany"
header-img: "img/bg-art/post-bg-one.jpg"
header-mask: 0.4
catalog: true
tags:
  - Linux
---

## iptables
#### 策略与规则链
防火墙会**从上而下**读取配置，在找到匹配项后**结束匹配**。

iptables 服务把用于处理货过滤流量的策略条目称为规则，多条规则可以组成一个规则链。

当有了规则之后，不一定能通过iptables，因为还有“动作”限制：
- `ACCEPT`  允许
- `REJECT`  拒绝但返回提示
- `LOG`     记录日志
- `DROP`    拒绝且无响应

#### iptables 中基本的命令参数和使用
详见👉 [基本的命令参数](https://www.linuxprobe.com/chapter-08.html#822)

## firewalld
 RHEL 7系统中集成了多款防火墙管理工具，其中firewalld（Dynamic Firewall Manager of Linux systems，Linux系统的动态防火墙管理器）服务是默认的防火墙配置管理工具，它拥有基于CLI（命令行界面）和基于GUI（图形用户界面）的两种管理方式。

`firewalld`提供了**区域**的概念。
> 区域就是firewalld预先准备了几套防火墙策略集合（策略模板），用户可以根据生产场景的不同而选择合适的策略集合，从而实现防火墙策略之间的快速切换。

#### 终端管理工具
命令格式：`firewall-cmd + 参数` 支持Tab补齐
![](https://i.loli.net/2019/08/03/Ksfakc21ZoXmpSl.png)

与其他防火墙策略工具一样，`firewalld`策略默认为`Runntime`模式（当前模式），另外一种就是`Permanent`模式（永久模式）

开始永久模式的方法：设置防火墙策略是添加 `--premanent` 参数。之后还需要手动执行 `firewall-cmd --reload` 使其生效

详见👉 [`firewall`使用](https://www.linuxprobe.com/chapter-08.html#831)

firewalld中的富规则表示更细致、更详细的防火墙策略配置，它可以针对系统服务、端口号、源地址和目标地址等诸多信息进行更有针对性的策略配置。它的优先级在所有的防火墙策略中也是最高的。比如，我们可以在firewalld服务中配置一条富规则，使其拒绝192.168.10.0/24网段的所有用户访问本机的ssh服务（22端口）：

```shell
[root@linuxprobe ~]# firewall-cmd --permanent --zone=public --add-rich-rule="rule family="ipv4" source address="192.168.10.0/24" service name="ssh" reject"
success
[root@linuxprobe ~]# firewall-cmd --reload
success
```

#### 图形管理工具
命令：`firewall-config` 进行图形化管理工具
详见👉 [firewall图形化管理工具使用](https://www.linuxprobe.com/chapter-08.html#832)

## 服务的访问控制列表
> TCP Wrappers是RHEL 7系统中默认启用的一款流量监控程序，它能够根据来访主机的地址与本机的目标服务程序作出允许或拒绝的操作。

可以理解为 **Linux有两个层面的防火墙，第一种是外层的，对TCp/Ip协议的流量进行过滤，`Tcp Wrapper`服务是对Linux防火墙进行控制**

`Tcp Wrapper` 由两个文件构成：
1. 系统首先检查 **控制列表文件**（天使文件）`/etc/hosts.alow`，遇到匹配项就去放行该流量。
2. 没有的话去检查 **拒绝控制列表文件**（恶魔文件）`/etc/hosts.deny`，遇到匹配项就拒绝掉该流量。
3. 如果两种都没有，系统默认放行流量

**注意**：
- 编写拒绝策略规则时，填写的是**服务名称**，而非协议名称
- 不需重启即可生效

## 图片笔记
![](https://i.loli.net/2019/08/03/2XrB3TRQIMtEoys.jpg)
![](https://i.loli.net/2019/08/03/ewkxP7g8Rp4vUNA.jpg)
![](https://i.loli.net/2019/08/03/WL2wpFnEb18TaSx.jpg)
![](https://i.loli.net/2019/08/03/3oZUzcqxADYBgfv.jpg)

