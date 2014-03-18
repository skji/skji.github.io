---
layout: post
title: "使用tumx代替screen"
category: posts
---

经常在linux环境中工作的人,想必对**screen**这个工具不会陌生,如果你不知道,说明你还距离让自己的工作环境最大限度的方便舒适的目标非常远.在使用**screen**一段时间后,我发现了一款比它更棒的工具**tmux**.


----------
###Why Tmux###
**tmux**是和**screen**类似的一款工具,相比**screen**,**tmux**更加方便、灵活和高效:

 - 垂直or水平分割窗口
 - eamcs or vi的按键绑定模式
 - 有多个粘贴缓冲，可完全由按键进行选取、复制、以及粘贴操作
 - 配置很容易，尤其是状态行
 - 脚本化，通过脚本可以方便的控制 tmux 会话
 - 有预设布局，可搜索窗口，自动命名窗口名称
 - 文档清晰、详尽
 
###Tmux Setting###
从 screen 切换到 tmux 十分平滑，tmux 的按键设置与 screen 大都相同，只是其默认按键前缀为 ctrl-b。为了延续在 screen 中的使用习惯，我将其更改为 ctrl-a。将下列内容加到 $HOME/.tmux.conf 中即可：

```
set -g prefix ^a
unbind ^b
bind a send-prefix
```

####按键绑定####

- 水平或垂直分割窗口

```
unbind '"'
bind - splitw -v # 分割成上下两个窗口
unbind %
bind | splitw -h # 分割成左右两个窗口
```

- 选择分割的窗格

```
bind k selectp -U # 选择上窗格
bind j selectp -D # 选择下窗格
bind h selectp -L # 选择左窗格
bind l selectp -R # 选择右窗格
```

- 交换两个窗格

```
bind ^u swapp -U # 与上窗格交换 Ctrl-u
bind ^d swapp -D # 与下窗格交换 Ctrl-d
```

- 定制状态行
状态行左边默认就很好了，我对右边定制了一下，显示 uptime 和 loadavg：

```
set -g status-right "#[fg=green]#(uptime.pl)#[default] • #[fg=green]#(cut -d ' ' -f 1-3 /proc/loadavg)#[default]"
```

下面两行设置状态行的背景和前景色:

```
set -g status-bg black
set -g status-fg yellow
```

###Common Commands###
```
C-a d   //返回主 shell ， tmux 依旧在后台运行，里面的命令也保持运行状态tmux attach 
C-a ?  // 显示快捷键帮助
C-a C-o  //调换窗口位置
C-a 空格键  //采用下一个内置布局
C-a ! // 把当前窗口变为新窗口
C-a "  // 模向分隔窗口
C-a % // 纵向分隔窗口
C-a q // 显示分隔窗口的编号
C-a o // 跳到下一个分隔窗口
C-a 上下键 // 上一个及下一个分隔窗口
C-a C-方向键 //调整分隔窗口大小
C-a & // 确认后退出 tmux
C-a c // 创建新窗口
C-a ，//修改当前窗口名称
C-a 0~9 //选择几号窗口
C-a c // 创建新窗口
C-a n // 选择下一个窗口
C-a l // 最后使用的窗口
C-a p // 选择前一个窗口
C-a w // 以菜单方式显示及选择窗口
C-a s // 以菜单方式显示和选择会话
C-a t //显示时钟
```

