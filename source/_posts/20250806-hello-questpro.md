---
title: Meta Quest Proに移行した話
date: 2025-08-06 14:00:42
tags:
    - VR
    - QuestPro
    - PC
---

顔トラでフレンドにあくびを監視してもらおう！
<!-- more -->

[Valve IndexからMeta Quest3に移行した話](/2024/09/01/hello-quest3/)で書いた通り、去年の夏にIndexからQuest3に移行し、その後は何の問題もなく安定してVRを楽しんでいました。
Quest3を購入したときは顔トラは必要ないと思ってQuestProを選ばなかったのですが、周囲にいるフレンドの顔トラを何度も見ていると羨ましくてしょうがない。
Questの次世代機登場にはまだ時間がかかりそうな気配で、顔トラ対応のHMDに少々興味を持っていたところ、今ならまだQuestProが新品で入手可能なことを教えてもらったので購入してしまいました。

今回はQuest3環境からQuestProへ乗り換えるにあたって調べたことをまとます。
なお、Quest3をセットアップするときに設定したことはすべて飛ばします。つまり、このブログ記事はほとんどの内容が顔トラ環境の構築です。
必要に応じて[Valve IndexからMeta Quest3に移行した話](/2024/09/01/hello-quest3/)を見てください。
また、Quest3は手放さないので、3とProが併用できる環境になるように考えて作業しています。(特別なことはほとんどしていませんが…)

細かい設定について色々と教えてくれたフレンドに感謝…！
※Quest Proのスクリーンショットはバージョンがv74.0のものになります。使っているファームウェアバージョンによってQuestのUIが違う可能性があります。


用語の意味がズレそうなので、このページでは
* アイトラ: 視線のトラッキング
* 表情トラッキング: 口元など視線以外のトラッキング
* 顔トラ: アイトラと表情トラッキング両方

に統一して書いています。

## 組み合わせについて

* HMD: Meta Quest Pro
    今回の主役。Quest3と同じく、PCとはVirtual Desktopを使用して無線で接続します。
    なお、Index HMDには引き続きドングルとして頑張ってもらっています。
    
* コントローラー: Valve Index コントローラー
    ここはQuest3セットアップ時と何も変わりません。
    Quest自体の操作はハンドトラッキングを使い、SteamVR上の操作はIndexコントローラーを使います。Quest Touch Proコントローラーは使う機会が一切無く、箱に片付けています。

* トラッカー: VIVE トラッカー3.0 + Tundra Tracker
    今は腰と両足にVIVE トラッカーを使用し、キャリブレーション用の頭トラッカーにTundra Trackerを使用しています。
    Quest Proでもそのまま使用します。

* ベースステーション: Valve Indexベースステーション
    特に触っていません。そのまま使います。

