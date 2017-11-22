---
title: Cloud at Costで追加料金を払わずにUbuntu16.04をインストールする
date: 2017-11-22 18:24:07
tags: 
 - Cloud at Cost
 - Ubuntu
 - Server
---

# はじめに
一度支払ったらそれ以降は料金が発生しない買い切りを謳い、話題となった激安VPSのCloud at Cost (略してCaC)。突然サーバーが止まったり、データ消されたりといった問題はありますが、覚悟していれば使えないことはないです。
そんなCaCですが選択可能なOSが少なく(ISOからのインストールも公式には非対応)、若干古めなのがちょっと気になりました。$19.99の[Template Pack - OS PRO Pack](https://members.cloudatcost.com/order.php?step=0&productGroup=18)を購入すると使えるOSが増えますが、それだけ払う価値があるのかといわれると・・・
自力でUbuntu 16.04をインストール方法は無いのかと調べたところ、EasyBCDなるものを発見したので試してみました。Ubuntuのインストーラー起動方法とネットワークの設定方法のみを書きます。

# 準備
- Windowsがインストール済みのサーバー
 - Win7ではネットに繋がってくれないことが多かったのでここではWin Server 2012 R2がインストールされているサーバーを使用します。また、Server 2012を使う場合は「ファイルのダウンロード許可」設定やライセンス認証等は済ませておいてください。
- EasyBCD
- Ubuntuインストールイメージ

Ubuntuインストールイメージは[Alternative downloads](https://www.ubuntu.com/download/alternative-downloads)のNetwork installerを使います。バージョン(ここでは16.04 LTS)を選び、"and64 - For 64-bit Intel/AMD(x86_64)"のmini.isoをダウンロードしてください。
EasyBCDは[Neo Smart Technologies](https://neosmart.net/EasyBCD/)のサイトからFree版を。REGISTERと書かれていますが登録せずにダウンロード可能です。

# Windows上での設定

EasyBCDをインストールして起動したら使用言語選択でEnglishを選択し、"EasyBCD Community Edition"のウィンドウが出たらOKをクリックしてください。

{% asset_img easy-bcd.png%}

左のToolboxから"Add New Entry"を選び、"Portable/External Media"のISOタブに移動します。PathにUbuntuのmini.isoを登録し、Modeを"Load from Memory"に変更してください。その後右下の"Add Entry"をクリックし、左下に"Neo Smart ISO Entry added to the boot menu successfully!"と表示されたら成功です。

# Ubuntuのインストール
CaCのServer Consoleを起動した状態でサーバーを再起動すると、VM wareのロゴが出た後にWindows Boot Managerが表示されるのでNeoSmart ISO Entryを選択してください。Ubuntuの"Installer Boot menu"が表示されます。
{% asset_img win-boot-manager.png%}

Installを選択すると言語選択、場所の選択、キーボード設定があります。ネット接続の自動設定が始まりますが、これは失敗するので手動設定をする必要があります。しかし私が試した限りでは"ネットワークを手動で設定"を選択しても設定画面が表示されませんでした。そこでdebconfを変更し、手動設定の画面が出るようにします。

1. 左下の<戻る>を選ぶ
1. "Ubuntu インストーラメインメニュー"が表示されるので"debconf 優先度の変更"を選び、優先度"中"を選択
1. 再びインストーラメインメニューに戻るので"ネットワークの設定"を選ぶ

{% asset_img ubuntu.png%}

ネットワークの自動設定をするか聞かれるので<いいえ>を選択し、手動設定に進みます。IPアドレス、ネットマスク、ゲートウェイはCaCの管理パネルの"Advanced Server Information"に書かれています。ネームサーバーはお好きなものを。ホスト名、ドメイン名を入力したらインストーラメインメニューが表示されるので"debconf 優先度の変更"から優先度を"高"に戻し、"Ubuntu アーカイブのミラーを選択"に進みます。



