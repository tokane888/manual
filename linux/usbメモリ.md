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
* lsblkでusbのデバイスパス確認
  * /dev/sddのような感じのパスになるはず
* 下記のような感じでパーティション1つにしてフォーマット
  * 最後の書き込みは数10秒かかる

    ```
    # fdisk /dev/sdd

    Welcome to fdisk (util-linux 2.31.1).
    Changes will remain in memory only, until you decide to write them.
    Be careful before using the write command.

    Device does not contain a recognized partition table.
    Created a new DOS disklabel with disk identifier 0xd27acde7.

    Command (m for help): m

    Help:

      DOS (MBR)
      a   toggle a bootable flag
      b   edit nested BSD disklabel
      c   toggle the dos compatibility flag

      Generic
      d   delete a partition
      F   list free unpartitioned space
      l   list known partition types
      n   add a new partition
      p   print the partition table
      t   change a partition type
      v   verify the partition table
      i   print information about a partition

      Misc
      m   print this menu
      u   change display/entry units
      x   extra functionality (experts only)

      Script
      I   load disk layout from sfdisk script file
      O   dump disk layout to sfdisk script file

      Save & Exit
      w   write table to disk and exit
      q   quit without saving changes

      Create a new label
      g   create a new empty GPT partition table
      G   create a new empty SGI (IRIX) partition table
      o   create a new empty DOS partition table
      s   create a new empty Sun partition table


    Command (m for help): p
    Disk /dev/sdd: 28.9 GiB, 31029460992 bytes, 60604416 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disklabel type: dos
    Disk identifier: 0xd27acde7

    Command (m for help): n
    Partition type
      p   primary (0 primary, 0 extended, 4 free)
      e   extended (container for logical partitions)
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-60604415, default 2048):
    Last sector, +sectors or +size{K,M,G,T,P} (2048-60604415, default 60604415):

    Created a new partition 1 of type 'Linux' and of size 28.9 GiB.

    Command (m for help): w
    The partition table has been altered.

    Calling ioctl() to re-read partition table.
    Syncing disks.
    ```
