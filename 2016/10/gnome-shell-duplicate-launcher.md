Title: 解决 gnome shell 中自定义 launcher 重复的问题
Date: 2016-10-27 10:22
Tags: gnome-shell, launcher, Android Studio
Category: 备忘

安装 Android Studio 后手动添加了一个 desktop 文件便于启动，
但是每次启动后都会在原来的 launcher 下重新显示一个新的 launcher，
查询后得到以下解决方法[参考链接][1]

- 启动 Android Studio
- 打开终端输入 `xprop WM_CLASS`, 然后点击 Android Studio 程序窗口
- 然后终端会显示 `WM_CLASS(STRING) = "sun-awt-X11-XFramePeer", "jetbrains-studio"`,这里选用 class `jetbrains-studio`
- 将上面选择的 class 作为 desktop 文件中 `StartupWMClass` 的值,这里是 `StartupWMClass=jetbrains-studio`
- 保存 desktop 文件，按 `Alt-F2` 显示快速启动，输入 `r` 重启 gnome-shell, 然后右键点击图标会出现 `Add to Favorites` 菜单，点击即可

[1]: http://askubuntu.com/a/635839
