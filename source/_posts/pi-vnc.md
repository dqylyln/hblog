---
date: 2013-04-08
title: Raspberry Pi 配置VNC
categories:
- RaspberryPi
tags:
- 配置VNC
---

今天一个同事问我是否能远程访问RaspberryPi的桌面环境，当时一口回答VNC，至于咋搞一直没尝试过，因为我一直认为Raspberry Pi上搞X的话，性能真
是惨不忍睹。但是本着探索的精神今天还是尝试了下顺便记录了下过程。

安装 tightvncserver
===

    sudo apt-get install tightvncserver

启动VNC Server
===

    vncserver

设定访问密码,会出现两次，之后会询问一个只读密码，n跳过

客户端连接
===

![](//www.tightvnc.com/logo/tightvnc-logo-90x90.png)

tightvnc的客户端有很多平台的，可以根据需要下载 [下载连接](//www.tightvnc.com/download.php)
<!--more-->
安装好客户端后，输入要连接RaspberryPi的地址以及端口(默认端口是5901) 就可以连接了

当然你也可以让RaspberryPi上的VNC的SERVER默认启动，那么可以参考以下方法。

设定开机启动(如果你愿意)
===

准备开机启动脚本
---
    sudo vi /etc/init.d/tightvncserver

*下面是脚本内容

    #!/bin/sh

    ### BEGIN INIT INFO
    # Provides:             tightvnc
    # Required-Start:       $remote_fs $syslog
    # Required-Stop:        $remote_fs $syslog
    # Default-Start:        2 3 4 5
    # Default-Stop:         0 1 6
    # Short-Description:    Start VNC Server as a service
    ### END INIT INFO

    VNCUSER='pi'
    eval cd ~${VNCUSER}

    case "$1" in
        start)
            su ${VNCUSER} -c '/usr/bin/tightvncserver :1'
            echo "Starting TightVNC server for ${VNCUSER}"
            ;;
        stop)
            pkill Xtightvnc
            echo "Tightvncserver stopped"
            ;;
        *)
            echo "Usage: /etc/init.d/tightvncserver {start|stop}"
            exit 1
            ;;
    esac

    exit 0

修改启动脚本权限
---

sudo chmod 755 /etc/init.d/tightvncserver

添加到开机启动
---

    sudo update-rc.d tightvncserver defaults

XDRP
===
如果感兴趣可以安装XDRP，这个玩意也依赖VNC。好处是可以直接使用WIN的远程桌面连接，而速度快。

安装命令：

    sudo apt-get install xrdp

注意：默认端口3389 如果是外网需要自己路由器映射暴漏这个端口。

WINDOWS 下远程桌面

![WIN远程桌面](//oss.aliyuncs.com/faxianla/wood/m939474-1366612952465.png)

![登录成功](//oss.aliyuncs.com/faxianla/wood/m939475-1366612965244.png)

输入你的用户名密码即可连接