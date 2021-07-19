## ラズパイインストール方法

### ラズパイdockerコンテナ作成

* 実機linux起動
* docker run --name pi -d raspbian/stretch
    * EC2のubuntu上で上記実行 => コンテナ起動。up状態に
    * windows10のdocker toolbox上で実行した際は失敗

### SDカードへOS焼付

* OSイメージのLite版zipを以下からダウンロード
    * https://www.raspberrypi.org/downloads/raspbian/
    * zipは解凍と焼付を同時に行うため、ここでは解凍不要

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
    lsblk -p
    NAME             MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    (省略)
    /dev/sda           8:0    0   477G  0 disk
    ├─/dev/sda1        8:1    0   512M  0 part /boot/efi
    └─/dev/sda2        8:2    0 476.4G  0 part /
    /dev/mmcblk0     179:0    0  59.7G  0 disk
    ├─/dev/mmcblk0p1 179:1    0   256M  0 part /media/tom/boot
    └─/dev/mmcblk0p2 179:2    0  59.4G  0 part /media/tom/rootfs
    ```
    * 下記コマンドでも可
        * lsblk -p
* SDカードをアンマウント
    * sudo umount (/devパス)
        * 例) sudo umount /dev/mmcblk0p1 && sudo umount /dev/mmcblk0p2
* パーティテョンフォーマット
    * sudo mkfs.vfat -F32 (/devパス)
    * 例) sudo mkfs.vfat -F32 /dev/mmcblk0p1
* 全体フォーマット
    * sudo mkfs.vfat -F32 -v -I (/devパス)
    * 例) sudo mkfs.vfat -F32 -v -I /dev/mmcblk0
* 焼付
    * --パス指定を誤るとOSが消えるので注意--
    * unzip -p (.zipパス) | sudo dd of=(/devパス) bs=4M conv=fsync
        * 例) unzip -p 2021-03-04-raspios-buster-armhf-lite.zip | sudo dd of=/dev/mmcblk0 bs=4M conv=fsync
        * zip解凍済みの場合
            * sudo dd if=(.imgのパス) of=(/devパス) bs=4M conv=fsync
* SDカードをマウント
    * mkdir /media/sd
    * mount (/devパス) /media/sd
        * /devパス2つある
            * /devの末尾が2の方が恐らく正解
            * マウントしてみてetc, boot等のディレクトリがあれば正解
        * 例) mount /dev/mmcblk0p2 /media/sd
* SSH有効化
    * touch /media/sd/boot/ssh
        * ファイルがあればOSの方でsshを有効化する
        * 実際にやってみたところ、sshが有効化されていなかった。sshがdisabledのまま
            * sshファイルでも、sshディレクトリでも失敗
            * TODO: 原因調査
* wi-fi設定
    * sudo wpa_passphrase (SSID) (pass)
        * 例) wpa_passphrase wi-fi password
            ```
            network={
            ssid="(SSID)"
            #psk="(生のWIFIパスワード)"
            psk=(暗号化されたWIFIパスワード)
            }
            ```
    * 上記で出力された設定を下記の末尾へ追記
        * vim /media/sd/etc/wpa_supplicant/wpa_supplicant.conf
            * update_config=1の直下に下記も追記
                * country=JP
                    * countryを記載しないと、rfkillにWi-Fiを無効化される
* (option)IP固定
    * ルーター側から新規追加のIPを把握できない場合等は固定必須
    * 下記へIP追記
        * vim /media/sd/etc/dhcpcd.conf
            ```
            interface wlan0
            static ip_address=192.168.11.2/24
            static routers=192.168.11.1
            static domain_name_servers=192.168.11.1
            ```

### OSセットアップ

* デフォルトパスでログイン
    * user: pi
    * pass: raspberry
* 他のユーザー作成
    * adduser tom
    * visudo修正
* piユーザーを残す場合は必ずパスワード変更
    * passwd
    * バックドア(kaiten), ネットワークスキャナ(zmap)など仕掛けられるケースあり
    * 削除する手順は後述。opensshでの接続及びsudo su成功後の削除
* 日本語キーボード配列に変更（半角／全角は効かない。追って調査）
    * `sudo raspi-config`
    * "Localisation Options"
    * "L3 Keyboard"
    * "Generic 105-key PC (intl.)"
    * Keyboard layoutから"Other"選択
    * "Japanese"
    * "Japanese (OADG 109A)"
    * AltGrキーの役割選択で"The default for the keyboard layout"選択
    * "No compose key"
    * 参考）/etc/default/keyboard に設定追加されてるっぽい
* ネットワーク設定
    * `sudo raspi-config`
    * "Localisation Options"
    * "L4 WLAN Country"
    * "JP Japan"
    * "OK"
    * SSID入力
        * ラズパイ4の場合は、"1 System Options" => "S1 Wireless LAN"でSSID入力
        * ラズパイzeroは5.0GHz非対応なので注意
    * 課題
        * 何故かwifi接続が不安定
            * pingで一度叩いてやるまでssh接続できないなど
                * zeroではこれ。pi4ではpingで叩いても接続できないことあり
            * Low voltage detected 警告が出ているので、電源の問題かもしれない
    * 参考) 上記の設定は下記に書き込まれる
      * /etc/wpa_supplicant/wpa_supplicant.conf
        * 複数wifi設定時
          * priority: 高い値の方に優先して接続される
        * 単一IPにした
        * ここの設定反映にはラズパイ再起動が必要
          * 少なくとも下記のservice restartでは反映されないことを確認
            * systemctl restart wpa_supplicant
      * /etc/dhcpcd.conf
        * 電源不安定だと固定IP設定、wifi設定が正常反映されないケースあり
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
    * "Interface Options"
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
* OS起動後にHDMIを指しても画面表示可能に
    * /boot/config.txt
    * 下記をコメントアウト解除して有効化
        * hdmi_force_hotplug=1
* 分かりやすいように適切なhostname設定
    * sudo hostnamectl set-hostname (host名)
* hostname変更するとsudo時に下記のような警告が出るようになるので対応
    ```
    $ sudo su
    sudo: unable to resolve host test: No address associated with hostname
    ```
    * /etc/hostsに下記追記
        * 127.0.1.1 (host名)
* 半角／全角キー有効化（調査中）
    * TODO: 下記の方法では全角／半角キーが効かないので対応方法調査
        * localeをraspi-configで設定するとターミナルの日本語が■になってしまう等ややこしいのでやらない
    * sudo apt update -y; sudo apt install -y uim uim-anthy fonts-ipafont fonts-ipaexfont;
* カメラ有効化
    * 最初はレンズに小さい保護シールが貼られているので、必要に応じて剥がす
    * sudo raspi-config
        * 表示された設定項目一覧からcameraをenable
    * rtspサーバインストール
        * ラズパイ4の場合

            ```
            git clone https://github.com/mpromonet/h264_v4l2_rtspserver.git
            cd h264_v4l2_rtspserver
            cmake .
            sudo make install
            cd ..
            rm -rf h264_v4l2_rtspserver/
            ```
    * 下記コマンド実行で配信
        * sudo v4l2rtspserver
        * 解像度指定も可能
            * 例) v4l2rtspserver -W 1920 -H 1080 -F 10
        * カメラ映像は事前に角度を指定してからサーバを起動することで回転可能
            * v4l2-ctl -c rotate=180
    * VLC Media Playerで下記を"ネットワークストリームで開く"と視聴可能
        * rtsp://(ラズパイIP):8554/unicast
        * ラズパイのピントが合っていない場合、専用器具でレンズの周りの舵のような部分を回転させてピント合わせ
    * カメラ使用権限設定
        * sudo chmod 755 /dev/vchiq
        * 静止画撮影時には必要。動画撮影時に必要かは不明瞭
    * 下記で自動起動設定
        * crontab -e
        * 下記追記
            * @reboot /usr/local/bin/v4l2rtspserver > /var/log/camera.log 2>&1
    * 参考) 静止画撮影
        * sudo raspistill -v -o test.jpg
        * 上下左右逆の場合
            * sudo raspistill -vf -hf -v -o test.jpg

### 参考

* https://www.1ft-seabass.jp/memo/2018/07/23/raspbian-install-201807-memo/