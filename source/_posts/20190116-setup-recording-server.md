---
title: Ubuntu18.04で録画サーバーを構築した
date: 2019-01-16 18:49:41
tags:
    - Server
    - Ubuntu
---

[録画サーバーを建てた](/2018/12/30/20181230-recording-server/) で購入したサーバー機に、Ubuntu Server 18.04 LTSを入れて録画サーバーにした話。

<!--more-->

# BIOSの設定
`SATA Mode`を`RAID Mode`から`AHCI Mode`に変更。

`Power Failure Recovery`を`Previous State`から`Disabled`に変更。

# Ubuntu Server 18.04 LTS
タイトルに書いてある通りOSはUbuntuを選択。特にこだわりはなく、単に一番多く使ったことがあるLinuxだからというだけです。Azure for Microsoft Imagine(Microsoft Imagine)からWindows Server 2016を貰ってインストールする手もありますが、面倒くさそうだったのでやめました。Linuxで不自由は無いはず。

が、Ubuntuのインストール中にネットワーク設定のところで止まってしまい、先に進めない。どうやらubuntu-18.04.1.0-live-serverのISOだとオンライン環境が必要らしい。ubuntu-18.04.1-serverのISOでインストールをやり直したところ問題なく終わりました。

[Install 18.04 server without network connection](https://askubuntu.com/questions/1042364/install-18-04-server-without-network-connection)

# ドライバ
チューナーはPX-W3PE4なので、PLEX社のサイトからUbuntu18.04向けのものをダウンロードしてインストール。

が、このドライバをインストールしたあとで`checksignal`を実行しても常に`No Signal!`で受信ができない。

<iframe src="https://mstdn.maud.io/@Otakan951/101279292259273091/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://mstdn.maud.io/embed.js" async="async"></script>

原因はよく分かりませんが、[非公式ドライバ](https://github.com/nns779/px4_drv)に切り替えたら受信できるようになりました。kernelの制限などを受けないので、非公式の方が良いのかもしれないです。知らんけど。

# ffmpeg
Kaby LakeなCeleronなので、Intel Media Server Studio 2018 R2を使ったQSVエンコードは使えません。Media Server Studioを使ったエンコードには及ばないものの、VAAPIを使えばQSVエンコードが可能らしい。[Ubuntu 18.04 LTS QSVエンコード 成功？編](http://nodoka.org/ubuntu-18-04-lts-qsv%E3%82%A8%E3%83%B3%E3%82%B3%E3%83%BC%E3%83%89-%E6%88%90%E5%8A%9F%EF%BC%9F%E7%B7%A8/)を参考にインストール。一度失敗したけど最初からやり直したらうまく使えました。

<iframe src="https://mstdn.maud.io/@Otakan951/101291203969366204/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://mstdn.maud.io/embed.js" async="async"></script>

<iframe src="https://mstdn.maud.io/@Otakan951/101296160408382033/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://mstdn.maud.io/embed.js" async="async"></script>

# Mirakurun
[このサイト](https://www.jifu-labo.net/2016/05/mirakurun/)を参考にインストールしました。PX-W3PE4に非公式ドライバを使っているので、その辺りを多少読み替えつつ作業しましたが、特に問題は起きなかった…ハズ。

# EPGStaion
最初はChinachuを使うつもりでしたが、EPGStationのほうがスマホで使いやすいらしい。視聴はPCでも予約はスマホからやることが多いと思ったので、EPGStationを使ってみることにしました。

公式の[インストールマニュアル](https://github.com/l3tnun/EPGStation/blob/master/doc/linux-setup.md)を参考にインストール。DBは推奨になっていたMySQLを使用。DBの文字コードをUTF-8にするのを忘れて時間を潰した…

# EPGS-to-Discord
EPGStationの状態をDiscordに飛ばすやつです。予約の追加、録画開始、録画終了を知らせてくれる。録画終了時にはドロップが発生したかどうかも教えてくれます。

使い方が書いてなかったのでメモ。
```
$ git clone https://github.com/advancedbear/EPGS-to-Discord.git
$ cd EPGS-to-Discord
$ npm install
$ cp config.json.sample config.json
```
`config.json`の`host`と`webhookURL`を環境に合わせて書く。Webhook URLは[Intro to Webhooks](https://support.discordapp.com/hc/en-us/articles/228383668-Intro-to-Webhooks)を参考に取得。

EPGStationの`config.json`の編集
```
"isEnabledDropCheck": true,
"reservationAddedCommand": "/usr/bin/node <EPGS-to-Discord>/index.js reserve",
"recordedStartCommand": "/usr/bin/node <EPGS-to-Discord>/index.js start",
"recordedEndCommand":"/usr/bin/node <EPGS-to-Discord>/index.js end",
```
※ `<EPGS-to-Discord>` はEPGS-to-Discordのフルパス

こんな感じに飛んできます。

<iframe src="https://mstdn.maud.io/@Otakan951/101398513456406094/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://mstdn.maud.io/embed.js" async="async"></script>

<iframe src="https://mstdn.maud.io/@Otakan951/101400477578566201/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://mstdn.maud.io/embed.js" async="async"></script>

# エンコード結果がおかしい

<iframe src="https://mstdn.maud.io/@Otakan951/101317640426528987/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://mstdn.maud.io/embed.js" async="async"></script>

VAAPIでエンコードしたら、出力結果が酷い。なんだこれ。発生するのはすべてのチャンネルでなく、BSの一部のチャンネルのみ。調べるとBS再編の影響(エンコーダのMPEG2AD化)らしい。ffmpegでhwaccelを使うのをやめると回避できる問題だったので、とりあえず特定のチャンネルの放送を録画したものはhwaccelを使わずにエンコードすることに。

<iframe src="https://mstdn.maud.io/@Otakan951/101317741224656577/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://mstdn.maud.io/embed.js" async="async"></script>

<iframe src="https://mstdn.maud.io/@Otakan951/101317756964583985/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://mstdn.maud.io/embed.js" async="async"></script>

速度は落ちたけど結果は正常なものになりました。~~CPUがCeleronなので、なんとかしたい問題です。~~

なんとかなりました。[録画鯖で発生していたエンコ時のブロックノイズ問題が解決した](/2019/02/28/news/#録画鯖で発生していたエンコ時のブロックノイズ問題が解決した)