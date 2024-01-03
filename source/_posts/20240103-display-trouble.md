---
title: ディスプレイを買い替えたら色々な問題が起きた話
date: 2024-01-03 04:42:26
tags:
    - Windows
    - ディスプレイ
    - PC
---

この記事はmstdn.maud .io Advent Calendar 20yyのdd日目の記事ではありません。
3年ぶりのアドベントカレンダーと関係ない更新です。
ディスプレイをMSI MAG 323UPFへ買い替えたところ、色々とめんどくさいことになったのでまとめておきます。

<!-- more -->

# はじめに
## PC構成について
最初に、使っているPCの構成について書いておきます。

|||
|:-:|:-:|
|CPU|AMD Ryzen 9 5950X|
|メモリ|DDR4-3200MHz 16GB*4 (CORSAIR DOMINATOR PLATINUM)|
|GPU|ASUS ROG-STRIX-RTX3090-O24G-GAMING|
|マザーボード|ASUS ROG Crosshair VIII Hero WI-FI|
|電源|1000W (CORSAIR HX1000i)|
|メインディスプレイ|4K 144Hz DP接続 (GIGABYTE M32U)|
|サブディスプレイ1|4K 60Hz DP接続 (Lenovo ThinkVision T27p-10)|
|サブディスプレイ2|4K 60Hz HDMI接続 (Lenovo ThinkVision P27u-10)|
|その他|Valve Index VR HMD|
|マウス/キーボード|G604/G913TKL|
|OS|Windows11|

GPUはライザーケーブル等使用せず、マザーボードに直挿ししています。

## ディスプレイを買い替える
今回、メインディスプレイとして使っているGIGABYTE M32UをMSI MAG 323UPF買い替えることにしました。
性能的には4K 144Hz→4K 160Hz、HDR 400→HDR 600、USB-PD 18W→90Wの向上です。
まぁ現実的に最新ゲームで4K 160Hzを処理できるグラボは無いですし、HDRは使わないし、USB-PDもめったに使わないんですが…
それでもYahoo!ショッピングで20%以上の還元があったり、もうすぐM32Uが保証期限間近だったこともあり、まあ満足しています。

# 最初の問題
M32Uを片付け、323UPFをPCと接続して使い始めたところ、いくつか問題が発生しました。

1. PC起動時のMSI MAG 323UPF復帰が遅い
2. VRを起動するとブルースクリーンになる

## 復帰が遅い
ディスプレイの電源を入れて画面が表示される動作は至って普通なのですが、ディスプレイのスリープ(電源ランプ橙点灯)からの復帰がとても遅いです。
PC起動時は復帰が遅いためにサブディスプレイ側で起動ロゴが表示されたり、Windowsのロック画面まで何も表示されなかったり…
Windowsがスリープから復帰するときもメインディスプレイは認識されずにサブディスプレイにロック画面が表示され、メインディスプレイが認識されるのは1テンポ遅れます。
とくにWindowsのスリープからの復帰時は、すべてのウィンドウがサブディスプレイに移動してしまって面倒。

## VRでブルスク
M32Uのときは一切問題がなかったのですが、VRを起動するとWindowsがブルスクになってしまうようになりました。
ブルスクのエラーはDRIVER OVERRAN STACK BUFFERだったり、Nvidiaドライバー系のなにかだったり(メモる前に消えた)。
場合によってはブルスクさえなく、全ディスプレイが暗転して40秒後勝手に再起動されることも。
そしてブルスクの表示はメインディスプレイではなくサブディスプレイに表示されます。
<iframe src="https://hota.hirachon.otakan.jp/@otakan/111630244044446339/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400" allowfullscreen="allowfullscreen"></iframe><script src="https://hota.hirachon.otakan.jp/embed.js" async="async"></script>

とにかく心臓によくない。

## 対処しようとした
MAG 323UPFの復帰が遅い問題はおそらくディスプレイ自体の問題と判断しました。
このディスプレイは発売が2023年11月30日なため、ファームウェアが未熟なだけかもしれません。
そのうち修正ファームウェアが公開されるだろうと考え、ブルスク問題への対応をすることに。

とりあえず思いつくのは各種アップデートです。
とりあえず、Geforeドライバーが1年以上前のものだったので最新の546.33まで更新することにしました。
DDUを使って古いドライバーを削除してから546.33をインストールしましたが、特に変化はありませんでした。

次にBIOSアップデートをしてみました。
元のバージョンは4006で、これも2年近く前のバージョンです。
更新内容に"Improve system stability"と記載されていたので、最新の4702までバージョンアップをしました。

