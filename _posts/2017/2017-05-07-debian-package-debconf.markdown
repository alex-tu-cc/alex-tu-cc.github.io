---
layout: "post"
title: "debian package: debconf"
date: "2017-05-07 23:57"
---

想在 user 裝 package 時, 讓使用者設定可以有幾個方式:
* dialog - 只能作用於本地安裝 (dpkg -i)
* debconf - 透過 archive/ppa 安裝時有效 (dpkg -i 安裝時無效)

dialog 不用說了，debconf 的話可以參考 [webupd8team 的 java installer][1], 及 [The Debconf Programmer's Tutorial][], 或是[我的小測試][2].

其主要是用 /debian/templates用以宣告將要使用的變數，再利用/debian/config 在 apt-config 階段詢問使用者問題，也可以放在 postint 中去詢問(ex. java installer)。在我的測試，不知為何，用 db_input priority medium 以下的都不會 show。


[1]: https://launchpad.net/~webupd8team/+archive/ubuntu/java
[2]: https://git.launchpad.net/~alextu/somerville/+git/test-meta-meta/tree/?h=no-binary
[The Debconf Programmer's Tutorial]: http://www.fifi.org/doc/debconf-doc/tutorial.html
