---
layout: post
title: "跟我学Linux——第十一日"
subtitle: "第9章 使用 ssh 服务管理远程主机 "
author: "Twany"
header-img: "img/bg-art/post-bg-two.jpg"
header-mask: 0.4
catalog: true
tags:
  - Linux
---  

## 配置网络服务
#### 配置网络参数
配置网络服务有4种方式：
- 编辑服务 `vim /etc/sysconfig/network-scripts/ifcfg-enoxxxxxxxx` （其中xxxxx可通过`ifconfig`命令得到）
- 图形化设置：`nmtui`
- 图形化设置：`nm-connection-editro`
- RHEL 7 右上角设置

详见：[Linux-配置网络的四种方法](https://blog.csdn.net/xin1889/article/details/80025564) 、[配置网卡参数](https://www.linuxprobe.com/chapter-09.html#911)

#### 创建网络会话
> RHEL和CentOS系统默认使用NetworkManager来提供网络服务，这是一种动态管理网络配置的守护进程，能够让网络设备保持连接状态。可以使用nmcli命令来管理Network Manager服务。nmcli是一款基于命令行的网络配置工具，功能丰富，参数众多。它可以轻松地查看网络信息或网络状态：

RHEL7系统支持网络会话功能，允许用户在多个配置文件中快速切换（非常类似于firewalld防火墙服务中的区域技术）。

格式：使用`nmcli connection add con-name type iframe`

成功创建后，可通过 `nmcli connection show` 查看

## 远程控制服务
#### 配置 sshd 服务
> SSH（Secure Shell）是一种能够以安全的方式提供远程登录的协议，也是目前远程管理Linux系统的首选方式。

`sshd` 提供了两种验证方式
- 基于口令的验证—用账户和密码来验证登录；
- 基于密钥的验证—需要在本地生成密钥对，然后把密钥对中的公钥上传至服务器，并与服务器中的公钥进行比较；该方式相较来说更安全。

此处我们只讲如何配置使得`root`用户不能使用sshd服务，保障系统安全性
```shell
vim /etc/ssh/sshd_config
--------
...
PermitRootLogin no //原来是yes，允许 
...
--------
```

#### sshd安全密钥验证
1. 在客户端主机生成“密钥树”     `ssh-keygen`
2. 把客户端主机生成的公钥文件发到远程主机(服务器)   `ssh-copy-id 192.168.10.10`
3. 对服务器进行设置，使其只允许密钥登录 `vim /etc/ssh/sshd_config` => `PasswordAuthentication no` => `systemctl restart sshd`
4. 测试 `ssh 192.168.10.10` 成功连接

#### 远程传输命令
`scp`命令

#### 不间断会话服务
连接主机，有一个问题：当与远程主机的会话被关闭，在远程主机运行的命令也将被关闭

`screen` 就是为了解决网络异常中断或为了同时控制多个远程终端窗口而设计的程序。有以下功能
- **会话恢复**：即便网络中断，也可让会话随时恢复，确保用户不会失去对远程会话的控制。
- **多窗口**：每个会话都是独立运行的，拥有各自独立的输入输出终端窗口，终端窗口内显示过的信息也将被分开隔离保存，以便下次使用时依然能看到之前的操作记录。
- **会话共享**：当多个用户同时登录到远程服务器时，便可以使用会话共享功能让用户之间的输入输出信息共享。

##### `screen` 的安装
安装需要用到`yum`，首先我们需要配置Yum源
1. ![](https://www.linuxprobe.com/wp-content/uploads/2015/05/%E7%A1%AE%E8%AE%A4%E5%85%89%E7%9B%98%E5%B7%B2%E8%A2%AB%E9%80%89%E4%B8%AD.png)
2. 只需要在设备状态中的**已连接**打上勾即可，[镜像下载地址](https://www.linuxprobe.com/tools)
3. 将系统镜像挂载到`/media/cdrom`目录
   ```shell
    $ mkdir -p /media/cdrom     //创建挂载目录
    $ mount /dev/cdrom /media/cdrom 进行挂载
   ```
4. 配置Yum仓库配置文件
    ```shell
    [root@linuxprobe ~]# vim /etc/yum.repos.d/rhel7.repo
    ----vim模式
    [rhel7]
    name=rhel7
    baseurl=file:///media/cdrom
    enabled=1
    gpgcheck=0
    -----vim模式
    ```
5. 下载 `yum install screen`

#### 管理远程会话
`screen + 参数`
- -S    创建会话窗口
- -r    恢复指定会话（后加对应序号或名称）
- -x    一次性恢复所有会话
- -ls   显示所有会话
- -wipe 删除会话
- exit  退出会话

并不是要先创建会话，再开始工作。也可以`screen vim a.txt`，命令执行结束会话结束

#### 会话共享
多人控制一台主机，共享屏幕内容

步骤：
1. 使用sshd将终端 **A** 远程连接到服务器  `ssh 192.168.10.10`
2. 创建会话 `screen -S linuxprobe`
3. 使用sshd将终端 **B** 远程连接到服务器  `ssh 192.168.10.10`
4. 获取会话 `screen -x`
5. 至此，两台终端已实现会话共享

## 图片笔记
![](https://i.loli.net/2019/08/03/GnlNe4t6XuwBbWU.jpg)
![](https://i.loli.net/2019/08/03/q7ojK9Q2haWwBJS.jpg)
![](https://i.loli.net/2019/08/03/YqiCd1aneHv38fO.jpg)
![](https://i.loli.net/2019/08/03/BxN4iwog1K8fydW.jpg)
