## linuxでのデバイスの扱い

* OSが認識している全デバイスは/dev配下に配置される
  * HDD, SSD
    * /dev/sda : 最初に見つかったHDD
    * /dev/sdb : 2番目に見つかったHDD。以下c,d,eと振られる
    * /dev/sda1 : HDDの最初のパーティション
    * /dev/sda2 : HDDの2つ目のパーティション
  * HDD, USB等一覧表示
    * ls /dev/sd*
      * ubuntu 18.04での検証
        * usbマウント済みの場合
          ```
          root@tom-PC-LZ550NSB:/dev# ls /dev/sd*
          /dev/sda  /dev/sda1  /dev/sda2  /dev/sdb  /dev/sdb1
          ```
        * usbマウント解除状態
          ```
          root@tom-PC-LZ550NSB:/dev# ls /dev/sd*
          /dev/sda  /dev/sda1  /dev/sda2
          ```
          * GUIから"取り外し"時の表示
            * umount時はデスクトップからusb表示は消えるものの、/dev/sd* の結果は変化しなかった
              * /media/(ユーザー名) 配下のusbコンテンツ表示は消えた

## どのデバイスが目的のusb, hddか判別する方法

* fdisk -l
  * これで各デバイスの容量等確認して判別
  * /dev/sdb1 等、対象のデバイスを探す
* findmnt /dev/sdb1
  * 当該devのマウント先ディレクトリ確認
  * 未マウントの場合、出力は空
