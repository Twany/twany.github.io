---
layout: post
title: "跟我学Linux——第八、九日"
subtitle: "第7章 使用RAID与LVM磁盘阵列技术"
author: "Twany"
header-img: "img/bg-scene/post-bg-three.jpg"
header-mask: 0.4
catalog: true
tags:
  - Linux
---

## RAID（独立冗余磁盘排列）
> RAID技术通过把多个硬盘设备组合成一个容量更大、安全性更好的磁盘阵列，并把数据切割成多个区段后分别存放在各个不同的物理硬盘设备上，然后利用分散读写技术来提升磁盘阵列整体的性能，同时把多个重要数据的副本同步到不同的物理硬盘设备上，从而起到了非常好的数据冗余备份效果。

#### RAID 0
![](http://ww1.sinaimg.cn/large/006Sc6e5ly1g57x9rxdwvj306f06kaba.jpg)
**RAID 0能有效提升硬盘数据的吞吐速度，但不具备数据备份和恢复功能。**

#### RAID 1
![](http://ww1.sinaimg.cn/large/006Sc6e5ly1g57xbb5xdsj3072081q4q.jpg)
**将两块硬盘绑定，可理解为右侧为左侧的备份，可用率降低，增大了系统计算的负载。**

#### RAID 5
![](http://ww1.sinaimg.cn/large/006Sc6e5ly1g57xd6n16kj30gr0brabm.jpg)
**硬盘中`parity`部分为数据奇偶校验信息（压缩过的信息），“妥协地”兼顾硬盘设备的读写速度、数据安全性和存储成本问题。**

#### RAID 10
![](http://ww1.sinaimg.cn/large/006Sc6e5ly1g57xfm46aoj30k00b1agt.jpg)
**RAID 10 为 RAID 0 和 RAID 1 的组合体。**

#### 部署磁盘阵列
- `mdadm` 命令管理Linux2系统中的软件RAID硬盘阵列
  - `mdadm -Cv /dev/md0 -a yes -n 4 -l 10 /dev/sdb /dev/sdc /dev/sdd /dev/sde`
  - 上述命令创建RAID 10 ，名为 `/dev/md0`,设备数量为**4**，RAID 级别为 10，四块硬盘分别为/dev/sdbcde
![](https://i.loli.net/2019/07/23/5d37238f0a9ec46739.png)

- 磁盘阵列组RAID部署好之后，将其格式化为 `ext4`格式
  - `mkfs.ext4 /dev/md0`

- 再次，创建挂载点然后把硬盘设备进行挂载操作
  - ```
    [root@linuxprobe ~]# mkdir /RAID
    [root@linuxprobe ~]# mount /dev/md0 /RAID   //挂载在新建目录/RAID下
    ```

- 把挂载信息写入到配置文件中，使其永久生效

#### 磁盘修复
详见 👉 [第七章](https://www.linuxprobe.com/chapter-07.html)

## LVM逻辑卷管理器
##### 何为LVM？
**LVM是一种非常普及的硬盘资源管理技术，允许用户对硬盘资源进行动态调整**

![](https://i.loli.net/2019/07/23/5d3725aa05cd111142.png)
![](https://i.loli.net/2019/07/23/5d3725846df2d66486.png)

详细可见 上述链接

> 😘 Mrs.Lgg

## 图片笔记
##### 7.21
![](http://ww1.sinaimg.cn/large/006Sc6e5ly1g57xonhx1oj30qo0e0abj.jpg)
![](http://ww1.sinaimg.cn/large/006Sc6e5ly1g57xoni4ikj30qo0e0wfz.jpg)
![](http://ww1.sinaimg.cn/large/006Sc6e5ly1g57xoning1j30qo0e0abd.jpg)
![](http://ww1.sinaimg.cn/large/006Sc6e5ly1g57xonkpxlj30t71hc44u.jpg)

##### 7.22
![](https://i.loli.net/2019/07/23/5d37261d572c719282.jpg)
![](https://i.loli.net/2019/07/23/5d37261e1376e20500.jpg)
![](https://i.loli.net/2019/07/23/5d37261ec394a48657.jpg)
![](https://i.loli.net/2019/07/23/5d37261f95fc468935.jpg)
