---
title: virtualBox上的那些事(二)
date: 2018-08-17 17:17:47
tags:
  - linux
---

> 接下来，我准备尝试一下主机访问虚拟机，当然也为集群做准备。

首先查看虚拟机ip地址，得到的值是：10.0.2.15

然后在主机上ping一下，发现不通。很开心！<span style="color: #999">（幸福就是猫吃鱼，狗吃肉，奥特曼打小怪兽）</span>

想要主机访问虚拟机，我们可以通过host-only模式。添加一块新网卡，连接方式设置为host-only。

进入虚拟机，用ifconfig命令可以查看到，多了一个enp0s8的网卡，复制一份enp0s3的配置文件，进行修改。
```
BOOTPROTO=dhcp
改成：
BOOTPROTO=static

下面三行是新增的
IPADRR=192.168.56.102
GATEWAY=192.168.56.1
NETMASK="255.255.255.0"
```

接着来，测试一下能访问web服务不？

新建一个web项目，用parcel搭个服务器，用主机访问192.168.56.102发现无法访问，原因是虚拟机的防火墙没有开放端口，把对应的web服务端口打开，即可。

在第二台虚拟机上访问，也同样没问题。 哦了！

接下来，更好玩。