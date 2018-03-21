---
title: Cloud at CostでファイルシステムがRead Onlyになってしまった
date: 2018-03-21 15:41:05
tags:
 - Cloud at Cost
 - Server
 - Ubuntu
---

# CaCのサーバーがRead Onlyになった
タイトルそのままです。突然SavaMoniからアラートが飛んできたので確認したら、Mastodonが動作しているサーバーが書き込み不可能になっていました。調べてみると一台の物理サーバーで動作している仮想サーバーが多すぎるためにI/Oがタイムアウトし、Read Onlyになってしまうんだとか。「何度も再起動すれば直るかも」と書いてあったので再起動を掛けましたが直る気配は無し。仕方がないのでCaCのサポートに連絡をしてみました。
<iframe src="https://mstdn.maud.io/@Otakan951/99704072672203087/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://mstdn.maud.io/embed.js" async="async"></script>

# 方法１ (試せてない)
サポートから返ってきた対処法その１です。前述の通り再起動を掛けたところ、Ubuntuが起動してくれない状態になっていたため、この方法は試していません。
次のコマンドをシングルユーザーモードで順に実行します。
```
mount -o remount, rw /
lvm vgscan
lvm vgchange -a y
mount -t ext4
/dev/primary_vg/root_lv /root
exit
```

# 方法2
サポートから返ってきた対処法その2です。これでうまくいかない場合はサーバーを作り直せと言われました。
- Console右上のボタン(Send Ctrl-Alt-Del)をクリックしてサーバーを再起動します。
- GRUBメニューが表示されたらEキーを押してGRUB設定ファイルを開きます。
- ファイルの下の方に`linux /vmlinuz-3.16.0-4-amd64 root=/dev/mapper/localhost-vg-root ro quiet`といった記述があることを確認してください。
- `linux /vmlinuz-3.16.0-4-amd64 root=/dev/mapper/localhost-vg-root ro splash rootdelay=120`に変更します。(変わったのは末尾のみです)
- F10を押すと、この変更を適用して再起動がされます。(この変更は一度のみです)

# 方法3
結局この方法で直しました。
上にあるMastodonの投稿に添付されている画像の画面の状態で`fsck /dev/sda1`と入力して実行すると
```
group descriptor checksums are invalid. Fix<y>?
Pass 1: ...
Block bitmap differences : ...
Fix<y>?
```
といった具合で各項目を修復するか聞かれるのですべてyを選びます。終了したら`exit`で再起動します。

# 最後に
- 方法2で"="が入力できない場合はキーボードをUSにしてください。
- 方法3はyオプションをつけることで毎回yを押さずとも自動的にすべて修復していきます。

方法１は試せていませんが、方法2では直すことができませんでした。CaCが用意したUbuntuテンプレートではなく、[Cloud at Costで追加料金を払わずにUbuntu16.04をインストールする](/2017/11/22/cac-ubuntu1604-install/)の方法でインストールしたUbuntuを使用していたためなのかは不明。
