# android root化

## android端末購入時等に注意しておきたい点

* google製のものを使用
  * TWRP等の対応が早いため
* androidのmajor ver upは安易に行わない
  * TWRPの対応に1年以上かかるため
* 後述の"Patch済みのBoot Imageを使用する方法"でroot化した後の各アプリの挙動
  * kyash: サポート外と表示され使用不可
  * suica: ログインするだけで復旧
  * paypay: ログインするだけで復旧

## 検証環境

* android pixel 3a
  * android ver 11
    * 11はTWRPが未対応であったため、途中で10に戻している
    * 10はTWRP上でmountエラーが発生したため、9に戻している
* windows 10
* twrp-3.5.2_9-0-sargo.img
* Magisk-v23.0.img => .zipに名称変更
* 参考手順) https://sp7pc.com/google/root/34313

## 事前準備

* windowsへadb導入
  * 下記からandroid SDK Platform-Toolsダウンロード
    * https://developer.android.com/studio/releases/platform-tools
  * ダウンロードしたディレクトリを任意のパスに配置
  * 配置先のディレクトリにPATHを通す
  * コマンドプロンプトで"adb"実行し、メッセージが出れば成功
* 開発者モード有効化
  * 設定画面のビルド番号7連打
* USBデバッグ有効化
  * 開発者向けオプションから有効化
* localで動作するアプリからデータバックアップ

## root化

* PCのusb type-Aとスマホのtype-Cへ接続
  * PC側をtype-Cにすると安定しない場合が多いとのこと
    * 実際に3,4本試したがどれも接続失敗した
  * USB接続後、USBデバッグを許可
    * 許諾を求める通知が出ない場合はUSBを交換
* OEMロック解除の許可
  * 開発者向けオプションから許可
* boot loaderをunlock
  * adb reboot bootloader
* ブートローダー解除
  * 下記実行でandroid初期化されるので注意
  * fastboot flashing unlock
  * androidから"Unlock the bootloader"選択で解除
  * 電源ボタン押下でandroid再起動

## TWRPを使用する方法

### 長所

* 簡単らしい。結局できなかったが

### 短所

* TWRPが未対応のverのandroid OSでは使用不可

### 手順

* 機種に応じたTWRP image download
  * https://twrp.me/Devices/
  * TWRPはandroidを復号化し、カスタムファームウェア等の導入を可能にする
* android OSのversionを確認し、TWRPが当該OSに対応済みであるか各種サイトで確認
* TWRPが現在のandroid verに非対応の場合、下記手順で古いandroidインストール(pixelの場合の手順)
  * 下記リンクから当該のPixel又はNexusデバイスを選択し、"Flash"押下
    * https://developers.google.com/android/images
    * 基本的に古いversionの方が安定して動作するので、最新より2つ程major verのOSを選択
  * ブラウザ上のadblockなどは念の為offに
  * "Get started"押下
    * Android USB Driverのダウンロードを促される。インストールされていない場合はインストール
  * ADB keysへのアクセスを求めるダイアログが出るので、"Allow ADB Access"押下
  * Add new device => (androidデバイス選択)
  * Unavailableの欄にデバイスが表示された場合はコマンドプロンプトから下記実行
    * adb kill-server
  * Select device => Force Inspect
    * android上でPCからのUSBデバッグを許可
      * 既に許可済みでも再度表示される
        * chrome上からの実行になるためと思われる
  * 当該デバイスを"Select"
  * Selected Targetからandroidのverに応じたimageを選択
    * 誤ったものを選択したところ"Install build"ボタンが無効化された
  * "Wipe Device"選択
  * Install build => Confirm => I Accept
  * 経過が報告されるので、ブラウザurl欄右から通知有効化
    * 他のプログラム実行中でfailした場合は下記実行
      * adb kill-server
  * androidがFastboot Modeで再起動し、ブラウザ上に"Reselect your device"表示が出るので下記押下
    * "Reselect device"
  * ポップアップからデバイスを再度選択
  * 完了通知を受け取り次第、androidをオフライン設定でセットアップ
  * ビルド番号7連打で開発者モードに
  * 念の為下記で自動updateを無効化
    * 設定アプリ起動 => 開発者向けオプション => 自動システムアップデートを無効化
  * 開発者向けオプション画面で"USBデバッグ"有効化
  * USBでandroidとwindowsを接続し、USBデバッグ許可
  * adb devicesで正常にデバイス一覧が表示されること確認
    ```
    C:\>adb devices
    List of devices attached
    95AAY0UBG7      device
    ```
