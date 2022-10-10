+++
title = "关于搞 Hadoop 结果解锁我另一些技能的故事"
slug = "Unknown Hadoop Notes"
description = "所以 Hadoop 是个啥东西"
date = "2022-10-10"
image = "https://legacy.superbart.xyz/picture/Random/Cirno-Easter%20Egg.png"
categories = [
    "Technology"
]
tags = [
	"Linux",
    "虚拟机",
    "ssh"
]
+++

如何在虚拟机之间搞互联互通？—— 以 VirtualBox 为示例  
仅仅是个笔记，不传到老博客了。

### 概述
1. 管理 -> 主机网络管理器新建一个新网卡 vboxnet0
2. 在每个虚拟机的设置中，设置网络为仅主机网络，界面名称 vboxnet0
3. 每个虚拟机里面要设置好固定 ip ，关闭防火墙，和 ssh 免密码登录
4. (可选但推荐) 修改所有虚拟机的 host 文件，标记所有虚拟机的地址

### 截图~主机网络管理器

![](https://legacy.superbart.xyz/picture/Random/VirtualBox-1.png)

如果设置成功的话，你的宿主机应该可以 ping 到你的虚拟机。查看你电脑的 ip，可以执行 `ifconfig` 或者 `ip a`。

### 如何关闭网络防火墙

基本上我见到的 Linux 系统，防火墙软件都是 [firewalld](https://firewalld.org/)。关闭防火墙，也就是关掉这个服务。所以我们要执行

```bash
sudo systemctl stop firewalld.service # 停止防火墙服务
sudo systemctl disable firewalld.service # 防止防火墙开机自启动
```


说到这了，看看 Systemd 还能搞啥
```bash
sudo systemctl status firewalld.service # 看看这玩意运行状态
```

另外，如果你安装的是 Ubuntu Server ，安装时候可以关掉防火墙选项的。如果你安装的是 OpenSUSE，你也可以在 YaST 里面关掉，或者开 22 和 23 端口。

### 如何设置免密码登录 ssh
```bash
$ ssh-keygen
$ cd ~/.ssh
$ ssh-copy-id 另外一个虚拟机的用户名@另外一个虚拟机的ip
$ ssh 另外一个虚拟机的用户名@另外一个虚拟机的ip # 测试是否成功
```

注意，.ssh目录的权限为700，其下文件authorized_keys和私钥的权限为600。如有问题，请使用 chmod 修改。

## 修改 Host 文件
```
sudo nano /etc/hosts
```

修改方式是 ip + 电脑名称