# 問題が増えた
1. PC起動時のMSI MAG 323UPF復帰が遅い
2. VRを起動するとブルースクリーンになる
3. BIOSアップデートしたらPCが起動しなくなった ← New!

## PCが起動しない
BIOSを4702までアップデートしたら、PCが起動しなくなりました。
使用しているマザーボードには[Q-Code](https://www.asus.com/jp/support/FAQ/1043948/)と呼ばれる起動時の状態を判断するための機能がありますが、このQ-Codeで「98」と表示されて処理が動かない状態でした。

Q-Code 98で調べてみると、CMOSバッテリーが寿命とか、USBデバイスが悪いとか、マウス/キーボードが原因だという書き込みが見つかります。
CMOSバッテリー切れは違う気がしたので、とりあえずキーボード以外のUSBデバイスを可能な限り外してみましたが改善せず、キーボードも含めて外してみると起動するようになりました。
ちなみにCMOSクリアも試しましたが効果はありませんでした。

## 暫定対処
BIOSのPOST時にキーボードが接続されていると起動処理が止まってしまうことが分かったので、起動時だけキーボードを外せばWindowsは起動します。
しかし、これからPCの起動時に毎回キーボードを外す運用なんてしたくないですね。
幸い、BIOSを4006までバージョンダウンしたところ正常に起動するようになりました。

# 問題が変わった
1. PC起動時にMSI MAG 323UPFの復帰が遅い
2. VRを起動しようとしてもHMDに接続できない ← Updated!
3. BIOSアップデートしたらPCが起動しなくなった

## HMDに接続できない
ひとまずBIOS 4006の状態で使用していたのですが、試しにSteamVRを起動したところHMDと接続できなくなってしまいました。

<iframe src="https://hota.hirachon.otakan.jp/@otakan/111657099656656581/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400" allowfullscreen="allowfullscreen"></iframe><script src="https://hota.hirachon.otakan.jp/embed.js" async="async"></script>

エラー表示としてはHMDのケーブルが断線したときと同じものになります。
Valve indexのケーブルはとにかく断線しやすいと評判(?)で、私が使っているものも一部被覆が割れています。
BIOSアップデートだのUSBデバイス全部外すだのやっていたため、思い返せば断線してもおかしくなったのかもしれません。

問題は使っているIndexの保証がとっくに切れていて、交換対応等ができないこと。
[Amazon](https://www.amazon.co.jp/dp/B081M7WMTD)や[Tundra Labs](https://tundra-labs.com/en-jp/products/valve-index?variant=41527613882577)や[ifixit](https://www.ifixit.com/products/valve-index-tether-and-trident-cables)にて交換品の販売がありますが、とにかく良いお値段がします。
なるべく手を出したくない…

## 原因
なんとかケーブルを買わずに済む方法はないかと全力で試行錯誤を始めました。
なにせ数日前まで使えていたわけですから断線だとしてもまだ軽度なものだろうと判断し、index側のケーブル抜き外し、コネクタ清掃、ケーブル捻じれ解消等を行いましたが、特に変化はありませんでした。

保証交換をValveへ申請するといくつか確認事項や設定変更の案内をされるとの情報があったので、そこで案内される確認事項もすべて試しましたが、効果はありませんでした…

これは流石にどうしようにも無いかと諦めかけていたとき、あることに気が付きます。
SteamVR > 開発者 > 開発者設定と画面を開くと、「ダイレクトディスプレイモード」という項目があります。
これはHMDを一般的なディスプレイとしてWindowsに認識させるかどうかの設定で、通常は有効(Windowsに認識させない)になっていると思います。
この設定を無効化、つまりディスプレイとして認識させるように設定変更したところ、あっさりと認識しました。
{% asset_img steamvr.jpg%}

更に、Nvidiaコントロールパネルでは、各ディスプレイ毎に有効/無効を切り替える事ができるのですが、こちらで操作したところIndex + 他2つのディスプレイという組み合わせであれば正常に有効化することも確認。
この状態であればVRも正常に起動しました！
<iframe src="https://hota.hirachon.otakan.jp/@otakan/111658999202109025/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400" allowfullscreen="allowfullscreen"></iframe><script src="https://hota.hirachon.otakan.jp/embed.js" async="async"></script>

つまり、ケーブルが断線したわけではなく、PCがディスプレイ*3 + Indexの計4出力をすることができない状態だったということです。

## 暫定対処
そもそもRTX 3090は最大4画面までの出力に対応しています。
実際、ディスプレイを買い替える前はディスプレイ3枚+Indexの計4画面出力ができていました。
これはボードメーカーやグレードに関係しないため、[AORUS GeForce RTX™ 3090 XTREME 24G](https://www.gigabyte.com/jp/Graphics-Card/GV-N3090AORUS-X-24GD#kf)のようなDP\*3+HDMI\*3の計6ポートを載せているカードでも、同時に出力できるディスプレイ数は4つまでとなります。

とりあえず、ディスプレイを減らせばIndexが正常に機能することが確認できたことから、VR時は表示ディスプレイを「PC画面のみ」(メインモニタのみ)にすることで対応することにしました
<iframe src="https://hota.hirachon.otakan.jp/@otakan/111661824619043684/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400" allowfullscreen="allowfullscreen"></iframe><script src="https://hota.hirachon.otakan.jp/embed.js" async="async"></script>

ノートPCにプロジェクター等を繋いだときに表示を切り替えるための機能ですが、Nvidiaコントロール上でメインモニタ以外を無効化した時と同様の状態になるっぽいです。
デスクトップPCで使う機会があるとは思わなかったよ。

ただし、VR開始前に「PC画面のみ」へ切り替えるのは上手くいくものの、VR後に「拡張」へ戻す動作が不安定で、思うように動かないことも…

# 問題をまとめてみる
ごちゃごちゃ書いててわからなくなってくるので、一度まとめます。

1. PC起動時のMSI MAG 323UPF復帰が遅い
    * 原因は不明で、未対処
2. 4画面同時出力ができない
    * ディスプレイ買い替える前は問題なかった
    * VR時のみサブディスプレイを無効化することで対処
3. BIOSアップデートしたらPCが起動しなくなった
    * Q-Code 98でPOSTしない
    * キーボードを外すと起動する
    * BIOSを4002にバージョンダウンして対処

なんでディスプレイ買い替えただけでここまでの問題を引くねん…

# 対処
## グラフィックボードのバージョンアップ
色々と検索していたところ、[価格.comの書き込み](https://bbs.kakaku.com/bbs/K0001428351/SortID=24782414/#24853908)から[【RTX3080Ti・RTX3060要注意】PC起動後にDP接続のモニターが映らない不具合について](https://pc-gamers.info/displayid-vbios/)にたどり着きました。
ディスプレイとGPUとの間で交わされるDisplayID情報の扱いについてGPU側に不具合があり、OSが起動するまで正常に表示されない…とのこと。
ディスプレイの復帰が遅い問題これじゃね？

Nvidiaが修正のためのアップデートツールを提供しているので、[NVIDIA GPU Firmware Update Tool for DisplayID](https://nvidia.custhelp.com/app/answers/detail/a_id/5233/~/nvidia-gpu-firmware-update-tool-for-displayid)からダウンロードします。
画面の指示に従ってアップデートと再起動したところ、正常にディスプレイ出力がされるようになりました！
MAG 323UPFくんは何も悪くなかったんやね、疑って悪かった。

ついでに調べたところ、ROG-STRIX-RTX3090-O24G-GAMINGには複数のvBIOSアップデートが[サポートページ](https://rog.asus.com/jp/graphics-cards/graphics-cards/rog-strix/rog-strix-rtx3090-o24g-gaming-model/helpdesk_bios/)で公開されていたので、これについても全部適用することにしました。
vBIOSはバージョン1～5まで公開されていますが、いま現状のバージョンがどれにあたるのかわかりません。
vBIOSのバージョン自体はNvidiaコントロールパネル > システム情報から「ビデオBIOSのバージョン」という項目で確認できますが、当然ながらバージョン1なんて単純な数字ではありません。

私はよく分からなかったのでバージョン1から順に3までアップデートしました。
v2→v3適用はResizable BARが有効化されるものでvBIOSの値は変わらず、最終なバージョンは94.02.42.00.A9になりました。
ちなみにv4以降は「必要がなければ適用する必要はない」と書かれていますが、必要かどうか(前バージョンのインストールに失敗したかどうか)はアップデータが判断してくれるらしいです。友人のMくんが言っていました。
{% asset_img nsysinfo.jpg%}

起動時にBIOSがメインディスプレイへ正常に映るようになったついでに、もう一度BIOSを4702へアップデートしてみたところ、何事もなかったかのように起動してきました。
Q-Code 98問題もGPUが原因だったっぽいです…

## 私は5画面出力しようとしていた
DisplayPortには様々なバージョンがありますが、元々使用していたM32Uでは[DisplayPort1.4](https://vesa.org/featured-articles/vesa-publishes-displayport-standard-version-1-4/)が搭載されていました。
DP1.4は25.92Gbpsまでのデータ転送に対応しているため、4K 120Hz SDR(25.82Gbps)が実質的な最大解像度/リフレッシュレートになりますが、Display Stream Compressionを使用することで4K 144Hz SDR(31.35Gbps)にも対応します。
M32UではDSCを使用することで4K 144Hz SDRの表示を実現していたわけですね。
買い替えた323UPFではDP1.4aが搭載されていますが、[1.4と1.4aに大きな差は無い](https://vesa.org/vesa-display-compression-codecs/dsc/)らしいです。
つまり323UPFでもM32Uと同様に、DSCによって4K 160Hz SDR(35.1Gbps)の表示を実現します。

ここでNVIDIAの資料を漁っていたところ、[Using high resolution/refresh rate displays with VESA Display Stream Compression (DSC) support on NVIDIA GeForce GPUs](https://nvidia.custhelp.com/app/answers/detail/a_id/5338/)という長ったらしいタイトルのドキュメントを見つけました。

> When a display is connected to the GPU and is set to DSC mode, the GPU may use two internal heads to drive the display when the pixel rate needed to drive the display mode exceeds the GPU’s single head limit. This may affect your display topology when using multiple monitors.  For example if two displays with support for DSC are connected to a single GeForce GPU, all 4 internal heads will be utilized and you will not be able to use a third monitor with the GPU at the same time.

つまり、通常は1ディスプレイに1つ割り当てられる内部的な出力系統が、DSC使用時は1ディスプレイに2つ使用される場合がある。この場合、GPU的には4系統の出力処理を行っているので、4画面出力と同等の状態になる。結果的に、見かけ上は3画面までしか出力できなくなる…らしいです。
今回4K 160Hzになった結果、この2系統を使う状態になってしまったためにメインディスプレイとサブディスプレイの3台で4系統を使い切ってしまったということですね。

解決策としては2系統使わない状態にすれば良いわけなのですが、私の環境では4K 60Hzに設定変更しても4画面出力はできませんでした。
ディスプレイによっては使用するDisplayPortのバージョンが選択可能で、DisplayPort1.3(DSC未対応)へ変更することで回避する方法もあるそうですが、323UPFにそういった設定項目は見当たりませんでした。

こうなってくると残された解決策は別のGPUを用意するしかありません…
残念ながら私の使用しているRyzen 5950XはiGPUが存在しないので、使用するグラボを追加することになります

<iframe src="https://hota.hirachon.otakan.jp/@otakan/111667515596813996/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400" allowfullscreen="allowfullscreen"></iframe><script src="https://hota.hirachon.otakan.jp/embed.js" async="async"></script>

幸いなことに、以前 [ひらちょんから貰ったGTX 1050Ti](https://hinabita.com/2017/08/26/compaq-8200-sff%e3%81%abgtx1050ti%e3%82%92%e8%bc%89%e3%81%9b%e3%81%a6%e3%81%bf%e3%81%be%e3%81%97%e3%81%9f%e3%80%82/)が手元にあったのでこれを使って対処しました。
1050Tiに取り付けられていた2連ファンはクソうるさかったので、取り外してファンレス状態で頑張ってもらっています。

## その後
1050Tiで問題なく使用できていたのですが、ゲーム中に瞬間的なFPS低下が発生したりと微妙に安定しなかったのでDDUを使用してドライバーを再インストールしました。
これで改善されましたが、まだパフォーマンスが落ちている感じがしたのでNvidiaコントロールパネルから「スケーリングを実行するデバイス」を「GPU」に変更したら安定しました。
{% asset_img ncp.jpg%}

ゲーム中に画面が一瞬だけ暗転する事があるけど、それ以外は安定しています。助けてくれ
ちなみにWindowsがスリープから復帰するときだけ、323UPFの復帰が遅い問題が毎回発生します。助けてくれ

# おまけ
## 時系列について
ブログに纏めるために時系列が前後したり省いた部分がありますが、実際はこんな流れでした

1. ディスプレイを323UPFに交換する
2. 復帰遅延問題とVR時ブルスク発生問題が起きる
3. メインディスプレイを外してVRする←？？？？
4. DDUでのドライバー再インストールをする
5. BIOSアプデをして起動しなくなったので切り戻す
6. BIOSのアプデ手順や設定変更で試行錯誤するけど結局切り戻す
7. GPUのファームウェアとvBIOSアップデートを行う
8. Valve indexが断線のような状態になり、試行錯誤する
9. ダイレクトディスプレイモードに気が付く
10. DSCの仕様を知る
11. 1050Tiを使う&DDUで再インストールする

特にIndexの断線問題がvBIOSのアップデート後に発生していたので、余計に混乱していた覚えがあります。
本当にカス。

## GPUのDual BIOS機能について
ROG-STRIX-RTX3090-O24G-GAMINGにはDual BIOSという機能が搭載されています。
グラフィックボードの側面に搭載されたスイッチを切り替えることで、パフォーマンス優先のP-Modeと静音性優先のQ-Modeに切り替えることができる機能ですが、名前の通りこれは使用するvBIOS自体を切り替えるものになります。
今回DisplayIDのファームウェアアップデートやvBIOSのアップデートを行いましたが、これはP-ModeとQ-Modeそれぞれでアップデートを行う必要があります。
私は常にP-Mode状態で使っていて、Q-Modeの方は未だにアップデートを行っていないので、うっかりQ-Mode状態で起動しようとするとQ-Code 98問題が再発します。
vBIOSをアップデートするときは両方のModeをアップデートするようにしようね。

## GPUの仕様について
今回色々と調べているときに知ったのですが、GPUの仕様に記載されているDigital Max Resolutionは各ディスプレイの出力解像度の合計値ではなく、各ディスプレイ1台毎の出力解像度で、「出力可能な最大解像度」ではなく「動作確認済み最大解像度」だそうです。
つまり、帯域幅が許す限り(=リフレッシュレートを犠牲にすれば)出力解像度は大きくできるそうです。

# 最後に
MSI MAG 323UPFを購入して学んだ教訓があります。
1. めんどくさい構成が好きなオタクはiGPUが載った構成で組もう
2. 4K 160Hzなんて出力できるグラボは無いんだから、そんなディスプレイは選ばないほうが良い
3. アップデート情報は適時確認しよう

年末年始の休みだったから色々調べる時間があったけど、普通こんな問題はわからないよ…

参考
* DisplayPortについて
[DisplayPort - Wikipedia](https://ja.wikipedia.org/wiki/DisplayPort)
[What is DisplayPort 1.4?](https://www.sabrepc.com/blog/Product-Review/displayport-1-4)
[VESA Publishes DisplayPort™ Standard Version 1.4 - VESA - Interface Standards for The Display Industry](https://vesa.org/featured-articles/vesa-publishes-displayport-standard-version-1-4/)
[VESA Display Compression Codecs - VESA - Interface Standards for The Display Industry](https://vesa.org/vesa-display-compression-codecs/)
[Video Timings Calculator](https://tomverbeure.github.io/video_timings_calculator)

* DisplayID不具合について
[価格.com - 『デュアルディスプレイ環境でBIOS画面が表示されない』 MSI PRO Z690-P DDR4 のクチコミ掲示板](https://bbs.kakaku.com/bbs/K0001428351/SortID=24782414/#24853908)
[NVIDIA，DisplayPort 1.4＆1.3接続時に画面が表示されなかったりする問題へ対策するファームウェアを公開](https://www.4gamer.net/games/251/G025177/20180612034/)
[【RTX3080Ti・RTX3060要注意】PC起動後にDP接続のモニターが映らない不具合について | PCゲーマーの備忘録](https://pc-gamers.info/displayid-vbios/)

* 画面同時出力について
[4 displays vs RTX 4090 - Displays - Linus Tech Tips](https://linustechtips.com/topic/1535051-4-displays-vs-rtx-4090/)
[Why am i loosing my 4th monitor to DSC? - Displays - Linus Tech Tips](https://linustechtips.com/topic/1509589-why-am-i-loosing-my-4th-monitor-to-dsc/)
[Question - Triple Samsung LS28BG702EPXEN monitors are stuck on 120Hz ? | Tom's Hardware Forum](https://forums.tomshardware.com/threads/triple-samsung-ls28bg702epxen-monitors-are-stuck-on-120hz.3819333/)
[What is the maximum amount of monitors you can have connected? Does it depend on their resolution? · NVIDIA/open-gpu-kernel-modules · Discussion #396](https://github.com/NVIDIA/open-gpu-kernel-modules/discussions/396#discussioncomment-3937173)
[Question - Can't connect 4 monitors to a 3090Ti GPU | Tom's Hardware Forum](https://forums.tomshardware.com/threads/cant-connect-4-monitors-to-a-3090ti-gpu.3814708/#post-23056965)
[Using high resolution/refresh rate displays with VESA Display Stream Compression (DSC) support on NVIDIA GeForce GPUs | NVIDIA](https://nvidia.custhelp.com/app/answers/detail/a_id/5338/kw/Display%20Stream%20Compression)
