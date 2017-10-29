Title: HTC Desire S(g12) 距离感应器参数调整
Date: 2011-08-04 23:27
Tags: Android, optical sensor
Category: 备忘

desire s 有个距离感应器,
主要是用于防止打电话时误触屏幕.
是一个根据光线强度判断手机附近遮蔽物的小东西,
属于光学感应器,
位于屏幕上方 logo 的左侧.

我的手机这个东西的校准值有问题,
导致不能正常工作,
现在列下**修改方案**.

root 后安装 teminal emulator 或通过 adb shell 执行
`echo 0x10>/sys/devices/virtual/optical_sensors/proximity/ps_canc`


**说明:**

- `/sys/devices/virtual/optical_sensors/proximity/ps_canc` 文件设置了降噪级别.

- 可通过 `echo 0x10>/sys/devices/virtual/optical_sensors/proximity/ps_canc` 来设置降噪级别为 0x10. 
0x10 这个值是从正常工作的手机上获取的,
我手机上原有值为 0x07,
不能正常工作.

这样设置重启后则须重新设置.为了使用方便,我修改了init.d里的脚本来实现启动时配置.
~~~
>adb remount
>adb shell
#echo "echo 0x10>/sys/devices/virtual/optical_sensors/proximity/ps_canc" >> /system/etc/init.d/99-proximity-recalibration
#exit
~~~

即先使用 remount 将 system 所在分区挂载为可写,
然后在 init 脚本里追加设置语句.

**以下为问题总结:**

1. 最开始现象为打电话时系统自动锁屏,
必须手动按挂机键解锁.
以为是为了防止贴近脸部打电话时的误触操作,
但拨打 1008611 之类需要输入数字时较不方便,
以为是"不够人性化的设计".

2. 破解为 s-off 后装了第三方 rom,
拨打电话时仍自动锁屏,
但通话未结束前不能通过挂机键解锁,
导致对方简单锁屏后我这边一直处于通话中.
以为是第三方 rom 的 bug.

3. 纠结后刷入官方最新 rom,
包含最新 radio,
问题同1

4. 了解到同事的三星 android 以及华为 android 无此问题.
安装 z-device test,
发现距离感应器一项始终显示距离为0.
同事的则可根据遮挡距离变化显示不同数值.
以为硬件问题,
准备去修.

5. 次日发现在太阳光下可显示距离变化,
怀疑驱动问题.

6. 搜索类似问题,
在 xda 上找到一个其他款手机的距离感应器校准小程序.
下载后不适用.

7. 后发现小程序提供了源码,
查看发现对 `/sys/devices/virtual/optical_sensors/proximity/` 文件夹下的文件进行了操作.
查看自己手机,
文件结构不一致.

8. 探索发现 ps_canc 文件最简单,
尝试修改.
发现照原有格式
`echo PS_CANC = 0x01 > ps_canc`
文件值改变,
但不是给定的值而是0x70,
此时光学感应器可使用了

9. 搜索 ps_canc 等,
找到某人的源码仓库,
发现 `sscanf("0x%d",...)` 类似语句,
尝试使用 `echo 0x01 > ps_canc` 等语句,
发现文件内容按给定值改变.
遂测试各个值的影响,
选用了0x20

10. 查看同学功能正常的机器,
设定值为0x10,
即按此修改.

11. 后发现重启后设置丢失,
须重设.
试图添加,
但提示文件系统只读.

12. 搜索后得到 adb remount 命令可以将文件系统挂载为可写.
遂修改

以上探索过程中,尝试修改参数这一环节比较危险,之后应尽量避免
