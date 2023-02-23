# usbメモリ

## linuxでusbメモリ使用手順

### windows上作業

* usbメモリをfat32(default)でフォーマット
  * ntfsだとmount時に"unknown file system type 'ntfs'"と言われるため

### linux上作業(CentOSで検証)

* usbメモリ刺す
* マウントポイント作成
  * mkdir /mnt/usb
* usbが/dev配下のどこにマッピングされているか確認
  * 見分け方は下記
    * 基本的に/dev/sdb1とかにマッピングされている
    * C:\Users\tom\Desktop\program\other\manual\open\linux\device.md
* mount /dev/sdb1 /mnt/usb
  * /mnt/usbにusbがマウントされる
  * /dev/sdb1 のように、末尾の数字まで記載する必要あり

## USBメモリフォーマット手順

* 参考) https://www.wikihow.jp/Ubuntu%E3%81%A7USB%E3%83%A1%E3%83%A2%E3%83%AA%E3%83%BC%E3%82%92%E5%88%9D%E6%9C%9F%E5%8C%96%E3%81%99%E3%82%8B
