---
layout: "post"
title: "wrap stuff in debian package"
date: "2017-05-07 22:47"
---

大家都知道在 Ubuntu 上，可以在  launchpad 上申請 ppa 以方便讓人直接用 apt 安裝 package ,如果想讓使用者安裝 package 時作一些其他安裝也可以放在 debian/postinst，但如果想讓使用者安裝別家公司的 private build debian package呢? 問題是 apt 執行中無法再執行另一個 apt 怎麼辨呢?

目前想到的方法是:
 * 透過 service 在後面用裝
  * xxx.service
 * 重新開機後自動安裝
  * autoconf: xxx.desktop

另外還有一些其他問題:
  * 因為在後面裝如果使者不知道如果他關機了怎麼辨
    * 對每一個使用者都送訊息, 告知進度([notification in background]{{ site.baseurl }}{% 2016-05-07-notification-in-background })
     * notify-send, xterm, gnome-terminal
  * 如果使用者不想裝，或至少現在不想裝想晚點再裝怎麼辨
    * install package 時詢問 user: debconf
    * autoconf start 時詢問 user: zentify, dialog
