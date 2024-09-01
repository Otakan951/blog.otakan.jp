---
title: Valve IndexからMeta Quest3に移行した話
date: 2024-08-11 11:59:50
tags:
---
Lighthouse環境を卒業できない俺達は。
<!-- more -->

期待していた[Nofio](https://nofio.co/products/nofio-wireless-adapter-for-valve-index)がなんとも言えない感じになってしまい、Valve Index自体の入手性も悪くなってきたこの頃、[Meta Quest3のセール情報](https://x.com/MetaQuestJapan/status/1813015704238473581)が流れてきたので勢いのまま購入しました。
Quest3を使うにあたって、色々と調べたり試したりしたことをまとめます。

今まではIndexとVIVEトラッカーと組み合わせたフルトラ環境でVRをプレイしていましたが、inside-out方式のQuest3は本来Lighthouse方式の機器と互換性がなく、VIVEトラッカーが使えなくなってしまいます。
とはいえ、先人たちの知見と様々な素晴らしいソフトウェアにより、Quest3とVIVEトラッカーを組み合わせることは不可能ではありません。
Quest3でフルトラを行う場合、HaritoraXやVIVE Ultimate Trackerと組み合わせるのが正解かと思いますが、Lighthouse環境が既に存在するのであればLighthouse機器と組み合わせたほうが安上がりですので、Quest3とVIVEトラッカーを組み合わせたフルトラ環境構築を目指します。

※IndexフルキットなどのLighthouse環境を持っていない場合、このページの内容はおそらく意味がありません。今からLighthouse環境を新規に構築することは、入手性的にもコスト的にもおすすめしません。

## 組み合わせについて
* HMD: Meta Quest3
    今回の主役。PCとは無線で接続します。
    なお、Index本体には重要な任務が残っているので、PCと接続したままにします。
* コントローラー: Valve Index コントローラー
    Quest3付属のQuest Touch Plusコントローラーは使用しません。Indexコントローラーのほうが高性能ですし、Indexコントローラーを使うからと言って手順が難しくなることもありません。
    Quest Touch Plusコントローラーは一切使わないので箱に片付けてあります。Quest自体の操作はハンドトラッキングで十分ですし、2つのコントローラーを持ち替えるのは面倒なので…
* トラッカー: VIVE トラッカー
    フルトラ用にVIVEトラッカー 2.0を3つ持っているので、それをそのまま使用します。
    なお、後述する頭キャリブレーション用として、追加で一つ必要になります。私はVIVEトラッカー 3.0を追加購入しましたが、Quest3と一緒に頭に載せることになるため、少しでも軽量なTundra Trackerのほうが適しているかもしれません。
    追加トラッカーを必要としない方法(8の字キャリブレーション)もありますが、VRを起動するたびに手動でキャリブレーションを行う必要がある上に、プレイ中も少しずつズレていくのでおすすめしません。
* ベースステーション: Valve Indexベースステーション
    特に触っていません。そのまま使います。

## Index本体について
VIVEトラッカーがPCとの接続にUSBドングルを必要とするのと同じように、Indexコントローラーもドングルを必要とします。そして、Index本体はこのコントローラー用ドングルを2個内蔵しています。
このためIndexはHMDとしては使用しないものの、コントローラー用のドングルとして引き続き使用します。
もしIndexを片付けたい場合、別途2個分のドングルを用意する必要があります。入手性を考えると、Shiftallの[X2 ドングル](https://shop.shiftall.net/products/x2-dongle)や、IntoFreeで販売されている[SteamVRドングル](https://market.intofree.world/products/steamvr-dongle)あたりになるでしょうか。

このあたりの説明は[Watchman Dongle についての基本知識](https://5er.jp/20240114-watchman-dongle/)が分かりやすいかと思います。

## PCとQuest3の接続
PCとQuest3の無線接続方法にはAir LinkやSteam Linkなどありますが、今回はVirtual Desktopを使用します。
選んだ理由はQuest1のときにVirtual Desktopで安定してプレイできていたからですが、今でもこれが最適解なんじゃないかと思います。

### Virtual Desktopのインストール
詳しいセットアップ方法は色々なサイトで説明されているので割愛しますが、基本的に4ステップで完了します。
1. [公式サイト](https://www.vrdesktop.net/)からStreamer AppをダウンロードしてPCにインストール
2. PCでStreamer Appを起動してMetaアカウントでログイン
3. [Meta Store](https://www.meta.com/ja-jp/experiences/2017050365004772/)で購入したクライアントをQuest3にインストール
4. Quest3でVirtual Desktopアプリを起動してPCに接続

PCとの接続後、Quest3のVirtual Desktopで以下のような表示がされていれば、PCと正常に接続ができています。
{% asset_img vd-start.jpg%}

この状態で「Launch SteamVR」を選択するとSteamVRが起動しますが、Quest3にSteamVRの画面は表示されません。実はIndexがPCに接続されている状態では、SteamVRはIndexの方に表示されてしまいます。
前述の通りIndexにはドングル以上の仕事を求めないので、IndexのDisplayPortケーブルを抜いてしまいましょう。Indexには電源とUSBケーブルが接続されていれば、十分仕事してくれます。
※ベースステーションが自動起動しなくなった場合は[OVRLighthouseManager](#OVRLighthouseManager)を参照してください。

もう一度起動し直すと、Quest3にSteamVRの画面が表示されるかと思います。もし表示されない場合、SteamVRアドオン管理で「Virtual Desktop Streamer (Quest)」が有効になっているか確認すると良いかもしれません。
{% asset_img steamSettings-VD.png%}

※現時点ではVIVEトラッカーはもちろん、Indexコントローラーは位置が激しくずれるため使用することができません。

### Virtual Desktopの設定
#### コントローラー
今の状態では、SteamVRにIndexコントローラーとQuestコントローラーの両方が認識される状態になるため、色々と面倒くさいです。
幸いにもVirtual DesktopにはQuestコントローラーをSteamVRから切断する設定が存在するため、こちらを有効化します。これにより、Quest自体とVirtual Desktopの操作にはQuestコントローラー、SteamVR上の操作はIndexコントローラーと、完全に分けることができます。
私はQuestコントローラーはハンドトラッキングで使用しているため、Indexコントローラーを手放すだけでQuest側を操作できます。逆も同様ですね。

設定は簡単で、Virtual DesktopのStreaming設定画面で「Track controllers」を無効化するだけです。
{% asset_img vd-streaming.png%}

#### その他
コントローラーのついでに、他の設定も調整すると良いかもしれません。
検索すると各設定の解説をしてくれているサイトはたくさん出てきます。環境によって最適値は異なるのでここでは省きますが、とりあえず以下2つは設定を変更しておくと良いと思います。

1. Use optimal resolution: 無効化
    Virtual Desktop内で表示される、PCのデスクトップ画面の解像度を自動調整する設定です。とくに4Kモニターなどを使っている場合は、Virtual Desktopを起動するたびに画面解像度が変更されてPCに余計な処理を発生させるので、無効のほうが良いかと思います。
2. Synchronous Spacewarp (SSW): 無効化
    フレームレートが表示レート(VR Frame Rate)に到達しなかった場合に、補完を入れることでカクツキを低減する機能ですが、逆に画面がぐにゃぐにゃとして酔いの原因になる場合があります。VRタイトルにもよりますが、常時VR Frame Rateを下回りがちな環境であれば無効化したほうが良いと思います。

{% asset_img vd-settings.png%}

[A Virtual Boost in VR Rendering Performance with Synchronous Space Warp](https://www.qualcomm.com/developer/blog/2022/09/virtual-boost-vr-rendering-performance-synchronous-space-warp)

## LighthouseとInside-out間のキャリブレーション
最初に書いた通り、inside-out方式のQuest3は本来Lighthouse方式の機器と互換性がありません。
この穴を埋めるために2方式の間で位置情報を同期するキャリブレーションを行いますが、そのために必要なソフトウェアがOpenVR-SpaceCalibratorです。
[いくつかのFork](https://www.reddit.com/r/MixedVR/comments/19ay1bp/which_version_of_openvr_space_calibrator_should/)があるようですが、今は[hyblocker/OpenVR-SpaceCalibrator](https://github.com/hyblocker/OpenVR-SpaceCalibrator/releases/latest)を使えばいいみたいです。

ダウンロードしたOpenVR-SpaceCalibrator.exeをインストールし、SteamVRのスタートアップとアドオン管理から「SpaceCalibrator」を有効化しましょう。
{% asset_img steamSettings-SC.png%}

なお、サイトによってはsteamvr.vrsettingsのファイルを手動で書き換える必要があるとの説明がありますが、私が試した限りでは書き換えることなく使えました。

### キャリブレーション用トラッカーの準備
トラッカーのSteamVRセットアップは[公式のマニュアル](https://www.vive.com/us/support/tracker3/category_howto/pairing-vive-tracker.html)に従います。

キャリブレーションを行うために頭用トラッカーを識別する必要がありますので、トラッカー、ペアリングしているドングル、ドングルのID(LHR-から始まる文字列)の3つを紐づけておきましょう。
ドングルのIDは「SteamVR設定」→「コントローラー」→「トラッカーの管理」から確認できます
{% asset_img steamSettings-tracker.png%}

私はトラッカーとドングルの両方に、記号を書いたマスキングテープを貼って判別できるようにしています。
余談ですが、マスキングテープはアクリル系の接着剤になっているものをおすすめします。ゴム系ではベタつくので。

頭用トラッカーはQuest3に固定する必要があります。Tundra Trackerであれば、ベースプレートでQuest3のバンドを挟むことで固定できるそうですが、VIVEトラッカーではできません。
見た目を気にしないのであれば、こんな感じに無理やりの固定でも問題なく使用できます。
<iframe src="https://hota.hirachon.otakan.jp/@otakan/112904192143039822/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400" allowfullscreen="allowfullscreen"></iframe><script src="https://hota.hirachon.otakan.jp/embed.js" async="async"></script>

私は最終的に[専用の固定具](https://booth.pm/ja/items/5241079)に落ち着きました。トラッカーだけ簡単に取り外せるので、充電時に便利です。
バックルにはA,Bの2種類がありますが、ネジ穴と固定ピン用のくぼみの位置関係が違うみたいです。AであればVIVEトラッカーの充電端子が上を向き、Bであれば下を向くはずです。
<iframe src="https://hota.hirachon.otakan.jp/@otakan/112914959116094214/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400" allowfullscreen="allowfullscreen"></iframe><script src="https://hota.hirachon.otakan.jp/embed.js" async="async"></script>

### キャリブレーション設定
Virtual DesktopからSteamVRを起動し、頭用トラッカーの電源も入れましょう。
PCのデスクトップにOpenVR-SpaceCalibratorが表示されるので、以下のように設定します。
1. Reference SpaceからQuest3のHMDを選択
2. Target Spaceから頭用トラッカーを選択(LHRのIDで判断)
3. Start Calibrationを実行し、キャリブレーションの完了(Finished calibration, profile saved)を確認
4. Continuous Calibrationで自動キャリブレーションを有効化
5. 両方ともにStatusがOKになっているか確認

{% asset_img sc-settings.png%}

SteamVRの再起動後に、自動的にキャリブレーションが動作すれば成功です。
頭用トラッカーの位置によっては視界に入って邪魔ですので、「Hide tracker」を有効化して頭用トラッカーは非表示にすることをおすすめします。

## その他
ここまでやれば、Lighthouse環境下のQuest3でVRができるようになるかと思いますが、快適プレイのためにやれることはまだあります。

### OVRLighthouseManager
HMDにIndexを使用していたときは、SteamVRでベースステーションの電源管理が可能でした。
SteamVRの起動とともにベースステーションも自動起動し、SteamVRを終了するとベースステーションも同時にスリープ(or スタンバイ)に移行するわけです。

残念なことにHMDをQuest3に切り替えると、このSteamVRでの電源管理が不可能になります。これに対処できるのが[OVR Lighthouse Manager](https://booth.pm/ja/items/5315515)です。
OVR Lighthouse Managerを使用することで、Quest3でもSteamVRの状態に合わせてベースステーションの状態を管理してくれるようになります。
なお、PCがBluetooth 4.0以上に対応している必要があります。

OVR Lighthouse Managerのインストール後、SteamVRのスタートアップ設定からOVR Lighthouse Managerが有効化されていることを確認しましょう。
{% asset_img steamSettings-LM.png%}

次に、スタートメニューからOVR Lighthouse Managerを起動し、以下設定を行います。
1. `ベースステーションの電源を管理する`を有効化
2. ダウン時の状態を`スタンバイ`か`スリープ`から選択
3. 電源管理するベースステーションについて、チェックボックスを入れる
{% asset_img OVRLM.png%}

この状態でSteamVRを起動すると、OVR Lighthouse Managerも一緒に起動してベースステーションを復帰させてくれるようになります。

余談ですが、ベースステーションの電源操作にはAndroidアプリの[Lighthouse Power Management](https://play.google.com/store/apps/details?id=com.jeroen1602.lighthouse_pm)でも可能です。1台だけ状態遷移が成功しなかった場合などは、こちらのアプリで操作するほうが便利かもしれません。

### ヘッドストラップ
どのHMDにも言われていることですが、重量が前方に寄っているため、VRをプレイしていると徐々に首が疲れてしまいます。フロント部分が比較的薄く軽いQuest3でも、デフォルトの状態では装着感に改善の余地があり、その対策として有効なのがヘッドストラップの変更です。
装着感の向上のほか、追加バッテリー機能によって長時間のVRプレイを可能にしたものもあります。ヘッドストラップはMeta公式のほかサードパーティからも発売されており、それなりに選択肢があります。

公式の[Meta Quest 3 Eliteストラップ バッテリー付き](https://www.meta.com/jp/quest/accessories/quest-3-elite-strap-battery/)や、価格の安い[KIWI design Battery Head Strap for Quest 3](https://www.amazon.co.jp/dp/B0CMW2WTNJ)、評価の高い[BOBOVR S3 Pro](https://www.amazon.co.jp/dp/B0CSY8PD63)などがありますが、私は[KKCOBVR Q3PRO](https://kkcobvr.com/products/kkcobvr-q3-pro-battery-head-strap-10000mh-compatible-with-meta-oculus-quest-3-extend-2-3h-usage-time-replacement-elite-head-strap-for-oculus-3-by-adjusting-the-side-knobs-1)を購入してみました。価格は$49.99 USDでBOBOVR S3 Proよりは安い…といった感じ。

Amazon.co.jpでは販売されていませんが、公式サイトから送料無料で日本配達が可能です。
8/9に購入したところ、8/12にYanwen Expressで発送され、8/17に日本に到着し、その後佐川急便に引き渡されて8/18日に受け取りました。

{% asset_img kkcobvr-q3pro.jpg%}
総重量は925gほどで、バッテリーを外すと744gでした。
※Indexは公称値750gほどです。
どのヘッドストラップにも言えることだとは思いますが、総重量は重くなるものの前後のバランスは改善されるため、長時間プレイしたときの疲労感はかなり変わってきます。
KKCOBVR Q3PROはBOBOVR S3 Proと同じく額を使ってQuest3を支えるものですので、頬が痛くならないどころか接眼部クッションを外すこともできます。
没入感とのトレードオフだと思いますが、メガネを使う場合などは便利かもしれません。
Quest3の接眼部クッションがとても取り外しにくく、状況によって切り替えることは難しいのが残念…

バッテリーは5V3Aの15W出力が可能で、とりあえず今のところは給電が追いつかないといった問題は発生していません。とは言ってもQuest3の充電能力は18Wですので、このあたりを気にするのであれば20W出力が可能なBOBOVR S3 Proのほうが良いでしょう。

なお、価格が高いとよく言われる公式のEliteストラップ バッテリー付きですが、Quest3本体と連携した[充電制御機能](https://www.meta.com/ja-jp/help/quest/articles/getting-started/getting-started-with-quest-3/elite-strap-with-battery-quest-3/)を搭載しているそうです。バッテリーの寿命を重視するようであれば、こちらも十分選択肢に入るかと思います。
…と書きましたが、KKCOBVR Q3PROもQuest3が90%まで充電された時点で給電を止めていそうな挙動しています。

### 無線LANルータについて
Quest3での無線VRで一番ネックになってくるのが、無線LANルータの性能です。
私の設定では、大体25時間ほどのプレイでこの通信量。
<iframe src="https://hota.hirachon.otakan.jp/@otakan/113050733850849814/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400" allowfullscreen="allowfullscreen"></iframe><script src="https://hota.hirachon.otakan.jp/embed.js" async="async"></script>

PCとQuest3の接続が頻繁に切断される場合は、Quest3専用の無線APを設置すると良いでしょう。実際そういった対応をされている方もいるので、調べてみると環境に合わせた対策が色々と見つかります。
私はひらちょんから頂いた[ASUS RT-AX86](https://www.asus.com/jp/networking-iot-servers/wifi-routers/asus-gaming-routers/rt-ax86u/)を無線LANルータとして使用していますが、Quest3専用の無線APを用意することなくプレイできています。
※戸建ての1階中央にRT-AX86があり、2階にVR部屋がある位置関係です。