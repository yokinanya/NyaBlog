---
title: '使用Termux给另一台设备刷机'
date: 2025-03-28 22:58:43
updated: 2025-03-28 22:58:47
tags: ['Termux']
---

为方便演示截图，本文截图使用ssh了连接手机上的termux操作，实际与完全在手机上操作无任何差异。

本教程已经过群友验证，全程不需要root权限

# 准备
1. 下载并安装 [Termux](https://f-droid.org/packages/com.termux/) 和 [Termux:API](https://f-droid.org/packages/com.termux.api/)
2. 为Termux:API自启动权限
3. 配置termux镜像源 (有配置过的可以跳过)
```shell
termux-change-repo
```
通过上下键移动选择框，空格为选中，回车为确定。
![](https://cdn.jsdelivr.net/gh/yokinanya/NyaBlog-Resource@master/images/termux/01.png)
选中 `Mirrors in Chinese Mainland` （如果人不在中国大陆可以选择对应的其他选项）按空格，然后回车
![](https://cdn.jsdelivr.net/gh/yokinanya/NyaBlog-Resource@master/images/termux/02.png)

4. 执行 `pkg update && pkg -y upgrade` 更新软件包

5. 安装 [termux-adb](https://github.com/nohajc/termux-adb) (注意，不是termux软件仓库的android-tools)
```shell
curl -s https://raw.githubusercontent.com/nohajc/termux-adb/master/install.sh | bash
```

6. 安装 root-repo
```shell
pkg install root-repo
```

# 刷机
1. 将需要刷机的手机重启进入fastboot模式，使用otg转接头或者ctoc数据线连接两台手机
2. 为termux授权读取文件权限，`termux-setup-storage` 输入y回车确认
![](https://cdn.jsdelivr.net/gh/yokinanya/NyaBlog-Resource@master/images/termux/03.png)
3. 将需要刷入的镜像 (刷机包需要解压，并且必须是线刷包) 到手机的Download (Documents、Pictures、DCIM、Music、Movies等公共目录均可) 目录下，演示使用Download目录
4. 通过 cd 命令进入刷机包目录或者镜像所在目录，路径需要替换为对应的实际路径，比如 Download 则替换为 `~/storage/downloads` ，具体可以查看termux的[文档](https://wiki.termux.com/wiki/Termux-setup-storage)，假设我的镜像存放于Download目录下的rom目录内，则路径为 `~/storage/downloads/rom/`
```shell
cd ~/storage/downloads/rom/
```
5. 执行 `termux-fastboot flash boot boot.img` 刷入boot文件，同理，执行 `termux-fastboot flash recovery recovery.img` 刷入recovery，如果需要刷入刷机包，需要编辑对应的刷机脚本，如 flash_all.sh ，替换其中的 `fastboot` 为 `termux-fastboot` 然后执行，`bash ./flash_all.sh` 刷机，在此期间，可能会出现如下图所示弹窗，确认即可
<img src="https://cdn.jsdelivr.net/gh/yokinanya/NyaBlog-Resource@master/images/termux/04.jpg" height="600">

6. 不出意外的话应该没什么意外