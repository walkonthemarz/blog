##### VirtualBox 安装CentOS7无法使用桥接模式
  今天安装CentOS7时，需要使用VBox的桥接网卡模式，但是一直无法启动网卡，但是换成NAT模式却能正常启动网卡；一直不知道原因，因为一开始安装CentOS6的时候使用桥接模式正常，但是装CentOS7却无法起来，换了各种CentOS7版本，后来[Stack Overflow](https://stackoverflow.com/questions/31922055/bridged-networking-not-working-in-virtualbox-under-windows-10)找到了方法，原来是要手动建一个虚拟网络适配器，然后将该适配器于宿主机网络适配器进行桥接，然后虚拟机器的桥接网卡选择刚刚的虚拟适配器就行了，问题解决。
  其实一开始Google的时候就看到了这个链接，当时认为之前的CentOS6都行，应该不会是这个问题，觉得应该是镜像的问题，可能是CentOS7自动启用了IPv6导致的问题，后来一直没解决，就尝试了一下，成功了。哎，当时就应该想到，既然NAT没有问题，那么镜像肯定是没问题的，问题只能出在网卡方面，照着Google出来的第一个方案设置一下很快就能解决的，白白浪费了那么多时间。:unamused:
