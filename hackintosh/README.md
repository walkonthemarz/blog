### Hackintosh

###### 1. Realtek Alc audio card not working: https://github.com/acidanthera/AppleALC/wiki/Installation-and-usage

##### 2.For audio solution, optional link: https://blog.daliansky.net/Use-AppleALC-sound-card-to-drive-the-correct-posture-of-AppleHDA.html

### Steps：

#### Catalina:

* Know the effection of kexts in your EFI folders. See [https://hackintosher.com/downloads/kexts/](https://hackintosher.com/downloads/kexts/) or googe **clover kexts descriptions**. Of course you can Baidu **黑苹果常见驱动**. See [https://imac.hk/heipingguo-kext-description-1.html](https://imac.hk/heipingguo-kext-description-1.html), [https://www.jianshu.com/p/b3e9fd096e33](https://www.jianshu.com/p/b3e9fd096e33), [https://www.jianshu.com/p/81e329c50120](https://www.jianshu.com/p/81e329c50120), 

  see appendix.

* 主要的常见几个驱动：

  * `/EFI/kexts/other`: 与other同级的其他目录一般只是用于备份或根据系统版本号存储相关驱动

    * AppleALC.kext: 解决主板声卡，我的声卡是REALTEK AL887（查看主板信息）。

    * FakePCIID.kext: 对黑苹果必备的一个驱动文件，由于macOS系统会对PCI device-id进行验证，导致黑苹果的硬件不能通过这一验证，所以需要就需要这个PCIID文件。

    * VirtualSMC.kext: 告诉系统这是Apple的硬件，允许系统安装在非苹果的硬件上，所以这是黑苹果的关键，FakeSMC.kext的新版本，两者不能同时使用，后面带有`_with_sensors`功能相同，不过多了sensor的功能。**经本人实测， 发现VirtualSMC不能支持我的主板，只有使用FakeSMC才可以，个人猜测可能是因为我的主板太老了？**

    * WhateverGreen_1.3.4.kext：修复AMD/NVIDIA显卡黑屏、花屏、睡眠黑评估等各种问题显卡驱动补丁，依赖lilu.kext

    * Lilu.kext: 一个开元的内核扩展补丁驱动文件，可以为任意kext驱动提供补丁。**被很多其他驱动依赖**，所以也是必需的。

    * XHCI-200-series-injector.kext:200系列主板， 我的主板是`华硕 B250m-plus`，相应的还有300系列、x99等。

    * CodecCommander.kext：用于破解 4K支持，合理选用 ，Whatevergreen.kext v1.2.1已经 包括这个补丁

    * VoodooPS2Controller.kext：键盘鼠标触摸板万能驱动，这个好像很好的解决了我的罗技蓝牙鼠标漂的问题。

    * IntelMausiEthernet.kext：Intel网卡驱动，跟我的主板网卡匹配，Google搜索自己网卡型号所需要驱动

    * HoRNDIS.kext：连接开启 USB tethering 模式的Android 手机接入互联网

    * CPUFriend：动态电源管理数据注入

    * `/EFI/drivers/UEFI`:或者 `/EFI/drivers64UEFI`或者`/EFI/drivers/BIOS`目录:**[这些驱动程序只在bootloader运行时有效，不会影响最终启动的操作系统。](https://blog.daliansky.net/clover-user-manual.html)**

    * | 驱动程序                | 详解                                                         |
      | ----------------------- | ------------------------------------------------------------ |
      | ApfsDriverLoader-64.efi | 苹果新推出的`apfs`文件系统，macOS 10.13/10.14必备            |
      | FSInject.efi            | 控制文件系统注入kext到系统的可能性。详细解释请参照[WithKexts](https://clover-wiki.zetam.org/Configuration#boot-args) |
      | AptioMemoryFix-64.efi   | 修复AMI Aptio EFI内存映射。如果没有就不能启动OS X            |
      | OsxFatBinaryDrv-64.efi  | 允许加载FAT模块比如boot.efi                                  |
      | CsmVideoDxe.efi         | 比UEFI里提供更多分辨率的显卡驱动(可选)                       |
      | HFSPlus.efi             | HFS+文件系统驱动程序。这个驱动对于通过启动方式 **UEFI** 来启动Mac OS X是必须的。启动方式 **BIOS** 中用到的启动程序(CloverEFI)已经包含了这个驱动，现在可以使用 **VBoxHfs-64.efi** 替代。 |

#### config.plist:

* 主板型号：**华硕 B250m-plus**,Intel 200系列主板，网上搜索主板信息，包括主板的声卡，网卡等型号
* 声卡：**集成Realtek ALC887 8声道音效芯片**，kexts/others 添加最新 AppleALC 驱动，**BOOT** 参数添加 **alcid=11**,其他声卡查询AppleALC驱动的使用说明
* 网卡：**板载Intel I219V千兆网卡**，kexts/others 添加 **IntelMausiEthernet.kext** 驱动

# 文件说明

- `Wireless.USB.Adapter.Clover-V7.zip` REALTEK无线网卡驱动，包括安装程序及通过`CLOVER`加载的驱动

- `VoodooPS2Controller.kext`和`ApplePS2SmartTouchPad.kext`两者选其一，不可全用

- `IntelGraphicsDVMTFixup.kext`用于五代以上机器，四代及以下删除，`Whatevergreen.kext`v1.2.1已经包括这个补丁

- `XHCI`开头的三个`kext`对应适用于`x99`、`200`和`300`系主板，非此类主板删除

- `AzulPatcher4600.kext`只适用于核心显卡`HD4600`，其他型号删除

- `CoreDisplayFixup.kext`用于破解`4K`支持，合理选用，`Whatevergreen.kext`v1.2.1已经包括这个补丁

- `ATH9KFixup.kext`用于驱动`AR946x`、`AR956X`、`AR9485`高通无线网卡，合理选用

- `NoTouchID`用于禁止`TouchID`的检测，合理选用

- `WhateverGreen.kext`已经更新，合并了`igfx`、`ngfx`，以及`Shiki`，启动参数和以前一样，不论`A`卡、`N`卡还是`Intel`核心显卡，都建议使用

- `ACPIBatteryManager.kext`用于实现电量显示，如遇五国卡`BAT0`之类的请删除

- `AppleALC.kext`和`VoodooHDA-2.9.1.kext`任选其一

- `RTL8100.kext`、`RTL8111.kext`、`IntelMausiEthernet.kext`、`AppleIGB`、`SmallTree-Intel-211-AT-PCIe-GBE.kext`、`ALXEthernet.kext`、`AtherosE2200Ethernet.kext`分别对应不同的网卡合理选用

  > 对应关系如下

  | RTL8100.kext                         | RTL8107E、RTL810X、RTL8139                                   |
  | ------------------------------------ | ------------------------------------------------------------ |
  | RTL8111.kext                         | Realtek RTL8111/8168 B/C/D/E/F/G/H                           |
  | IntelMausiEthernet.kext              | 82578LM、82578LC、82578DM、82578DC、82579LM、82579V、I217LM、I217V、I218LM、I218V、I218LM2、I218V2、I218LM3、I219V、I219LM、I219V2、I219LM2、I219LM2 |
  | AtherosE2200Ethernet.kext            | AR816x、AR817x、Killer E220x、Killer E2400、Killer E2500     |
  | SmallTree-Intel-211-AT-PCIe-GBE.kext | Intel I211                                                   |
  | AppleIntelE1000.kext                 | Intel系列 82540, 82541, 82542, 82543, 82544, 82545, 82546, 82547, 82578 (P55/H55)  82579 (P67/H67) 82574L 82571 82572 82573 82574 82583 I217V |
  | AppleIGB.kext                        | Intel 82575, 82576, 82580, dh89xxcc, i350, i210 and i211     |
  | ALXEthernet.kext                     | Atheros alx Ethernet                                         |
  | FakePCIID_BCM57XX_as_BCM57765.kext   | BCM57XX                                                      |
  | FakePCIID_Intel_GbX.kext             | Small Tree drivers for Intel chipset                         |

----

- `FakePCIID_BCM57XX_as_BCM57765.kext`详细支持列表
  - Broadcom NetXtreme BCM5700 Gigabit Ethernet [14e4:1644]
  - Broadcom NetXtreme BCM5701 Gigabit Ethernet PCIe [14e4:1645]
  - Broadcom NetXtreme BCM5702 Gigabit Ethernet PCIe [14e4:1646]
  - Broadcom NetXtreme BCM5703 Gigabit Ethernet PCIe [14e4:1647]
  - Broadcom NetXtreme BCM5717 Gigabit Ethernet PCIe [14e4:1655]
  - Broadcom NetXtreme BCM5717 Gigabit Ethernet PCIe [14e4:1665]
  - Broadcom NetXtreme BCM5718 Gigabit Ethernet PCIe [14e4:1656]
  - Broadcom NetXtreme BCM5719 Gigabit Ethernet PCIe [14e4:1657]
  - Broadcom NetXtreme BCM5725 Gigabit Ethernet PCIe [14e4:1643]
  - Broadcom NetXtreme BCM5727 Gigabit Ethernet PCIe [14e4:16f3]
  - Broadcom NetXtreme BCM5761 10/100/1000BASE-T Ethernet [14e4:1688]
  - Broadcom NetXtreme BCM5762 Gigabit Ethernet PCIe [14e4:1687]
  - Broadcom NetXtreme BCM57760 Gigabit Ethernet PCIe [14e4:1690]
  - Broadcom NetXtreme BCM57764 Gigabit Ethernet PCIe [14e4:1642]
  - Broadcom NetXtreme BCM57767 Gigabit Ethernet PCIe [14e4:1683]
  - Broadcom NetLink BCM57781 Gigabit Ethernet PCIe [14e4:16b1]
  - Broadcom NetXtreme BCM57782 Gigabit Ethernet PCIe [14e4:16b7]
  - Broadcom NetLink BCM57785 Gigabit Ethernet PCIe [14e4:16b5] -- Confirmed
  - Broadcom NetXtreme BCM57786 Gigabit Ethernet PCIe [14e4:16b3] -- Confirmed
  - Broadcom NetXtreme BCM57787 Gigabit Ethernet PCIe [14e4:1641]
  - Broadcom NetLink BCM57788 Gigabit Ethernet PCIe [14e4:1691]
  - Broadcom NetLink BCM57790 Gigabit Ethernet PCIe [14e4:1694]
  - Broadcom NetLink BCM57791 Gigabit Ethernet PCIe [14e4:16b2]
  - Broadcom NetLink BCM57795 Gigabit Ethernet PCIe [14e4:16b6]
  - Broadcom NetLink BCM5785 Gigabit Ethernet [14e4:1699]
  - Broadcom NetLink BCM5785 Fast Ethernet [14e4:16a0]
  - Broadcom NetLink BCM5787M Gigabit Ethernet PCI Express [14e4:1693]
  - Broadcom Network Adapter [14e4:1689]

----

- `FakePCIID_Intel_GbX.kext`详细支持列表
  - Supported devices for SmallTreeIntel8254x.kext:
  - 8086:1010 82546EB Gigabit Ethernet Controller (Copper)
  - 8086:1011 82545EM Gigabit Ethernet Controller (Fiber)
  - 8086:1012 82546EB Gigabit Ethernet Controller (Fiber)
  - 8086:101d 82546EB Gigabit Ethernet Controller
  - 8086:1026 82545GM Gigabit Ethernet Controller
  - 8086:1027 82545GM Gigabit Ethernet Controller
  - 8086:1028 82545GM Gigabit Ethernet Controller
  - 8086:105e 82571EB Gigabit Ethernet Controller (Also covered by AppleIntel8254XEthernet.kext)
  - 8086:105f 82571EB Gigabit Ethernet Controller
  - 8086:1079 82546GB Gigabit Ethernet Controller
  - 8086:107a 82546GB Gigabit Ethernet Controller
  - 8086:107b 82546GB Gigabit Ethernet Controller
  - 8086:107c 82541PI Gigabit Ethernet Controller
  - 8086:107d 82572EI Gigabit Ethernet Controller (Copper)
  - 8086:107e 82572EI Gigabit Ethernet Controller (Fiber)
  - 8086:10a4 82571EB Gigabit Ethernet Controller
  - 8086:10b5 82546GB Gigabit Ethernet Controller (Copper)
  - 8086:10b9 82572EI Gigabit Ethernet Controller (Copper)
  - 8086:10bc 82571EB Gigabit Ethernet Controller (Copper)

  - SmallTreeIntel82576.kext:
  - 8086:1521 I350 Gigabit Network Connection
  - 8086:1522 I350 Gigabit Fiber Network Connection
  - 8086:1533 I210 Gigabit Network Connection (Also covered by AppleIntelI210Ethernet.kext)

  - SmallTreeIntel8259x.kext:
  - 8086:10c6 82598EB 10-Gigabit AF Dual Port Network Connection
  - 8086:10c7 82598EB 10-Gigabit AF Network Connection
  - 8086:10c8 82598EB 10-Gigabit AT Network Connection
  - 8086:10ec 82598EB 10-Gigabit AT CX4 Network Connection
  - 8086:10d8 82599EB 10 Gigabit Network Connection
  - 8086:10fb 82599ES 10-Gigabit SFI/SFP+ Network Connection
  - 8086:10f1 82598EB 10-Gigabit AF Dual Port Network Connection
  - 8086:151c 82599 10 Gigabit TN Network Connection
  - 8086:150b 82598EB 10-Gigabit AT2 Server Adapter
  - 8086:1528 Ethernet Controller 10-Gigabit X540-AT2
  - 8086:10fc 82599 10 Gigabit Dual Port Network Connection
  - 8086:1560 Ethernet Controller X540