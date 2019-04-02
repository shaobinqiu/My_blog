---
title: "Swap Caps and Ctrl_L"
date: 2019-04-02
lastmod: 2019-04-02
draft: false
tags: ["Caps"]
categories: ["linux"]
author: "qiusb"
--- 

```
$ cd ~
$ touch .xmodmap
$ vim .xmodmap
```

讲以下内容贴到.xmodmap

```
remove Lock = Caps_Lock
remove Control = Control_L
keysym Control_L = Caps_Lock
keysym Caps_Lock = Control_L
add Lock = Caps_Lock
add Control = Control_L
```

生效

```
xmodmap .xmodmap
```

more information：https://wxnacy.com/2018/10/25/deepin-capslock-control/
