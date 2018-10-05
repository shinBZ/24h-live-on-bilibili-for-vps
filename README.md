# 24h-raspberry-live-on-bilibili-for-vps

基于晨旭的树莓派驱动的b站直播弹幕点播台修改而来

[https://github.com/chenxuuu/24h-raspberry-live-on-bilibili.git](https://github.com/chenxuuu/24h-raspberry-live-on-bilibili.git)

非常感谢大佬的点播台，但是原来的是基于树莓派的，我做了小小的修改，以支持VPS（当然还是有点儿小问题）
-------
### 以下是原来的简介

本项目基本编写完毕，已经有的功能为：

- 弹幕点歌
- 弹幕点MV
- 弹幕反馈（发送弹幕）
- 旧版实现的视频推流功能
- 自定义介绍字幕
- 歌词滚动显示，同时滚动显示翻译歌词
- 切歌
- 显示排队播放歌曲，渲染视频
- 通过弹幕获取实时cpu温度
- 闲时随机播放预留歌曲
- 播放音乐时背景图片随机选择
- 可点播b站任意视频（会员限制除外，番剧根据b站规定，禁止点播）
- 已点播歌曲、视频自动进入缓存，无人点播时随机播放
- 存储空间达到设定值时，自动按点播时间顺序删除音乐、视频来释放空间
- 实时显示歌曲/视频长度
- 根据投喂礼物的多少来决定是否允许点播

已知问题：

- 换歌、视频时会闪断
- 不支持弹幕命令【温度】内的CPU温度，其余正常（这是）

### 以上是原来的简介

## 使用方法：

我这里用的是Ubuntu18.04，其他系统请自测

不过应该都是可以的，而且许多人用的都是16.04，升级到18.04也很简单


### 一：

```Bash
sudo apt update
sudo apt dist-upgrade -y
sudo apt install -y ffmpeg x264 x265 libx264* libx265*
```

libmp3lame：

```Bash
sudo apt install libmp3lame-dev -y
```

libopus:

```Bash
sudo apt install libopus-dev -y
```

libvpx:

```Bash
sudo apt install libvpx-dev -y
```

安装python3的mutagen库：

```Bash
sudo pip3 install mutagen
```

安装python3的you-get库：

```Bash
sudo pip3 install you-get
```

安装python3的moviepy库：

```Bash
sudo pip3 install moviepy
```

安装python3的aiohttp库：

```Bash
sudo pip3 install aiohttp
```

安装screen:

```Bash
sudo apt install screen -y
```

安装中文字体

```Bash
sudo apt install fontconfig
sudo apt-get install ttf-mscorefonts-installer
sudo apt-get install -y --force-yes --no-install-recommends fonts-wqy-microhei
sudo apt-get install -y --force-yes --no-install-recommends ttf-wqy-zenhei
#可能有装不上的，应该问题不大

# 查看中文字体 --确认字体是否安装成功
fc-list :lang=zh-cn
```

（字体安装来自[ubuntu下 bilibili直播推流 ffmpeg rtmp推送](https://ppx.ink/2.ppx)）

### 烹饪&摆盘：

下载本项目：

```Bash
git clone https://github.com/shinBZ/24h-raspberry-live-on-bilibili-for-vps.git
```

请修改下载里的`var_set.py`文件中的各种变量
其中，`cookie`需要使用小号（尽量使用小号，并且b站账户好像需要绑定手机号后才能发送弹幕）在直播间，打开浏览器审查元素，先发一条弹幕，再查看`network`选项卡，找到`name`为`send`的项目，`Request head`中的`Cookie`即为`cookie`变量的值。注意设置后，账号不能点击网页上的“退出登陆”按键，换账号请直接清除当前cookie再刷新

`csrf_token`请填写`Request head`中的`csrf_token`

`post_dm.py`文件的`if(user == '接待喵'):  #防止自循环`请改为你的机器人的名字

标注`#debug使用，请自己修改`的代码请自行修改，此为debug用的代码

如有条件，请`务必`自己搭建php的下载链接解析服务，源码都在`php`文件夹内

`default_mp3`文件夹内放入mp3格式的音乐，在无人点歌时播放，请尽量保证文件名全英文（可要可不要，因为现在已经改为放点播过的缓存歌曲、视频了）

`default_pic`文件夹内放入jpg格式的图片，用于做为放音乐时的背景，请尽量保证文件名全英文，分辨率推荐统一处理为1280x720

所有配置完成后，开启直播，然后启动脚本即可：

```Bash
screen python3 play.py
#按ctrl+a,按ctrl+d
screen python3 bilibiliClient.py
#按ctrl+a,按ctrl+d
#弹幕监控使用了弹幕姬python版：https://github.com/lyyyuna/bilibili_danmu
#感谢弹幕姬python版作者的分享
```

本程序协议为GPL

本README基于原来的修改而来
