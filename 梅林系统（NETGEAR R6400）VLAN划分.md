### 梅林系统（NETGEAR R6400）VLAN划分

使用梅林路由器作为交换机，单臂路由做软路由主路由：

1.梅林开启AP模式，过程中会要求设置无线名称以及密码；

2.梅林划分VLAN 11 和 VLAN 12：（过程中尝试直接修改VLAN1 VLAN2 总是无法成功）

​	0口用于接入上级路由或光猫；1口用于接单臂路由，2，3，4用于接入终端设备

​	VLAN11 端口：0u 	1t

​	VLAN12端口：1t 	2u	3u	4u

命令：

```shell
robocfg vlans reset vlan 1 ports "0 1 2 3 4 5t" vlan 11 ports "0u 1t" vlan 12 ports "1t 2u 3u 4u"
```

梅林重启后VLAN又会失效，在**/jffs/scripts** 目录添加开机自启动 **init-start** 脚本：

```shell
#!/bin/sh

/usr/sbin/robocfg vlans reset vlan 1 ports "0 1 2 3 4 5t" vlan 11 ports "0u 1t" vlan 12 ports "1t 2u 3u 4u"
```

3.单臂路由开启**NUC模式**: **在web界面->系统->进阶设置->NUC模式**，选择NUC模式后单臂路由会自动重启；

4.单臂路由LAN口：IPv4 设置为**192.168.30.1**,协议选择**静态地址**；其他默认，之后保存；

​				   WAN口：协议设置为**DHCP 客户端**，此处根据实际情况选择，我的上级是一个路由器，故选择DHCP，如果上级是光猫拨号，可以选择PPOE，我没试过，具体有待实验。

5.到此处单臂软路由设置完成，此时手机连接无线就能自动获取到**192.168.30.0/24**网段的局域网IP并能正常上网。