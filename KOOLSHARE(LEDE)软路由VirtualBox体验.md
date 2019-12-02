## KOOLSHARE(LEDE)软路由VirtualBox体验

1.下载虚拟硬盘**VMDK**文件，[combined-squashfs.vmdk](https://firmware.koolshare.cn/LEDE_X64_fw867/)

2.安装VitualBox

3.新建虚拟机，选择**Linux**类型，版本选择**other Linux(64-bit)**；

4.点击下一步，设置内存，使用默认的内存即可，点击下一步；

5.选择**已有的虚拟硬盘文件**，选中刚刚下载的**vmdk**后缀的文件，VirtualBox6以上版本要先添加该硬盘文件，之后在已添加的虚拟硬盘文件列表中选中刚刚添加的**vmdk**文件即可，点击创建即可；

6.选中刚刚创建的虚拟机，设置->网络：

* 选择**Adapter 1**，勾选**Enable Network Adapter**，选择**NAT**网络，展开**Advanced**，点击**Port Forwarding**，弹出页面中添加一条规则：

   | Name   | Protocol | Host IP   | Host Port | Guest IP  | Guest Port |
   | ------ | -------- | --------- | --------- | --------- | ---------- |
   | Rule 1 | TCP      | 127.0.0.1 | 18888     | 10.0.2.15 | 80         |
   
   点击**OK**
   
* 选择**Adapter 2**，勾选**Enable Network Adapter**，选择**Internal Network**，点击**OK**

7.启动虚拟机，显示画面不再滚动后，按任意键进入terminal，

8.配置更好启用的两个网卡，**网卡1**用于连接外网，**网卡2**用于软路由的网关：

​	编辑**/etc/config/network**文件，到文件末尾找到config lan 和 config wan 的模块

* ```ini
  config	interface 'wan'
  				option	ifname 'eth0'
  				option	proto	'dhcp'
  				
  config	interface 'wan6'
  				option	ifname 'eth0'
  				option	proto 'dhcpv6'
  				
  config	interface 'lan'
          option	type 'bridge'
          option	ifname 'eth1'
          option	proto	'static'
          option	'192.168.20.1'
          option	netmask '255.255.255.0'
          option	ip6assign	'60'
  ```

  文件中已经有对应的模块了，修改为不与其他网段冲突网段即可，上图为192.168.20.0/24，wan口用于连接外网，DHCP自动获取IP，VirtualBox的NAT默认分配的地址为10.0.2.15，所以上面的端口转发设置为这个地址；lan口就是软路由的网段了，对应之前步骤的**Adapter 2**。

  编辑保存后，执行**/etc/init.d/network restart** 重启网络。

9.关闭LEDE的防火墙以允许通过网页访问管理页面：**/etc/init.d/firewall stop**

10.浏览器打开[http://localhost:18888](http://localhost:18888)即可访问LEDE的管理页面，密码默认为**koolshare**



#### 单网口软路由设置：

1.下载koolshare镜像，使用dd命令刻录到U盘（其它刻录方式也可以）；

2.安装系统成功后，根据上面虚拟机教程，修改eth0的IP为主路由的可分配IP；

3.设置Lan和Wan均为eth0，同时Lan设置为静态IP，IP为当前主路由下可使用的一个IP；

4.重启网络，关闭单臂路由防火墙，单臂路由网线接到主路由的lan口，当前电脑接到主路由网口，浏览器打开刚刚单臂路由的IP，进入网络接口页面；

5.设置Wan口：基本设置使用DHCP，物理设置去掉桥接接口，点击保存；

6.设置Lan口：基本设置，使用静态IP，设置IPv4地址（其实刚刚命令行已经设置过了，此处再确认一下），物理设置去掉网桥接口的勾选；点击保存；

7.软件中心安装透明网桥，安装完成后，打开透明网桥，网关IP设置为主路由的IP，网桥IP设置为刚刚给单臂路由设置的IP，子网掩码根据主路由设置，之后启用网桥；

8.现在在主路由局域网下面有两个网关IP可以使用：主路由IP和网桥IP，如果要使DHCP的分配的默认网关为网桥IP，在主路由的设置里面可以把默认网关改为网桥IP，这样的话局域网下面的设备默认网关就是单臂软路由的IP了，可以在软路由里面对流量进行控制了。