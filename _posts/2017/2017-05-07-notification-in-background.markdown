---
layout: "post"
title: "notification in background"
date: "2017-05-07 23:59"
---

在背景執行的程式如何透過跟使用者的互動以通知或是獲得下一步的指示，目前我用過的方法有下列幾個，如有其他的再請分享:
依情境分類:
* 在同一login account 中執行的程式要通知 user
  * xterm
  * gnome-terminal
  * notify-send
  * zentify
* 在背景跑的 root 程式要通知 user, 但同時可能有多個 user 登入
  * wall - 只有 terminal 會收到
  * by xwindow program:
```bash
 notify-user(){
 [ $# == 0 ] && return
 local xuser_display_pairs=($(who | sed 's/[(|)]/ /g' | awk '{print $1 " " $5}' | grep ":"))
 for(( i=0; i < ${#xuser_display_pairs[@]}; i+=2));
 do  
# by notify-send
    XAUTHORITY=/home/${xuser_display_pairs[$i]}/.Xauthority DISPLAY=${xuser_display_pairs[$((i+1))]} notify-send -u critical "$@"
# or by xterm
XAUTHORITY=/home/${xuser_display_pairs[$i]}/.Xauthority DISPLAY=${xuser_display_pairs[$((i+1))]} xterm -e "while [ -e /tmp/wait ]; do echo wait 3 secs for $@; sleep 3;done"
 done
 wall "$1"
 }
```

同場加映，如果只是要在執行的時後使用互動可以直接使用:
* dialog - [example](https://github.com/alex-tu-cc/scripts/blob/master/ubuntu/mainline-kernels.sh)
* [bash - switch - case](http://stackoverflow.com/questions/226703/how-do-i-prompt-for-yes-no-cancel-input-in-a-linux-shell-script)
* zentify

另外如果是要在安裝 package 時跟使用者互動則可以使用 [notification in background]{{ site.baseurl }}{% 2017-05-07-debian-package-debconf }

Reference:
* http://users.stat.umn.edu/~geyer/secure.html
