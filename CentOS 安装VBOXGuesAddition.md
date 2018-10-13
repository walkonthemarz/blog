##### CentOS 安装VBOXGuesAddition

* 先安装所需要的包：

  ```shell
  sudo yum update
  sudo yum install -y bzip2 kernel-devel dkms kernel-headers
  ```

* Reboot CentOS

* 到[http://download.virtualbox.org/virtualbox](http://download.virtualbox.org/virtualbox) 下载对应的VBoxGuestAdditions_5.x.xx.iso 镜像

* vbox设置里面，存储里面挂载光驱，选择下载的iso文件

* 进入CentOS

  ```shell
  sudo mkdir -p /media/cdrom
  sudo mount /dev/cdrom /media/cdrom
  sudo bash /media/cdrom/VBoxLinuxAdditions.run
  ```

* 安装成功后，vbox设置里面，选择共享目录，添加共享目录，设置共享文件夹名称（假设为 **slice**），选择**固定分配**，**只读分配**

* 编辑/etc/fstab文件，添加开机自动挂载，将下面一行添加到文件末尾：

  ```
  slice /home/setup/steam vboxsf defaults 0 0
  ```

* 执行 **sudo mount -a**，df -h 查看一下是否成功挂载目录；

* 之后重启CentOS，df -h 查看是否自动挂载了共享目录

##### 补充：
* 测试使用了一个新装的CentOS7 系统，由于脚本没有执行 ``` sudo yum update ```命令，新装的系统的内核版本依旧是旧版本：**3.10.0-693.el7.x86_64**, 但是```yum install kernel-devel```却只会安装包管理器中最新更新的内核版本：**3.10.0-862.14.4.el7.x86_64**， 导致当前系统依旧运行的是就内核版本，但是安装的kernel-devel 包却是包管理器中最新的版本，VBoxLinuxAdditions.run 执行时一直报找不到匹配的旧内核头文件(3.10.0-693.el7.x86_64)，
尝试执行```yum install kernel-devel-$(uname -r)```却无法找到对应的包文件，yum update更新内核到包管理器中最新版即可
