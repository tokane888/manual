# kali linux導入

## ラズパイ4へ導入

* 検証環境
  * デバイス
    * raspberry pi 4B+
    * メモリ: 8GB
    * sd card: 128GB
  * 導入OS
    * kali linux 2021.1
* OSダウンロード
  * 下記から"Kali Linux RaspberryPi 2 (v1.2), 3, 4 and 400 (64-Bit) (img.xz)"をダウンロード
    * https://www.offensive-security.com/kali-linux-arm-images/
* SDカードにimage焼付
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
    * unxz -cd kali-linux-2021.1-rpi4-nexmon-64.img.xz | dd of=(/devパス) bs=4M
* ネットワーク接続
  * kali linuxではnetplanはなく、Network Managerで接続を管理
  * 下記にwifi設定書き込み
    * /etc/NetworkManager/system-connections/pi.nmconnection
      ```
      [connection]
      id=pi
      uuid=421a4580-c519-41f9-a5a4-dc6e793d636d
      type=wifi
      interface-name=wlan0
      #permissions=user:tom:;
      #permissionsはコメントアウトしておかないと、localでログインするまでssh接続不可

      [wifi]
      mac-address-blacklist=
      mode=infrastructure
      ssid=pi

      [wifi-security]
      auth-alg=open
      key-mgmt=wpa-psk
      psk=(password)

      [ipv4]
      dns-search=
      method=auto

      [ipv6]
      addr-gen-mode=stable-privacy
      dns-search=
      method=auto

      [proxy]
      ```
      * (passwordの部分はカッコも含めてパスに置き換え)
      * uuidは恐らくデフォルトで記載されている
  * 設定再読み込み&service再起動
    * systemctl daemon-reload
    * systemctl restart NetworkManager
  * wifiのpower management無効化
    * sudo crontab -e
    * @reboot /usr/sbin/iwconfig wlan0 power off
* 起動時にLED off設定
  * sudo crontab -e
    * 下記追記
      ```
      @reboot echo "none" >/sys/class/leds/led0/trigger
      @reboot echo "none" >/sys/class/leds/led1/trigger
      ```