## とりあえずPC VRを動かそう
環境はQuest3を使っていた時点でほぼ整っているため、Quest ProでPC VRをプレイすること自体はそこまで難しくないはずです。
適当にQuest Proの初回セットアップを済ませたら、QuestストアからVirtualDesktopをインストールし、起動します。
Quest3と同じくVirtual Desktopの設定を調整したらSteamVRを起動するだけですね。
トラッカーの固定については[ページ下部](/2025/08/06/hello-questpro/#%E3%83%88%E3%83%A9%E3%83%83%E3%82%AB%E3%83%BC%E5%9B%BA%E5%AE%9A)を見てください。

## 顔トラ機能をセットアップ
VRChatで顔トラ機能を使うには、以下設定が必要です。
1. Quest Proの顔トラ機能を有効化
2. Virtual Desktopの顔トラ転送機能を有効化
3. PCでVRCFaceTrackingをセットアップ
4. VRChatで顔トラ対応アバターを用意
5. VRChatでOSCを有効化

### Quest Proの顔トラ機能を有効化
もしかしたらQuestの初回セットアップ時に説明があって、すでに有効化されているかもしれません…

QuestProの設定アプリから「動きのトラッキング」画面を開きます。
画面最下部にある「アイトラッキング」がアイトラ機能、「自然な表情」が表情トラッキング機能になります。それぞれ有効化しましょう。
アイトラッキングについてはキャリブレーションをすることができます。
キャリブレーション無しでも使えますが、簡単ですのでやっておきましょう。
{% asset_img Quest_1.png%}

### Virtual Desktopの顔トラ転送機能を有効化
VirtualDesktopを起動し、「Streaming」から「Forward tracking data to PC」を有効化します。
PCとの通信量が増える旨の警告が表示されますので、「OK」を選択します。
{% asset_img VD_1.png%}

なお、「Forward tracking data to PC」の下に「Emulate SteamVR Vive trackers」と「Emulate Index controllers」の項目がありますが、顔トラを使うにあたってこちらを選択する必要はありません。

続いてVirtualDesktopがQuestProの「アイトラッキング」と「自然な表情」機能へアクセスするための許可プロンプトが表示されますので、アクセスを許可しましょう。
もし許可プロンプトが表示されない場合は、VirtualDesktopを再起動すると表示されると思います。

### PCでVRCFaceTrackingをセットアップ
VRCFaceTrackingとは、HMDメーカー事に異なる顔トラの情報のやり取りをVRC用に変換してくれる橋渡し的存在…らしいです。
VRCFaceTrackingは最新版がSteamにありますので、Steamからインストールしましょう。
<iframe src="https://store.steampowered.com/widget/3329480/" frameborder="0" width="646" height="190"></iframe>

VRCFaceTrackingをインストールしたら、「Module Registry」からVirtualDesktop用のモジュールを探してインストールしましょう。
{% asset_img VRCFT_1.png%}

モジュールがインストールできたらHomeに戻り、VirtualDesktopのアイコンが表示されていることを確認します

次に、SteamVR起動時にVRCFaceTrackingが自動起動するように設定します。
VRCFaceTrackingの設定を開くと「Auto start」という項目がありますが、グレーアウトしていて選択できません。
{% asset_img VRCFT_2.png%}

実はこれ、SteamVRを起動すると選択可能になります。
というわけで、以下手順で自動起動を設定します。
1. SteamVRを起動
2. VRCFaceTrackingの設定から「Auto start」をオンに変更
3. SteamVR設定の「スタートアップオーバーレイアプリを選択」からVRCFaceTrackingをオンに変更

{% asset_img VRCFT_3.png%}

### VRChatで顔トラ対応アバターを用意
もしも顔トラが動作しなかった場合に原因解明しやすいように、最初はすでに存在しているアバターで動作確認を行ったほうが良いです。
検索すると顔トラに対応しているパブリックアバターがありますので、そちらで動作確認をすると良いでしょう。

自分のアバターを顔トラ対応させたい場合は、Boothなどで顔トラ用ギミックを探しましょう。
[FaceTracking](https://booth.pm/ja/items?adult=include&tags%5B%5D=FaceTracking)などで検索すると出てきます。
同じアバターでも色々な方がギミックを作成されていて、それぞれ味付けが違いますので、動作デモ動画や扱うシェイプキーの数などで選ぶと良いと思います。

なお、大抵の顔トラギミックはアバターオリジナルのFBXに破壊的変更を加えます。プロジェクトをバックアップしておきましょう…
また、ギミックの特性上、元のアバターのバージョンやタイプを厳密に指定されているものもあります。商品の説明書きはよく読んでおくことをおすすめします。

### VRChatでOSCを有効化
VRChatを起動し、Expressionメニューから「オプション」> 「OSC」とすすみ、有効化します。
{% asset_img VRC_1.png%}

このときVRCFaceTrackingを起動していると、VRCFaceTrackingとVRChatとで顔トラデータのやり取りをしている旨の通知が表示されると思います。
{% asset_img VRC_2.png%}

ここまでくれば顔トラが動作するかと思います。

## 顔トラアバターについて


### HMD無しでのテスト
私はQuestProが届くのと同時に顔トラを体験したかったため、QuestProの配送中にアバターの顔トラ対応を進めていました。
他に顔トラ対応のHMDを持っていませんが、単純に顔トラギミックの動作テストをするのであればスマホを使用することで可能です。
Androidスマホなら[MeowFace](https://docs.vrcft.io/docs/hardware/desktop/android/meowface)、iPhoneなら[Unreal Live Link Face](https://docs.vrcft.io/docs/hardware/desktop/iphone/livelink)などがあります。
どちらもVRCFaceTrackingを介してVRChatに情報を送ることに変わりはありません。

私はAndroidスマホを使用しているため、MeowFaceを使います。
VirtualDesktopのときと同じく、VRCFaceTrackingの「Module Registry」からMeowFaceTrackingModuleをインストールします。
なお、VirtualDesktop用モジュールなど他のモジュールがインストールされている場合は、競合を避けるために一旦アンインストールすることをおすすめします。**アンインストールしたことを忘れないでおきましょう。**
モジュールのインストール後、VRCFaceTrackingを再起動してHomeにMeowFaceのアイコンが表示されていることを確認してください。
次にVRCFaceTrackingの「Output」画面からログを表示し、以下の文面を探してください。

> [MeowFaceExtTrackingInterface] Information: Seeking MeowFace connection for 60s. Accepting data on xxx.xxx.xxx.xxx:yyyyy

`xxx.xxx.xxx.xxx`がパソコンのIPアドレス、`yyyyy`が待ち受けポート番号になります。後で使うので覚えておいてください。

MeowFaceのアプリはGoogle Playストアから削除されている様子ですので、[MeowFace by Suvidriel](https://suvidriel.itch.io/meowface)からダウンロードします。
寄付の画面が表示されますが、無料でもダウンロード可能です。画面をよく読んで操作し、ダウンロードしてください。
※Androidでの野良アプリインストール方法についてはここには書きません。

ダウンロードしたアプリをインストールしたら起動しましょう。
「Waiting for connection or Click to enter PC ip」と書かれている場所をタップし、先程確認したパソコンのIPアドレスとポート番号を順に入力します。
{% asset_img MF_1.png%}

PCと接続ができたら、スマホに表示されているカメラに自分の顔を映すことで顔トラが可能です。
使っているスマホによるかもしれませんが、MeowFaceのトラッキング精度はあまり高くありません。
私はMeowFaceで意図した通りにウィンクできたためしがありません。視線はしっかりトラッキングしてくれたので、視線の動きでテストすると良いと思います。

### Unity上でのテスト
VRChatへアバターをアップロードする前に顔トラのテストをしたい場合は、GestureManagerを使用することでUnity上で顔トラの動作確認ができます。
通常通りGestureManagerが有効な状態でPlayモードに入りましょう。
GestureManagerの画面から「Debug」>「Open Sound Control」に進み、「Start on VRChat ports」を選択します。
※「Start on VRChat ports」を実行するとき、VRChatは必ず終了しておいてください。
{% asset_img GM_1.png%}

この状態でVRCFaceTrackingを起動し、顔トラを行ってください。
うまくいえけばの画像の通り「Receuved Osc Packet Messages」に顔トラで収集されたパラメータ値が表示され、UnityのSceneビュー上でアバターの表情が確認できると思います。
{% asset_img GM_2.png%}

### パラメータについて
顔トラギミックは様々なパラメータを使用するため、Expression Parametersをかなり消費する傾向にあります。
具体的な使用数はギミックによりますが、大抵150bit前後を消費する印象です。
この記事を書いている時点でVRChatのExpression Parametersは最大256bitまでと決められているため、使用できる枠のうち半分以上を顔トラに割く必要があるわけです。

もともとギミックなどが無いアバターであればさして問題はないかと思いますが、作り込んであったり、パラメータ数について深く考えていないアバターでは顔トラ対応によって限界値に達してしまい、アップロードできなくなってしまう場合があります。
限界値に達してしまった場合は、必要なパラメータのみに絞ってアバターを最適化することが最も重要ですが、使用するパラメータ数を抑えてくれる非破壊ツールを使用する手もあります。
それがVRC furyに含まれている[Parameter Compressor](https://vrcfury.com/components/other#parameter-compressor-beta)というツールで、Moduler Avater環境下でも動作してくれます。

なお、使われないパラメータを自動的に削除するわけではなく、あくまでも使用量が少なくなるように再構成してくれるツールです。
必要のないパラメータは削除するのが一番ですので、手動でパラメータを最適化したうえで、最後の悪あがき程度に使うことをおすすめします。
また、Moduler Avaterの[Sync Parameter Sequence](https://modular-avatar.nadena.dev/ja/docs/reference/sync-parameter-sequence)など一部互換性が無いコンポーネントもありますので、使用には注意してください。

## FaceEmoとの共存
私は今まで、[FaceEmo](https://suzuryg.github.io/face-emo)で表情管理をしていました。
最初に書いた通り、必要なときにはQuest3に戻れるようにしておきたいため、FeceEmoは残しつつ顔トラと共存できるようにしています。

やり方は簡単で、FaceEmoの設定画面で以下2項目のチェックを外してアバターに適用するだけです。
* ビルド時にまばたきをアニメーションに置き換える(推奨)
* ビルド時に目と口のVRCTrackingControlを無効にする(推奨)

{% asset_img FE_1.png%}

ついでに顔トラ用として、どのハンドジェスチャーでも表情が一切変わらない表情パターンを用意しておくと、顔トラとアニメーション表情が衝突して顔面崩壊する事故が防げます。

## ストラップについて
### KKCOBVR
QuestProはQuest3でいうところの[Eliteストラップ バッテリー付き](https://www.meta.com/jp/quest/accessories/elite-strap-battery/)が標準でついているような状態であるため、標準で前後の重量バランスが良く、長時間使用していても疲れにくいと言われています。
そのためQuest3ほどストラップを使用する必要性は薄いのですが、私はQuest3で使っていた[KKCOBVR Q3PRO](/2024/09/01/hello-quest3/#ヘッドストラップ)を気に入っており、これについていた追加バッテリーをQuestProでも使用したかったため、QuestPro用となる[KKCOBVR O2](https://kkcobvr.com/products/kkcobvr-battery-pack-head-strap-with-10000mah-compatible-with-meta-quest-pro-accessories-charging-dock-increase-the-use-time-by-2-3h-1)を購入しました。
KKCOBVR Q3PROを購入したときは公式ストアから注文しましたが、今回はセールをしていた[AliExpressのストア](https://ja.aliexpress.com/item/1005005757058183.html)から購入しました。
7月3日に注文したところ、7月4日にYFHEX Logisticsから発送され、7月13日に日本に到着して日本郵便に引き渡され、7月15日に受け取りました。

色々なパーツが付くQuest3用と違って、QuestPro用は後頭部パットを純正と入れ替え、額パッドにベルトを通すための小さなパーツを取り付けてからベルトを通すだけです。
そのため、追加バッテリー無しのときの重量バランスは元の状態とほぼかわりません。

肝心のつけ心地ですが、純正状態での重量バランスが比較的良いために追加バッテリーをつけると重量が後方へ傾きバランスが悪くなります。
HMDの締め付けを強めることで無理やり傾きを防ぐことができますが、当然すぐに頭が痛くなりやすいのであまりおすすめしません。
頭頂部のベルト長さを調節して、なるべく頭頂部で支えるようにすると良いと思います。

ちなみに[取り付けマニュアル](https://kkcobvr.com/cdn/shop/files/2_1c7afebe-fb39-4429-8566-75cd37937005.jpg)の手順3にあるケーブルガイド用のバックルですが、付属していません。
不良品かと思いましたが、[付属品一覧](https://kkcobvr.com/cdn/shop/files/6_1ca994b0-b675-4a0c-9e65-eb72a2e2a6e5.jpg)には確かに記載がありません。注意してください。

### Globular Cluster Comfortable Kit V2
人による部分もあると思いますが、後方に重量が傾いているために私はどうしても額が痛くなってしまい、追加で[Globular Cluster Comfortable Kit V2](https://www.globular-cluster.com/QCOQPCF01.html)を購入しました。
パッドのサポート面積が純正のものよりも大きく、素材も柔らかいため、比較的快適な装着が期待できます。
パッドにはベルトを通す用の穴も設けられているため、KKCOBVR O2とも問題なく併用可能です。
公式ストアが日本への発送にも対応しています。
7月23日に注文したところ、7月24日にYunExpressから発送され、7月28日に日本に到着してヤマト運輸に引き渡され、7月30日に受け取りました。

額パッドだけではなく後頭部のパッドや頭頂部ベルトも付属していますが、そちらはKKCOBVRのを使用しているため、私が使っているのは額パッドのみです。
この組み合わせで総重量が953g、バッテリーを外すと772gでした。
{% asset_img Quest_2.jpg%}

Quest3のときより32gほど重い構成になりましたが、今のところはこれで快適に使えています。

## トラッカー固定
[Quest3の記事](/2024/09/01/hello-quest3/#Lighthouse%E3%81%A8Inside-out%E9%96%93%E3%81%AE%E3%82%AD%E3%83%A3%E3%83%AA%E3%83%96%E3%83%AC%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3)に書いてあるとおり、LighthouseとInside-out間のキャリブレーションにはOpenVR-SpaceCalibratorを使用しています。
Quest3では[専用の固定具](https://booth.pm/ja/items/5241079)を使用していましたが、残念ながらこの固定具のQuestPro用は存在しませんでした。
充電時の利便性的にも、Quest3との併用環境にするという目標的にもQuestProに完全に固定されてしまうのは避けたく、簡単に取り外せる取り付け方法を検討していたのですが、最終的にヘッドストラップのベルトにマジックテープで取り付ける方法に落ち着きました。
{% asset_img Quest_3.jpg%}

Tundra trackerにはバックプレートがあるため、トラッカー側のマジックテープはバックプレートでテープを挟み込んで固定しています。
{% asset_img Quest_4.jpg%}

完全な固定とはなりませんが、少なくともTundra trackerであれば安定していて、ダンスなど激しい動きをしなければ大丈夫そうです。
トラッカーがちょっと動いたからといって一瞬でトラッキングがずれるわけでもないので、ダンスも大抵は大丈夫な気がしますが…
