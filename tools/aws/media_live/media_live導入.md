# MediaLive導入

## 概要

* MediaLiveのsystemは3つのsystemから成る
  * MediaLive channel
    * 映像を受信し、変換
  * upstream system
    * MediaLiveにsource content(映像)を提供する1つ以上のupstream system
    * 複数ある場合がある
    * 例
      * ネットに直接接続している映像配信カメラ
  * downstream system
    * MediaLiveから映像を受け取る
    * 例
      * 独自のservice(AWS Media Package等)
      * packager(AWS Media Package等)
      * CDN(Amazon Cloud Front)
      * 再生デバイス
      * 映像を視聴するwebsite
* MediaLive input
  * 取得方法
    * push
      * sourceがMediaLiveへ映像を送信
      * MediaLive input security groupが必要
    * pull
      * MediaLiveがsourceから映像を取得
  * input security group
    * upstream systemのIPを指定
      * 指定されたIPは、MediaLiveへの映像のpushを許可される
* MediaLive channel
  * channelへは複数入力を紐付け可能
    * 複数箇所からの同時に映像を受信することは不可
      * channelのscheduleで映像を切り替え可能
  * sourceから映像を取得し、変換し、output groupへpackagingする
  * 1つ以上のoutput groupを持つ
    * output groupは1つ以上のoutputから成る
  * channelには単一のscheduleを紐付け可能

## 試験的な映像配信手順

* 参考) https://blog.serverworks.co.jp/livestreamingwithmedialive
* 下記へアクセス可能なawsアカウント準備
  * MediaLive
  * MediaStore
  * IAM
  * CloudWatch
* obsをwin10上へインストール
  * choco install obs-studio
* MediaLiveのコンソールへsignin
  * https://ap-northeast-1.console.aws.amazon.com/medialive/home?region=ap-northeast-1#!/welcome
* channel作成
  * "Create channel"押下
  * Channel nameへ任意の名前を押下
    * 下記の例では"test_channel"とする
  * "Create role from template"選択の上、同名のボタン押下
* input作成
  * 左メニューから"create input"？押下
  * 下記入力
    * Input name: obus
      * サンプルでは上記
    * Input type: RTMP(push)
    * Network mode: Public
    * Input security
      * "Create"選択
      * IP入力
        * 例124.222.18.160/24
      * "Create input security group"押下
    * "Create"押下
  * 左メニューから"Input attachments"の"Add"押下
    * 

## 関連リソース


## 関連リンク

* APIリファレンス
  * https://docs.aws.amazon.com/medialive/latest/apireference/resources.html