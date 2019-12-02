## LEDE 使用 Netatalk 搭建 TimeMachine 

主要参照 [OpenWrt 官网的教程](https://openwrt.org/docs/guide-user/services/nas/netatalk_configuration)搭建，其中有部分配置做了修改，详细的 afp.conf 配置参考 [Netatalk 的官网手册](http://netatalk.sourceforge.net/3.1/htmldocs/)。

记录其中几个点：

1.使用root用户作为AFPD的用户应该是默认被禁止了，一直无法连接TimeMachine磁盘，所以得创建普通用户；

2.在备份目录中得手动创建 cnid scheme 目录：**CNID**。



**/etc/afp.conf**:

```ini
[Global]
uam list = uams_dhx.so,uams_dhx2.so
save password = no
vol dbpath = /mnt/sdb/TimeMachine/CNID
sleep time = 1
hostname = Openwrt
signature = Openwrt

[TimeMachine]
path = /mnt/sdb/TimeMachine
cnid scheme = dbd
ea = auto
time machine = yes
vol size limit = 512000
valid users = @users
```

