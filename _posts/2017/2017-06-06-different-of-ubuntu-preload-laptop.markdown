---
layout: "post"
title: "preload Ubuntu 的筆電有什麼不同?"
date: "2017-06-06 18:13"
---

記得早期買電腦的時後，可以選擇不要 OS (通常就是 Windows), 可以便宜幾千元，但現在似乎沒有這個選項了，基本上就是預載 windows 作業系統。如果想要買預載其他作業系統的通常都不太好找, 以小魯為一個攻城獅來說，其實比較常用的是 Linux 界的 Ubuntu 系統。所以預載 Ubuntu 的筆電，是一個不錯的選項，但一定會有人說，上網抓 official release 就好了呀，幹嘛那麼麻煩 ?

是的，我以前也是這樣想，所以想來比較一下，預載和 official public release (同事叫它 stock Ubuntu)有什麼不同?

## 價錢
較常聽到的是，"preloaded Ubuntu 的價錢較 preloaded Windows 的便宜，原因是，微軟收的錢比較多" 在某些機子是這樣的，但我認為這還是取決於品牌廠想賺多少，它可以每台 Ubuntu 機型賺多一點，每台 Windows 機型賺少一點，，而且還有微軟補貼等太多因素會影响價格。下面就較多人選購 Ubuntu 版本的 Dell XPS13 目前看到 Dell 官網上的價錢為例，定價上 Ubuntu 版本便宜 100美元，但windows 版有送 100元折價券，往返之前兩個的價錢是一樣的。這完全要看品牌廠的商業操作，只能看那天買 Ubuntu 的人變多後，價格是否會掉下來。(節日或許也會便宜一點)

 ![Dell XPS13]({{ site.baseurl }}/images/2017/06/dell-xps13-price.png)

## Maintenance
記得前幾年買手機，大家都會談到會不會過陣子原廠就不再發佈更新，變成手機孤兒。筆電也是一樣，除了一些 stock Ubuntu 也會有的 security 更新之外，還有一些是硬體相關的 hardware driver 更新並不一定會放到 stock Ubuntu 中，而是放到 issue 發生的相關機種的 ppa 中去作 on-line update.

以 Dell 為例，在 public 的 apt archive 中，可以看到很多機種，每台 preloaded Ubuntu 都會帶一個 project meta package, 指到它自已的一個 archive ppa，hardware 相關的 solution 出來時，都將透過這些 ppa 發佈。另外還有一個 dell 的 channel, 用來發佈通用的 dell 機種更新。
 * http://dell.archive.canonical.com/updates/dists/

那為什麼不把所有的 solution 都同步放到 stock Ubuntu 中呢? 因為硬體不盡相同，在 QA 人力的限制下，只能針對 Ubuntu preloaded 的機種先作驗証，當然也因為大多的 issue 都是從 preloaded Ubuntu 機種中被呈報。畢竟不像 Windows 勢力大每個 hardware vendor 都自動會去修它們自已的問題，並主動過 Windows 認証並推送到 Update channel。所以... 還是要使用者多指定有 Linux driver 的廠商商品才行。

## Certification
就我所知，preloaded Ubuntu 機種都是在開賣之前由專屬 Ubuntu 工程師和 QA 參於測試和解決問題，並將最後 solution 放入 preloaded image 之中，和上一項說法相同，這些 solution 並不一定會放到 stock Ubuntu 中，每個 preloaded Ubuntu image 也不一定相同 (hardware A 的東西不會放到 hardware B 上，對吧?)

也就是說，每個出貨都有人花很多時間幫使用者測試過，也拿到 Ubuntu certification，消費者不用買了一個 preloaded windows NB回來後，裝上 Ubuntu 發現有些地方不能用再上網 Google 修正。

網上有時會看到買同機種的 windows 版本消費者詢問自行安裝 stock ubuntu 的問題，這是因為在開發過程中，不論是 BIOS, OS images 都有為 Ubuntu 版本調校過。消費者買來再裝 stock Ubuntu 還是有些差異的。

## oem-priority
launchpad 上面有一個 oem-priority project, 用來收集 preloaded Ubuntu 機種上面使用者報出的問題，並依優先權處理，最後的 solution 也會依其是不是特定機種相關放到 stock Ubuntu 或是各個機種的 PPA.
 * [https://bugs.launchpad.net/oem-priority](https://bugs.launchpad.net/oem-priority)

## 怎麼找 Ubuntu preloaded laptop?
以 Dell 為例，它將 preloaded Ubuntu 的機種放在"developer"類別:
 * [http://www.dell.com/learn/us/en/555/campaigns/xps-linux-laptop?c=us&l=en&s=biz](http://www.dell.com/learn/us/en/555/campaigns/xps-linux-laptop?c=us&l=en&s=biz)

點 "shop now" 後，可以修改 "Refine your search:" 去找想要的配備，上面的 XPS13 也是這樣找出來的。

或是直接上 Ubuntu certification 網頁找，但是它的排序是用字母好像沒什麼參考性，畢竟大家想換電腦應該都是看其硬體效能如何。
 * [https://certification.ubuntu.com/desktop/models/?category=Laptop&vendors=Dell&page=9](https://certification.ubuntu.com/desktop/models/?category=Laptop&vendors=Dell&page=9)

## 結論
直接購買 preloaded Ubuntu 的確是比較省時，省力，又有後續的維護。就算有問題，因為同機型的使用者多也容易找到解法或是 oem-priority 會放在較高的優先權。

定價上也比 windows 機種便宜，如果確定就是要用 Ubuntu 沒必要買 Windows 版本來砟自已的腳, preloaded Ubuntu (or any Linux distribution) 的市場也是要有消費者支持才能存續或存長。
