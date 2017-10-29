Title: 板载 intel 网卡不能连接的问题解决
Date: 2016-07-12 22:31
Tags: network, linux
Category: 备忘

板载intel网卡在fedora下不能连接到网络， 而在windows下可以正常连接，
`journalctl -xb`下看到DHCP超时， 设置成静态IP可以显示为"已连接"，
但没有网络， 后来查到[有人遇到linux下正常连接，启动到windwos后再重启到linux不能正常连接的问题][1]，
其**解决方案**是[**重置网卡**][2]，
试了下发现我这里**再重启NetworkManager服务**就能成功分配到网络地址。 

----

重置网卡的脚本如下:

~~~
#Get the PCI-Address of network card (Caution: This works ONLY with ONE NIC)
PCI=`lspci | egrep -i 'network|ethernet' | cut -d' ' -f1`
PCIPATH=`find /sys -name *\${PCI} | egrep -i *pci0000*`

logger -t "ResetNIC" "Resetting PCI NIC ${PCIPATH}"

#Reset the PCI Device completely (like Power-ON/Off)
echo 1 >${PCIPATH}/reset
~~~


[1]: https://bbs.archlinux.org/viewtopic.php?id=191981
[2]: https://bbs.archlinux.org/viewtopic.php?pid=1575719#p1575719