* Magiskの.apkファイル download
  * https://github.com/topjohnwu/Magisk
* .apkを.zipに名称変更
* .zipをwindowsからandroidの内部ストレージの最上位パスに転送
  * この状態では他のパスは見えないはず
  * 転送できない場合、下記のデータマウント周り調整
* android上のusb接続通知を選択し、USBをファイル転送に使用するように設定
* adb reboot bootloader
  * fastboot modeの画面が表示される
* fastboot boot (downloadした.imgのパス)
  * 下記のメッセージが出る場合はデバイスが認識されていない可能性が高い
    * < waiting for any device >
      * fastboot devicesで認識済みデバイス一覧表示可能
    * デバイスが認識されない場合はデバイスマネージャでエラーが出ていないか確認
      * 出ていた場合、スマホがgoogle製ならgoogleのusbドライバをダウンロード
        * https://developer.android.com/studio/run/win-usb?hl=ja
  * 下記のメッセージが出る場合は、TWRPが、インストール済みのandroidのverに未対応の可能性がある
    * Booting   FAILED (remote: 'Error verifying the received boot.img: Invalid Parameter')
    * TWRPが対応済みのandroid verを確認
      * androidプリインストールのOS verがTWRPが対応済みのものであれば、androidを出荷時リセットする必要あり
    * 必要なら↑の"TWRPが現在のandroid verに非対応の場合、下記手順で下記手順で古いandroidインストール"に従ってandroid OSを古いものに置き換え
* "Swipe to Allow Modification"をスワイプ
* Install => 転送してあるMagiskの.zipのパスを指定 => swipe
  * android 10では後述のマウント周りの調整後に下記で見つかった
    * /data/media
  * 見つからない場合、内部ストレージのマウントに失敗している可能性がある
    * 画面右下の4本線アイコン押下でログ確認可能
      * 再度押下で元画面へ
      * android 10ではmount関連エラーがあったが、android 9ではエラーなし
    * 内部ストレージのマウントに失敗している場合は下記で復旧可能？
      * 実際に試したところ、ファイルはTWRP上で転送できるようになったが、OSは起動しなくなった。。
      * 参考) https://forum.xda-developers.com/t/how-to-fix-unable-to-mount-data-internal-storage-0mb-in-twrp-permanently.3830897/
      * Wipe
      * Advance Wipe
      * Data選択
      * Repair or change File System
      * Repair File System => (swipe)
      * Repairに失敗した場合は下記
        * Back
        * Change File System
        * Ext2 => (swipe)
        * Back => Ext4 => (swipe)
        * Back => (戻る矢印連打で最上位へ)
        * Installからdataディレクトリの項目が選択可能になっていることを確認
      * だめな場合はInternal Storage選択
        * 修復時にInternal Storageのデータは消去される
      * TODO: OSをandroid 9の一番古いverに下げて再度試した結果記載
    * OSの再インストールが必要になる場合がある
  * 下記のエラーが出る場合がある。ほかにも複数行。Exit 1で終わったと表示された
    * Failed to mount 'system_root' (Invalid argument)
    * 解決方法不明
    * android 10で確認
  * android 9では暗号化を解除しないと.zipがTWRPから読み取れない
* Wipe Delvik
* Reboot System
* TODO: 成功したら方法追記

## Patch済みのBoot Imageを使用する方法

### 長所

