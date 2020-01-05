## ラズパイインストール方法

### OSインストール

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
* OSイメージのLite版を以下からダウンロード
    * https://www.raspberrypi.org/downloads/raspbian/
* balena Etcherダウンロード、インストール
    * https://etcher.io/
    * 公式推奨とのこと
        * https://www.raspberrypi.org/documentation/installation/installing-images/README.md
* OSインストール
    * win10のスタートメニューから"balena Etcher"起動
    * "Select image"押下
    * 上でダウンロードしたOSイメージ(zip)選択
    * "Select target"押下
    * target選択 => Continue
    * "Flash!"押下
        * 確認時はSDカードの容量が通常より大きい(64GB)と警告が出たが"OK"押下

### OSセットアップ

* デフォルトパスでログイン
    * user: pi
    * pass: raspberry
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