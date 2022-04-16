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

## 自動マウント設定

* ubuntu 18.04で動作確認
* lsblkで現在のマウント状態確認
  ```
  [sudo] password for tom:
  root@test:/home/tom# lsblk
  NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
  loop0    7:0    0   548K  1 loop /snap/gnome-logs/103
  (省略)
  loop16   7:16   0 149.9M  1 loop /snap/gnome-3-28-1804/67
  sda      8:0    0   477G  0 disk
  ├─sda1   8:1    0   512M  0 part /boot/efi
  └─sda2   8:2    0 476.4G  0 part /
  sdb      8:16   0 447.1G  0 disk
  └─sdb1   8:17   0 447.1G  0 part /media/hdd
  ```
  * /dev/sdb1が/media/hddにマウントされていることが分かる
* マウントするHDD等のUUID確認
  * blkid (/devパス)
    * 例)
    ```
    # blkid /dev/sdb1
    /dev/sdb1: LABEL="SSD-PGCU3-A" UUID="B06EBF5B6EBF1954" TYPE="ntfs" PARTUUID="dc2663d6-01"
    ```
* /etc/fstabに下記記載
  * UUID=B06EBF5B6EBF1954 /media/hdd auto nosuid,nodev,nofail,x-gvfs-show 0 0
  
## デバイスがHDDかSSDか判定する方法

* fdisk -lで当該デバイス探す
  * mainのストレージはefiを含む
* 当該Diskがrotationalか確認し、0ならSSD
  * cat /sys/block/sda/queue/rotational