* 最新のandroid OSでも使用可能

### 短所

* TWRPは使えない？
* ソフトウェアupdateの際に困ることがあるらしい
* 参考リンクでは非推奨になっていた

### 手順

* 参考) https://forum.xda-developers.com/t/guide-how-to-root-your-pixel-3a-and-install-magisk-android-9-12.3938783/
* 最新のandroid OSを焼き付け
  * 2つmajor verが古いandroid 9で後述のpatch作業を行ったところOSが起動しなくなることがあったため
* 下記のgoogleのサイトから当該verの"Link"を押下し、zipをダウンロード
  * https://developers.google.com/android/images
* zipを解凍 => 解凍したディレクトリ内のzipを解凍 => boot.img取得
* androidのダウンロードディレクトリへboot.imgを転送
* 下記からMagisk Manager appの.apkをダウンロード
  * https://github.com/topjohnwu/Magisk
* 上記.apkをインストール
* Magiskを起動
* "Magisk"の"Ramdisk"が"対応"になっていることを確認
  * "Yes"でない場合は下記リンクの指示に従うようにとのこと
    * https://topjohnwu.github.io/Magisk/install.html#magisk-in-recovery
* "Magisk"欄と"アプリ"欄に"Install"があるが、"Magisk"欄の"Install"を選択
* "パッチするファイルの選択" => boot.img選択 => "始める"
  * Magiskがboot.imgをPatchしたmagisk_patched-***.imgをboot.imgがあるダウンロードディレクトリへ出力する
  * メッセージに出力先パスは出力される
* USBでWindowsと接続し、Patch済みの.imgをwindowsへ退避
  * エクスプローラ上にPatch済みの.imgのみ表示されない場合、下記のようなadb pullコマンドで退避
    * adb pull /storage/emulated/0/Download/magisk_patched-23000_kDTsh.img
* adb reboot bootloader
* .img焼付
  * fastboot flash boot magisk_patched-23000_kDTsh.img
* 再起動
  * 音量ボタン押下で"Power off"選択
  * 電源長押で起動
* Google playからRoot Checkerをインストールし、root化されていることを確認
* 自動システムアップデート無効化
  * 設定 => システム => 開発者向けオプション => 自動システムアップデート無効化

## 関連ツールインストール、設定

* busybox
  * google playから"Busybox(stericson)"インストール
    * 恐らくProでなくても良さそう
  * アプリ起動 => root要求されるので"Yes"
  * Install Busybox
  * ssh等でデプロイ先の下記パスが存在することを確認
    * /system/xbin
    * android 11では存在しなかったため、デプロイ先を下記に変更した
      * /system/bin
  * Install
  * 注意点
    * android再起動後に確認したところ、viコマンドがなくなっていた
      * 再度busybox installで復活
      * android OSが勝手に消した可能性がある
* AdAway
  * /system/etc/hostsを書き換えて広告ブロックするアプリ
    * root化しないと/systemに書き込めない
  * MagiskでSystemless hosts有効化
  * 設定有効化のためandroid再起動
  * 下記から.apkダウンロード
    * https://github.com/AdAway/AdAway
  * .apkインストール
* SSH Server
  * root化しなくても使用可能だが、ほとんど何もできない
  * Magiskでsudo権限付与
* Magisk
  * 設定 => Magiskアプリを隠す => 戻る => 画面下の縦アイコン押下 => MagiskHide
    * 下記に対してMagiskHide有効化
      * suica
      * EXアプリ
      * PayPay
      * kyash
      * Okta Verify
  * "Magiskアプリを隠す"有効化
* アルテ日本語入力キーボード
  * root化関係ないが。。
  * ケータイ配列での入力方法の設定 => ターンフリック(TFEi)

## 課題

* 下記アプリがログイン失敗
  * amazon
* 下記がroot検知されて動作せず
  * google pay
* 下記はインストール不可
  * kyash
* ドメインブロック
  * dnsmasqはテザリング時にしか実行されない