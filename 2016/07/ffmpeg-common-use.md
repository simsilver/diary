Title: FFMPEG 使用备忘
Date: 2016-07-10 00:57
Tags: ffmpeg, video edit
Category: 备忘 

**转换输入源到指定分辨率并加入黑边**

~~~
ffmpeg -i input.m2ts -s 960x720 -vf scale=960:540,pad=960:720:0:90 -ac 2 output.mp4
~~~

- "-i input.m2ts" 输入的文件名
- "-s 960x720" 指定输出分辨率为960x720
- "-vf scale=960:540,pad=960:720:0:90" 将原始文件缩放到960x540，然后在上边补90高的黑边，下边会自动补
- "-ac 2" 转换为立体声

----

**使用单张图片生成视频**

~~~
ffmpeg -loop 1 -i image.jpg -i audio.ogg -t 4 -s 960x720 -pix_fmt yuv420p -r 25 output.mp4
~~~

- "-loop 1" 当图片仅有一张时指定
- "-t 4" 指定生成的视频长度为4秒
- "-pix_fmt yuv420p" 指定格式，拼接视频时保持所有视频一致
- "-r 25" 指定fps为25

----

**合并视频文件**

~~~
ffmpeg -f concat -i list.txt -c copy out.mp4
~~~

- "-f concat" 指定连接模式
- "-i list.txt" 每行形如"file input1.mp4"的多行文本
- "-c copy" 直接复制音视频流，所有源视频文件音视频格式一致时使用，图片生成的视频需加音频流，否则合并会失败
