Title: 关闭 Gnome 终端中的 beep 提示
Date: 2016-10-27 22:23
Tags: gnome, beep, pcspkr
Category: 备忘

gnome-terminal 在补全遇到多个可匹配项或其他一些时候会发出 beep 音，
有时比较烦人，
搜了下关闭方法:
~~~
gsettings set org.gnome.desktop.wm.preferences audible-bell false
~~~

[参考链接][1] 中提示了多种方法，
以前记得使用 `rmmod pcspkr` 的方法很好用，
这次不知为什么没有成功,
而使用 gsettings 的方法成功了。

[1]: https://wiki.archlinux.org/index.php/Disable_PC_speaker_beep#In_GNOME
