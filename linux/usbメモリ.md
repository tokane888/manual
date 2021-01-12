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