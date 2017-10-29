Title: 笔记本在linux下重复待机唤醒的问题
Date: 2016-12-06 16:25
Tags: systemd, linux
Category: 备忘

很久之前在用 gentoo 的时候，
出现了笔记本在开机后
重复待机唤醒的问题，
一直没有解决，
后来换了 ubuntu 来规避这个问题，
前一段发现了问题原因，
在这里记录一下：

    systemd 会通过 acpi 来获取笔记本盖子状态，
    而在某些笔记本上系统刚启动时的状态是错误的，
    需要真正触发了合上盖子打开盖子的行为才会得到正确的结果，
    在 `/etc/systemd/logind.conf` 中的配置项
    `HandleLidSwitch` 默认值是 `suspend` ，
    就会导致 systemd 在刚开机时因认为盖子被合上了而待机，
    为什么会被唤醒暂时还不清楚，
    不过这里只要把 `HandleLidSwitch` 设为 `ignore` 就好了
    
