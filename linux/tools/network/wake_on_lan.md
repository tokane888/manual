# wake on lan

## ubuntuをラズパイから遠隔起動

* ubuntu側準備
  * BIOSでwake on lan有効化
  * ethtoolインストール
    * apt install -y ethtool
  * 現在のWOL設定確認
    * ethtool (network interface名) | grep -i wake
      * "Wake-on"がgであれば有効。dであれば無効
  * Wake on LAN一時有効化
    * ethtool -s (network interface名) wol g
  * Wake on LAN常時有効化
    * crontab -e実行し、下記記載
      * @reboot /usr/sbin/ethtool -s eno1 wol g
* ラズパイ側準備
  * apt install -y etherwake
  * etherwake -i (network interface名) (mac address)