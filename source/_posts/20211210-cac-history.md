---
title: おたちょんを支える Cloud at Costの昔と今
date: 2021-12-10 23:39:49
tags:
    - Cloud at Cost
    - Server
    - Advent Calendar
---
この記事は[mstdn.maud .io Advent Calendar 2021](https://adventar.org/calendars/6291)の23日目の記事です。
昨日はやさいさんの[]()でした。

<!-- more -->
こんにちは、**ｈｏｔａ．ｈｉｒａｃｈｏｎ．ｏｔａｋａｎ．ｊｐ**のかんです。
今年も去年と同様にブログの更新をほとんど行わなかったため、このブログがA:don:vent Calendar専用となりました。

去年の[2020年にPC周りで買ったモノ](/2020/12/20/maud-adc/)はとても読みにくい記事となってしまいましたので、今年は購入報告を書きません。

今年を振り返ってみると、去年に引き続き非常にﾁｮﾝﾁｮﾝｶﾝｶﾝが捗った年でした。
あまりにも捗りすぎた結果、[おたちょん](https://hota.hirachon.otakan.jp)が爆誕してしまうほどに。

さて、このおたちょん鯖は2015年頃に(多少)話題となったCloud at Costを使用しています。Cloud at Costは買い切り型のVPSサービスを中心としたデータセンター事業を行っているところで、[過去](https://web.archive.org/web/20131004060805/http://cloudatcost.com/)には"You don't have to pay sky high prices to be in the Cloud."と謳っていたりします。
現在も**部分的には**追加費用なしで使用することが可能で、サーバー代金を支払う余裕のないおたちょんはCloud at Costを使用して維持費用を抑えることにしたわけです。

使用するからにはCloud at Costの現状について色々と調べる必要があり、せっかく調べたのでまとめて書き残すことにしました。

## 2015年
私がCloud at Costを使用し始めたのが2015年の11月なので、それ以降の事を書きます。サービス自体は2013年頃からあるはず。

ベンチマークとかは他サイトにいくらでもあるので書きませのん。~~計測がめんどくさい~~
### Cloud Servers
この頃に提供されていたサービスはCloud Serversという、買い切り型VPSのみでした。
データセンターはカナダのトロントにあります。

このVPSの大きな特徴として、買い切り型以外にリソースの自由割り当てがあります。

CPU: 2Core, Memoey: 1GB, SSD: 20GBが使用可能となるDeveloper 2プランを購入した場合、2Core/1GB/20GBで1つのサーバーを建てることはもちろん、1Core/512MB/10GBのサーバーを2つ建てることもできました。
購入したリソースの範囲内で、サーバーのスペックを自由に変更できる仕組みです。
ただし、一度建てたサーバのスペックを後から増強することはできません。
データの移行機能等も用意されておらず、この仕様は現在も変わっていません。

||Developer 1|Developer 2|Developer 3|Big Dog 1|Big Dog 2|Big Dog 3|
|----|----|----|----|----|----|----|
|月額|$1|$10|$20|$40|$60|$80|
|買い切り額|$35|$70|$140|$280|$560|$1120|
|CPU|1|2|4|4|6|8|
|Memory|512MB|1GB|2GB|4GB|6GB|8GB|
|SSD|10GB|20GB|40GB|60GB|70GB|80GB|
|IP|IPv4 public IP|IPv4 public IP|IPv4 public IP|IPv4 public IP|IPv4 public IP|IPv4 public IP|
|回線速度|100Mbit|100MBit|100Mbit|100Mbit|1Gbit|1Gbit|
|月転送量|500GB|1TB|3TB|3TB|5TB|無制限|

※SSDとはSuper Slow Diskの略

プランにはDeveloper 1 - 3とBig Dog 1 - 3が選択可能でした。
この頃は月額制も利用可能で、買い切り制は常に50%OFFセールを開催していたように思います。
購入後はDeveloperとBig Dogの扱いに大きな違いはなく、DeveloperとBig Dogのリソースを組み合わせて一つのサーバーを建てることも可能です。

サーバーは全てVMwareで構築されています。
OSにはCentOS, Debian, Ubuntu等が選択可能で、Big DogプランではWindowsも選択可能でした。
なお、Windowsのライセンスは別途用意する必要があります。

ホストの負荷低減のため、構築直後は自動シャットダウンが有効になっていました。
うっかりこれを無効化し忘れると、使っていない(と判断された)ときに問答無用でシャットダウンされます。
無効化しても障害でシャットダウンされることはよくありましたが。

サーバーの構築は10分程度で完了すればかなり早い方で、セール時などは構築に数時間掛かることもありました。

稼働中の安定性もあまり良くはなく、最悪の場合は突然起動不能となってサーバーを削除することしかできなくなります。
この場合にサポートへ問い合わせても、「サーバー作り直してね。データ消えるけど強く生きて」としか言われない。
ても、「サーバー作り直してね。データ消えるけど強く生きて」としか言われない。
複数のプランを組み合わせた場合の回線速度や月転送量の扱いについてはよく分かりません…

SLAは99.99%を謳っていましたが、普通に落ちます。
"CloudatCost provides a 99.99% uptime SLA. Any lost time is credited back to your account at a pro-rated amount for your monthly package."と書かれていましたが、返金されたことはありません。
買い切りプランだからですかね。

サポートチケットは(障害が発生していないときは)半日ほどで返答があります。
返答と共に問答無用でcloseされますが。

### CloudPRO 誕生
2015年11月の下旬頃(ブラックフライデーに合わせた?)にCloudPROのサービスが開始されました。
名前が変わっただけで中身は既存のCloud Serversサービスと大差ありません。

これ以降、データセンターがDC-01, DC-02, DC-03…と増えていきましたが、全てカナダだったはずです。
時期によって**比較的**安定稼働しているDCは異なるので、それぞれのDCでサーバーを構築してから、一番安定しているサーバーだけ残して使うようにしていました。
そのサーバーも数ヶ月後には使い物にならなくなることがよくありましたが。

||Developer 1|Developer 2|Developer 3|Developer 4|Developer 5|Developer 6|
|----|----|----|----|----|----|----|
|買い切り額|$35|$70|$140|$280|$560|$1120|
|CPU|1|2|4|4|6|8|
|Memory|512MB|1GB|2GB|4GB|6GB|8GB|
|SSD|10GB|20GB|40GB|60GB|70GB|80GB|
|IP|IPv4 public IP|IPv4 public IP|IPv4 public IP|IPv4 public IP|IPv4 public IP|IPv4 public IP|
|回線速度|1Gbit|1Gbit|1Gbit|1Gbit|1Gbit|1Gbit|
|月転送量|無制限|無制限|無制限|無制限|無制限|無制限|

月額プランが消え、回線速度や月転送量も全プランで同じものとなりました。
リソースの足し合わせがされた場合にこの辺りがどうなるかはあやふやだったので、統一されたのは良かったですね。

肝心の安定性は変わりません。

CloudPRO誕生キャンペーンかなにかで、1ユーザーにつきIPv4アドレスが100個配られました。なんで？

## 2016年
### BANされた
管理画面にログインできなくなっている事に気が付き、サポートにログインしてチケット切ったらBANされてました。(サポートと管理サイトは別ページでアカウントも違い、サポート側は生きていた)

「スパム行為は規約違反なんでBANしたで」といわれ、スパム判定を受けたサーバーの情報を聞いても教えてもらえませんでした。

{% asset_img cacsupport.jpg%}
無言でチケット閉じられたときは流石にキレた。

その後何度か問い合わせていたら、「俺がやるべきことではないけれども、サーバーを全て削除していいならアカウント復旧できるで」って言われたのでやってもらいました。
クソザル。

あとで調べたら、スパム行為をしたサーバーと同じサブネットにあったサーバーの所有者をBANしていたらしい。
無茶苦茶や。その後何度か問い合わせていたら、「俺がやるべきことではないけれども、サーバーを全て削除していいならアカウント復旧できるで」って言われたのでやってもらいました。
クソザル。

### Dedicated Servers 誕生
確か2016年だったはず。

いわゆる専用サーバーです。
最初はDedicated CAC1プランだけで、途中からCAC2が追加されました。
私は使わなかったので内容に関してはあまりわかりません…

||Dedicated CAC1|Dedicated CAC2|
|----|----|----|
|月額|$99|$129|
|CPU|Dual Quad Core Intel CPU|Dual Quad Core Intel CPU|
|Memory|32GB|32GB
|SSD|500GB SATA|1TB SATA|
|回線速度|1Gbit (/29 IPv4)|1Gbit (/29 IPv4)|

OSはLinux, Windows, VMware ESXを始めとして多様なものに対応!って書かれてました。

1 Hour Support Responseだとか24 hour Setupだとかも書かれていた記憶がありますが、実現されていたのかは疑わしい…

### Web Hosting 開始
2016年の11月頃にWeb Hostingサービスが開始されました。

cPanelで構築されていますが、沢山のユーザーが一つのサーバーに詰め込まれていたのかとても動作が重く、特にセール時はまともに使えませんでした。

ドメインは自分で用意する必要があり、ドメインを変更したい場合はサポートに問い合わせる必要があります。

[Softaculous](http://www.softaculous.com/softaculous/apps/)にある400を超えるアプリが使用可能!と謳われていましたが、少なくともownCloudは選べませんでした。

||cPanel-Basic|cPanel-Pro|cPanel-Business|cPanel-WHM Reseller10|
|----|----|----|----|----|
|買い切り|$10|$20|$30|$35|
|ドメイン|1|1|1|10|
|保存容量|10GB|100GB|200GB|100GB|
|データ転送|1TB|5TB|7TB|5TB|
|メールアドレス|10|100|200|100|
|MySQL データベース数|10|100|200|100|

Businessなんてプランがありますが、間違ってもBusiness用途で使ってはいけない。
その後何度か問い合わせていたら、「俺がやるべきことではないけれども、サーバーを全て削除していいならアカウント復旧できるで」って言われたのでやってもらいました。
クソザル。
Basic, Pro, BusinessについてはあとからSubdomainプランも追加され、無料でサブドメインを1つもらうことができたらしいです。

### Cloud Storage 開始
Web Hostingの開始と同時に始まったサービス。

実は有効期限付き/DL回数制限付きなら無料でファイルをアップロードできたりします。

||Cloud Storage1|Cloud Storage2|Cloud Storage3|
|----|----|----|----|
|月額|$9.8|$13.8|$19.8|
|容量|100GB|500GB|1TB|

1ファイル辺りのファイルサイズは無制限、FTP/NFSからのアクセスに関してはComing Soonとのことでしたが、5年経った今でもComing Soonです。

### Cloud VPN Service 開始
これもWeb Hostingの開始と同時に始まったサービス。

サーバーは10個用意されていますが、全部カナダです。

||Cloud VPN1|Cloud VPN2|Cloud VPN3|
|----|----|----|----|
|月額|$199|$299|$399|
|帯域|無制限|無制限|無制限|
|速度|表記なし|より早いスピード|スピード制限なし|を
|IP|共有IPv4|パブリックIPv4|専用IPv4|

ログ取ってません!100%安全です!!って謳っていたけど、実際どうかはわからん。

## 2017年
### Maintenance Fee
買い切り型を謳っていたCaCですが、流石に無理があったのでしょうか(当たり前)。
CloudPROの使用者は、維持費として毎年$9の支払いを求められるようになりました。
請求書を発行しても、メールとかで通知してこない辺りがCaC品質。払わないと管理画面にアクセスできなくなり、サーバーも強制削除…されるわけではなかった。

管理画面にはアクセスできなくなりましたが、サーバー自体は生きていて普通に使えていました。
それでいいんか。
~~そのうち勝手に落ちるから、復旧手段だけ封じれば良いという判断かもしれん~~

もちろん支払いをすれば管理画面へのアクセスが再度可能となりました。

### Production CloudPRO 誕生
2017年8月頃に始まったサービス。

Mission Critical向けに構築されたCloudPROです。

普通のCloudPROとの違いは、ファイアウォールと仮想ネットワーク機能だそうな。

||CloudPro Production1|CloudPro Production2|CloudPro Production3|
|----|----|----|----|
|月額|$49|$69|$89|
|CPU|4|6|8|
|ECC RAM|4GB|6GB|8GB|
|SSD|80GB|120GB|160GB|
|回線|1Gbit|1Gbit|1Gbit|
|仮想ファイアウォール|1|1|2|
|月転送量|無制限|無制限|無制限|

SLA 100%って謳い文句に対して「CaCを信じるぜ!あきらかにAWSよりも高品質だろ!!!」ってコメントされていたことを覚えています。

### Template Pack - OS PRO Pack
確かこの時期だったはずです。
OSのTemplateを更新することができるモノで、CloudPROへのUpgrade Packageとして販売されました。

逆に言えば、これを買わなければ一生古いバージョンのOSしか利用できません。[回避方法](https://blog.otakan.jp/2017/11/22/cac-ubuntu1604-install/)はありますが。

価格は$19.99で、Windows10, CentOS 6.9, Debian 8.8, Ubuntu 16.04が追加されます。

例によってWindows10のライセンスは別途自分で用意する必要があります。

## 2018年
### IPv6対応
IPv4にしか対応していなかったCloudPROが、ついにIPv6に対応しました。

Upgrade Packageの扱いで、$18.99を支払うことで利用可能となります。

### CloudPRO v2
||Developer 1|Developer 2|Developer 3|Developer 4|Developer 5|Developer 6|
|----|----|----|----|----|----|----|
|買い切り額|$35|$70|$140|$280|$560|$1120|
|CPU|1|2|4|4|6|8|
|Memory|512MB|1GB|2GB|4GB|6GB|8GB|
|SSD|10GB|20GB|40GB|60GB|70GB|80GB|
|IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|
|回線速度|1Gbit|1Gbit|1Gbit|1Gbit|1Gbit|1Gbit|
|月転送量|無制限|無制限|無制限|無制限|無制限|無制限|

OSのTemplateを更新することができるモノで、CloudPROへのUpgrade Packageとして販売されました。
CloudPROがアップデートされました！
でもスペック表を見た感じ大して変わらんぞ！！

IPv6アドレスがもらえるようになったのと、OSのテンプレートが更新されたくらいですね。

IPv6パッケージ購入者とOS Template購入者は…

v1を持っている人はアップグレード料金を支払うことでv2になれます。
私はアップグレードしませんでした。

## 2019年
### CloudPRO v3
2019年5月にCloudPRO v3が開始されました。

||Developer 1|Developer 2|Developer 3|Developer 4|Developer 5|Developer 6|
|----|----|----|----|----|----|----|
|買い切り額|$35|$70|$140|$280|$560|$1,120|
|CPU|1|2|4|6|8|10|
|Memory|2GB|4GB|8GB|12GB|16GB|20GB|
|SSD|20GB|40GB|80GB|120GB|160GB|200GB|
|IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|
|回線速度|1Gbit|1Gbit|1Gbit|1Gbit|1Gbit|1Gbit|
|月転送量|無制限|無制限|無制限|無制限|無制限|無制限|

* 新しいハードウェア
* さらに高速なインターネット速度
* 冗長インフラストラクチャ
* さらに高速なストレージ
* ディスク暗号化

今まで冗長構成も暗号化もしていなかったってマジ? …してなさそう。

v2に比べてスペックがアップグレードされましたね。
v2使用者は自動的にv3へアップデートされ、v1使用者は追加料金を支払うことでv3になれます。

あとv3ではMaintenance Feeが廃止されました。(え？

序盤で説明したとおり、CloudPROは全サーバーリソースの合算が可能なシステムです。
つまりv3を一つ買えばv1のリソースと合算され、全てv3のリソースとして利用可能と考えました。
加えてv3では年間$9のMaintenance Feeが廃止されるということで、Developer 1を購入することにしました。

結果、v1とv3はリソースが別々のものと扱われ、合算はされなかったのですが。
でもMaintenance Feeの請求は来なくなったよ。なんで？

### macOS Mojave on CloudPRO
謎。
```
macOS Mojave on CloudPRO
CloudatCost is pleased to announce the upcoming release of macOS on CloudPRO as a standalone product.

Using a Mac has always inspired great work. Now macOS Mojave brings new features inspired by its most powerful users but designed for everyone. Stay better focused on your work in Dark Mode. Automatically organize files using Stacks. Take more kinds of screenshots with less effort. Try three handy new built-in apps, and discover even more in the redesigned Mac App Store. Now you can get more out of every click.
```

突然こんなメールが来てサービスが始まり、気がついたら消えてた。

## 2020年
### CloudPRO v4
2020年3月ごろにCloudPRO v4が開始されました。

||Developer 1|Developer 2|Developer 3|Developer 4|Developer 5|Developer 6|
|----|----|----|----|----|----|----|
|買い切り額|$100|$200|$400|$240|$320|$400|
|CPU|1|2|4|6|8|10|
|Memory|2GB|4GB|8GB|12GB|16GB|20GB|
|NVME SSD|20GB|40GB|80GB|120GB|160GB|200GB|
|IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|
|回線速度|1Gbit|1Gbit|1Gbit|1Gbit|1Gbit|1Gbit|
|月転送量|無制限|無制限|無制限|無制限|無制限|無制限|

SSDがNVMEになったくらいで、それ以外のスペックは見た目上変わっていないですね。

価格がv3以前と比べて落ち着きましたが、一体何があった?
このあたりから80% OFFクーポンとかをバラ撒かなくなった印象があります。

それでも価格はたまに60% OFF(あるいはそれ以上)くらいはやっていたと思うけど。

v1, v3からのアップグレードには追加料金がかかります。
v4へのアップグレードは、1CPUあたり$1で可能です。私はアップグレードしませんでしたが。

### vC Platform

||vC 1|vC 2|vC 3|vC 4|vC 5|vC 6|
|----|----|----|----|----|----|----|
|買い切り額|$30|$60|$120|$240|$480|$960|
|CPU|1|1|2|4|6|8|
|Memory|1GB|2GB|4GB|8GB|12GB|16GB|
|NVME SSD|25GB|50GB|80GB|120GB|160GB|200GB|
|IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|IPv4/6 public IP|
|回線速度|1Gbit|1Gbit|1Gbit|1Gbit|1Gbit|1Gbit|
|月転送量|無制限|無制限|無制限|無制限|無制限|無制限|

"High Performance Compute on AWS"ということで、AWSの再販…?
このプランはすぐに消えました。

### Dedicated Server
いつの間にかDedicated Serverが更新されていました。

||Dedicated Server1|Dedicated Server2|Dedicated Server3|Dedicated Server4|Dedicated Server5|Dedicated Server6|
|----|----|----|----|----|----|----|
|買い切り型|$1,188|$1,548|$2,268|$4,788|$5,988|$8,388|
|CPU|2 Quad Core Intel|2 Quad Core Intel|2 Quad Core Intel|4 Quad Core Intel|4 Quad Core Intel|4 Quad Core Intel|
|Memory|16GB|32GB|64GB|128GB|256GB|512GB|
|SSD|1TB SATA or 120GB NVME|1TB SATA or 120GB NVME|1TB SATA or 120GB NVME|1TB NVME|2*1TB NVME|4*1TB NVME|
|回線速度|1Gbit|1Gbit|1Gbit|1Gbit|1Gbit|1Gbit|

12ヶ月分割払いにも対応していました。

### Dedicated Server XL
Dedicated Serverの上位プランもいつの間にか出現。

||Dedicated Server1|
|----|----|
|月額|$499|
|買い切り|$9,999|
|CPU|4*8 Quad Core Intel|
|Memory|1024GB|
|SSD|2*2TB NVME|
|回線速度|1Gbit|

こんなん誰が契約するの。

XL2とXL3はComing Soonになっていましたが、結局出たのかは不明。

### Dedicated Pro Server
2020年11月頃にはDedicated Server XLが消滅し、Dedicated Pro Serverに置き換わりました。

||Dedicated PRO1|Dedicated PRO2|Dedicated PRO3|
|----|----|----|----|
|買い切り型|$3,770|$4,460|$5,580|
|CPU|Xeon Silver 4208 2.1G 8C/16T|Xeon Silver 4214 2.2G 12C/24T|Xeon Silver 4216 2.1G 16C/32T|
|Memory|16GB RDIMM, 3200MT/s|32GB RDIMM, 3200MT/s|64GB RDIMM, 3200MT/s|
|SSD|1TB NVME|1TB NVME|1TB NVME|
|回線速度|1Gbit|1Gbit|1Gbit|

[Dell PowerEdge R440 Rack Mount Server](https://www.dell.com/ja-jp/work/shop/cty/pdp/spd/poweredge-r440/asper440_vi_vp)を使っていると公言していました。
なんとなくCaC側の取り分が見えてくる。

## 2021年
### Virtual Mining
2021年3月、CloudPRO v5が来るのかと思っていたらVirtual Miningが登場しました。

||M1|M2|M5|F10|F20|F40|
|----|----|----|----|----|----|----|
|利益/月|$5|$10|$25|$50|$100|$200|
|買い切り額|$40|$80|$200|$400|$800|$1,600|

||X60|X70|X80|X100|X200|X300|
|----|----|----|----|----|----|----|
|利益/月|$300|$350|$400|$500|$1000|$1500|
|買い切り額|$2,400|$2,800|$3,200|$4,000|$8,000|$12,000|

※利益はBTCの現在の価格とハードウェアのコストに基づいて変化

表だけを見ると、8ヶ月で元を回収してウハウハになれるみたいですね。
そんな簡単なわけがないですが。
まず維持費として5%をCaCが持っていくそうです。

そもそも本当に利益が出るなら販売をせずにCaC自身がマイニングを行えば良いわけで、それをしない以上は利益なんてたかが知れている。
維持費すら支払えない利益になった場合は、強制的にシャットダウンされるそうです。
維持費の支払いギリギリの利益しか出ないんじゃないですかねと思っています。

## その他
### API
2015年の春頃にCloudPROの[API](https://github.com/cloudatcost/api)が公開されました。

Webの管理画面では組めないようなメチャクチャ構成のサーバーを作ることができました。

でもメンテしてくれないので使えないよ。

### 追加パック
発売時期が把握できていない追加パック達
#### Feature Addon Bundle
追加パックの詰め合わせ。
価格は$19.99です。

* 全てのTemplate Pack
* IPv6
* High Availability
* Post Install Scripting

追加パックを個別に買う場合との価格差が全く見合っていない。

v1向けなので、v3以降は買う必要がありません。でもそんな注意書きどこにもないよ。

#### Template Pack - OS PRO Pack 2
OS Template Packの第二段。
価格は$29.99です。

* Windows Server 2016
* CentOS 7.5
* Debian 9.4
* Ubuntu 18.04

Windowsのライセンスはもちろん別途用意する必要があります。

v3以降は標準搭載です。

#### OS PRO Pack 3
OS Template Packの第三段。
なんでこれだけ名前が違うの?
価格は$29.99です。

* Windows10 1909
* Windows Server 2019
* CentOS 8.0
* Debian 9.11
* FreeBSD 12

Windowsのライセンスはもちろん別途用意する必要があります。

v3以降は標準搭載です。

#### Template Pack - Hosting Pack
WebHosting用のセットアップがされたOS Templateが利用可能になるらしい。
価格は$19.99です。

* cPanel-WHM
* Plesk
* WHMCS

#### Template Pack - Turnkey 1
TurnKey LinuxのOS Templateが利用可能になるパッケージです。
価格は$19.99です。

* Turnkey LAMP
* Turnkey LAPP
* Turnkey MySQL
* Turnkey PostgreSQL
* Turnkey Apache Tomcat
* Turnkey Wordpress

v3以降は標準搭載です。

#### Post Install Scripting
サーバーのセットアップ時に、自前で用意したスクリプトを実行させることができるようになります。

価格は$14.99です。は？

v3以降は標準搭載です。

#### High Availability (HA) - Developer CloudPRO
サーバーの可用性を高めることで、ホストに障害が発生した場合の復帰時間が短縮されるらしい。

君SLA 99.99%を謳っていなかった??

v3以降は標準搭載です。

## まとめ
### 9年間、サービスを続けたCac。彼は今、どうしているのか…
沢山のサービスが生まれましたが、多くのサービスが既に消えたか死んでいます。

おそらくハードを仕入れて、適当なプランでバラ売りするのを繰り返しているのでしょうね…

#### CloudPRO
前述の通り、v4が現在も販売されています。
v1, v3も今の所は利用可能で、おたちょんサーバーはv1で稼働しています。
以前は突然のシャットダウンやファイルシステムのReadOnly化、起動不能などの障害が発生していましたが、おたちょんサーバーを建てて3ヶ月の中では一度も発生していないため、昔と比べるとかなり安定しているのではないでしょうか。

#### Dedicated Servers
消えた。
2020年頃まではあったようですが、現在は公式サイトに一切の記述がありません。

少なくとも新規購入は不可能になっています。既に買った人が今でも使えているのかは不明…

#### Web Hosting
死んだ。
管理画面にアクセスすることすらできず、特にアナウンスもないので完全に終わったとみなして良いでしょう。
公式サイトにはもちろん一切の記述がありません。

2018年まではSSLの自動更新通知が来ていたので、2018年の末頃に落ちてそう。

#### Cloud Storage
半分死んだ。
公式サイトからは消えましたが、ストレージのサイト自体は[こちら](https://download.cloudatcost.com/)に残っています。

匿名でのアップロード/ダウンロードはもちろん、ログインしての利用も可能です。

FTP/NFSへの対応はいつ？

#### Cloud VPN Service
消えた。
公式サイトからの記述は消えました。
新規購入も不可能になっています。

面白半分で購入したものの、一切使っていないので使えるかは分からない。

#### Production CloudPRO
不明。
公式サイトからの記述は消えました。
新規購入も不可能になっています。

#### 追加パック
殆どの新規購入は不可能になった。
#### macOS Mojave on CloudPRO
消えた。

#### Dedicated Servers XL
消えた。

#### Dedicated PRO Servers
消えた。

#### vC Platform
消えた。

#### Virtual Mining
生きてる。

というか最近はこればっかり押してる気がする。


### 結局いくら払ったの?

|パッケージ名|個数|決済日|定価|割引|購入額|
|----|----|----|----|----|----|
|Cloud Servers Developer3|1つ|2015/11/7|$140|50% OFF|$70|
|Cloud Servers Developer3|1つ|2015/11/7|$140|50% OFF|$70|
|CloudPRO 3|1つ|2015/11/27|$140|80% OFF|$28|
|CloudPRO 3|1つ|2015/11/27|$140|70% OFF|$42|
|CloudPRO 3|3つ|2015/11/27|$420|70% OFF|$126|
|CloudPRO 3|1つ|2015/11/27|$140|80% OFF|$28|
|Maintenance Fee|-|2017/5/17|$9|-|$9|
|Cloud Storage 3|1つ|2017/11/26|$99|90% OFF|$9.9|
|Web Hosting cPanel - Business|1つ|2017/11/27|$30|90% OFF|$3|
|Maintenance Fee|-|2018/5/17|$9|-|$9|
|Cloud VPN1|1つ|2018/11/23|$24|80% OFF|$4.8|
|CloudPRO 1 v3|1つ|2018/12/26|$35|20% OFF|$28|
|合計|-|-|$1,326|-|$427.7|

VPS周りだけでも$410。
いや、殆どがVPSだな。
アホ。

## 支払い方法について
初期の頃はPaypalでの支払いに対応していましたが、途中からPaypalが消えました。
あまりにも返金要求が多すぎてPaypalからBANされたと聞きましたが、本当のところは不明。

今はクレカとビットコインでの支払いに対応しています。


## おたちょんを支えるサーバー
ｈｏｔａ．ｈｉｒａｃｈｏｎ．ｏｔａｋａｎ．ｊｐは$410支払って手に入れたサーバーで、いつ起こるかわからない障害に日々怯えながら稼働していることがわかりました! いかがでしたでしょうか。

2015年頃は追加料金のかからない買い切り型として話題になりましたが、結局トータルコストで見ると普通のVPSと大して変わらないので、あえてCaCを選ぶ意味はないですね。
むしろサポート品質等を考えれば選ぶべきではない。(当たり前のことですが)

2018年以降はあまり把握しきれていませんが、なんだかんだ続いているのはえらい。
一生追加料金無しでCloudPRO v1を続けてくれ。

明日の担当はひらちょんです。去年が「ほたちょんまとめ」だったということは…今年はきっと「おたちょんまとめ」ですね！！