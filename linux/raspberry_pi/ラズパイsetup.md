## ラズパイインストール方法

### ラズパイdockerコンテナ作成

* 実機linux起動
* docker run --name pi -d raspbian/stretch
    * EC2のubuntu上で上記実行 => コンテナ起動。up状態に
    * windows10のdocker toolbox上で実行した際は失敗

### SDカードへOS焼付

* OSイメージのLite版を以下からダウンロード
    * https://www.raspberrypi.org/downloads/raspbian/

#### Windows10上で焼き付ける場合

* SD Card Formatterダウンロード、インストール
    * https://www.sdcard.org/downloads/formatter/
        * 公式推奨とのこと
            * https://www.raspberrypi.org/documentation/installation/sdxc_formatting.md
* SDカードフォーマット
    * win10のスタートメニューから"SD Card Formatter"起動
    * デフォルト設定(下記設定)のまま"Format"押下
        * "Select card"から"boot"がついてるドライブ選択
            * TODO: ラズパイイメージが元々入っていたから？これ関係ないか確認
        * "Formatting options"から"Quick format"選択
    * 警告が出るので"OK"
    * 完了メッセージが出るので"OK"
        * 確認時は下記フォーマットになっていた
            * File system: exFAT
            * Cluster size: 128 kilobytes
            * Volume label: boot
        * 確認時、カード挿入時はD, Eドライブがあったが、完了後はDドライブのみになった
* balena Etcherダウンロード、インストール
    * cinst --yes etcher
* OSインストール
    * win10のスタートメニューから"balena Etcher"起動
    * "Select image"押下
    * 上でダウンロードしたOSイメージ(zip)選択
    * "Select target"押下
    * target選択 => Continue
    * "Flash!"押下
        * 確認時はSDカードの容量が通常より大きい(64GB)と警告が出たが"OK"押下

#### Ubuntu18.04上で焼き付ける場合

* SDカード指す
* SDカードのデバイスパス特定

    ```
    $ df
    (省略)
    /dev/mmcblk0p1  60747776      384  60747392   1% /media/tom/0123-4567
    ```
    * 下記コマンドでも可
        * lsblk -p
* SDカードをアンマウント
    * sudo umount (/devパス)
        * 例) sudo umount /dev/mmcblk0p1
* SDカードフォーマット
    * sudo mkfs.vfat -F32 (/devパス)
    * 例) sudo mkfs.vfat -F32 /dev/mmcblk0p1
* 焼付
    * --パス指定を誤るとOSが消えるので注意--
    * sudo dd if=(.imgのパス) of=(/devパス) bs=4M conv=fsync

### OSセットアップ

* デフォルトパスでログイン
    * user: pi
    * pass: raspberry
* 必ずパスワード変更
    * passwd
    * バックドア(kaiten), ネットワークスキャナ(zmap)など仕掛けられるケースあり
* 日本語キーボード配列に変更（半角／全角は効かない。追って調査）
    * `sudo raspi-config`
    * "4 Localisation Options"
    * "I3 Change Keyboard Layout"
    * "Generic 105-key PC (intl.)"
    * Keyboard layoutから"Other"選択
    * "Japanese"
    * "Japanese (OADG 109A)"
    * AltGrキーの役割選択で"The default for the keyboard layout"選択
    * "No compose key"
    * 参考）/etc/default/keyboard に設定追加されてるっぽい
* ネットワーク設定
    * `sudo raspi-config`
    * "2 Network Options"
    * "N2 Wi-fi"
    * "JP Japan"
    * "OK"
    * SSID入力
    * 参考) 上記の設定は下記に書き込まれる
      * /etc/wpa_supplicant/wpa_supplicant.conf
        * 複数wifi設定時
          * priority: 高い値の方に優先して接続される
        * 単一IPにした
        * ここの設定反映にはラズパイ再起動が必要
          * 少なくとも下記のservice restartでは反映されないことを確認
            * systemctl restart wpa_supplicant
      * /etc/dhcpcd.conf
        * 固定IP設定したけどだめ => ルーター再起動してもだめ
      * /etc/network/interfaces は古い設定ファイル
        * 当該ファイルにもコメントがあり、これの使用は非推奨
      * wifi起動・停止コマンド
        * ifdown wlan0
        * ifup wlan0
        * unknown interface wlan0出る
      * systemctl status wpa_supplicant
    * 参考) トラブルシューティング
      * アクセス可能なSSID一覧表示
        * sudo iwlist wlan0 scan | grep ESSID
* ssh有効化
    * `sudo raspi-config`
    * "5 Interfacing Options"
    * "P2 SSH"
    * "YES"
    * "OK"
* timezone設定
    * `sudo raspi-config`
    * timezone設定
        * "4 Localisation Options"
        * "Change Timezone"
        * Configuring tzdataで"Asia"選択
        * "Tokyo"
* 半角／全角キー有効化（調査中）
    * TODO: 下記の方法では全角／半角キーが効かないので対応方法調査
        * localeをraspi-configで設定するとターミナルの日本語が■になってしまう等ややこしいのでやらない
    * sudo apt update -y; sudo apt install -y uim uim-anthy fonts-ipafont fonts-ipaexfont;

### 参考

* https://www.1ft-seabass.jp/memo/2018/07/23/raspbian-install-201807-memo/