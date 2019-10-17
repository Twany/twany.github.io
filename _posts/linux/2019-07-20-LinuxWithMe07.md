---
layout: post
title: "跟我学Linux——第七日"
subtitle: "第6章 存储结构与磁盘划分"
author: "Twany"
header-img: "img/bg-scene/post-bg-nine.jpg"
header-mask: 0.4
catalog: true
tags:
  - Linux
---

## 一切从“/”开始
Linux系统的文件存储系统极具特色，采用**文件系统层次化标准（FHS）**，如下图：
![](https://i.loli.net/2019/07/20/5d332eae295ba81301.png)

其中，各目录标准存放文件类型如下：
![](https://i.loli.net/2019/07/20/5d332ee12428e56424.png)

## 物理设备的命名规则
分区关系如图：
![](https://i.loli.net/2019/07/20/5d332f6626c0d20618.png)
![](https://i.loli.net/2019/07/20/5d332f7aa135e17710.png)

## 文件系统与数据资料
RHEL 7 默认的文件系统为XFS，作用是发生宕机后快速恢复可能被破坏的文件，存储容量大，是一种高性能的日志文件系统。

## 挂载硬件设备
##### 挂载意为将设备和已存在的目录文件进行关联，这个动作即为挂载
- `mount 文件 目录` 挂载文件系统
  - 上述操作重启失效，将挂载信息存储在`/etc/fstab`中则永久有效
  - 格式：`设备文件 挂载目录 格式类型 权限选项 是否备份 是否自检`
    - 例：`/dev/sdb2 /backup ext4 defaults 0 0`
- `umount [挂载点/设备文件]` 撤销挂载

## 添加硬盘设备
详细可知👉 [存储结构与磁盘划分](https://www.linuxprobe.com/chapter-06.html)

## 添加交换分区
**SWAP分区**是一种通过在硬盘中预先划分出一定空间，然后把内存中暂时不用的数据临时存储在硬盘中。

交换分区创建过程与前面的挂载过程类似，不同之处如下：
- 专用格式化命令`mkswap`

## 磁盘容量配额
> 引子：为了防止Linux系统中某个用户大量占用空间，可对磁盘容量进行**配额管理**

**RHEL 7 已经安装quote磁盘容量配额服务程序包，但默认不开启，可在配置文件中修改：
```shell
[root@linuxprobe ~]# vim /etc/fstab //修改配置文件
#
# /etc/fstab
# Created by anaconda on Wed May 4 19:26:23 2017
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/rhel-root / xfs defaults 1 1
UUID=812b1f7c-8b5b-43da-8c06-b9999e0fe48b /boot xfs defaults,**uquota** 1 2
/dev/mapper/rhel-swap swap swap defaults 0 0
/dev/cdrom /media/cdrom iso9660 defaults 0 0 
/dev/sdb1 /newFS xfs defaults 0 0 
/dev/sdb2 swap swap defaults 0 0 
[root@linuxprobe ~]# reboot //重启查看
[root@linuxprobe ~]# mount | grep boot
/dev/sda1 on /boot type xfs (rw,relatime,seclabel,attr2,inode64,usrquota)
```

#### xfs_quote命令
**对用户进行配额管理**
![](http://ww1.sinaimg.cn/large/006Sc6e5ly1g57x1sqvm1j30eu03qmx5.jpg)

#### edquota
**编辑用户的quote配置**
```shell
[root@linuxprobe ~]# edquota -u tom  //编辑tom，默认vim
Disk quotas for user tom (uid 1001):
 Filesystem blocks soft hard inodes soft hard
 /dev/sda1 6144 3072 8192 1 3 6
```

## 软硬方式链接
链接类似于Windows的“快捷方式”，在Linux分为“软链接”和“硬链接”，硬链接其实是**copy一个原文件**，软链接才是真正的**“快捷方式”**

#### ln命令
`ln [选项] 目标`  不加`-s`默认硬链接，加了为软链接
<hr>

## 笔记照片
![](https://i.loli.net/2019/07/20/5d33324498db776360.jpg)
![](https://i.loli.net/2019/07/20/5d3332455ad5980089.jpg)
![](https://i.loli.net/2019/07/20/5d3332461a62d59148.jpg)
![](https://i.loli.net/2019/07/20/5d333246d12c346683.jpg)
