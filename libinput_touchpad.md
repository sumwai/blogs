---
title: "linux 触摸板配置"
date: 2022-06-13T23:25:45+08:00
draft: true
---

## 借鉴文章

[libinput (简体中文)#使用 Xorg 配置文件][1]

在笔记本安装Linux系统时，需要为笔记本修改触摸板的相关配置，如“轻触点击”、“自然滚动”等，以下是配置触摸板的流程。

## 1. 复制示例文件

首先，我们需要将示例文件`/usr/share/X11/xorg.conf.d/40-libinput.conf`复制到`/etc/X11/xorg.conf.d/`目录下。

```bash
sudo cp /usr/share/X11/xorg.conf.d/40-libinput.conf /etc/X11/xorg.conf.d/
```

## 2. 修改配置

使用vim打开配置文件`/etc/X11/xorg.conf.d/40-libinput.conf`。

```conf
# Match on all types of devices but joysticks
#
# If you want to configure your devices, do not copy this file.
# Instead, use a config snippet that contains something like this:
#
# Section "InputClass"
#   Identifier "something or other"
#   MatchDriver "libinput"
#
#   MatchIsTouchpad "on"
#   ... other Match directives ...
#   Option "someoption" "value"
# EndSection
#
# This applies the option any libinput device also matched by the other
# directives. See the xorg.conf(5) man page for more info on
# matching devices.

Section "InputClass"
        Identifier "libinput pointer catchall"
        MatchIsPointer "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection

Section "InputClass"
        Identifier "libinput keyboard catchall"
        MatchIsKeyboard "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection

Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        Option "Tapping" "on"
        Option "NaturalScrolling" "true"
        Option "ClickMethod" "clickfinger"
        Driver "libinput"
EndSection

Section "InputClass"
        Identifier "libinput touchscreen catchall"
        MatchIsTouchscreen "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection

Section "InputClass"
        Identifier "libinput tablet catchall"
        MatchIsTablet "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection
```

## 3. 配置信息
上面是已经修改过的配置文件，这里解释一下配置的意思。

```conf
Section "InputClass"
        Identifier "libinput touchpad catchall"
        # 这个表明当前配置项是匹配触摸板的，修改的是关于触摸板的配置
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        # 轻触点击
        Option "Tapping" "on"
        # 自然滚动
        Option "NaturalScrolling" "true"
        # 不分触摸板区块，双指右键，三指中键
        Option "ClickMethod" "clickfinger"
        Driver "libinput"
EndSection
```

## 4. 重启x服务

在修改完配置之后，需要重启x服务才会生效。这里我使用的是`sddm`管理器，所以直接重启`sddm`，重启电脑也是可以的。

### 重启sddm

```bash
sudo systemctl restart sddm
```

### 重启电脑

```bash
sudo reboot
```

[1]: https://wiki.archlinux.org/title/Libinput_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E4%BD%BF%E7%94%A8_Xorg_%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6
