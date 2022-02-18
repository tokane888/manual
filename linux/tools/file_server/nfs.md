# nfs

* 主にlinux向けのファイルサーバ

## 構築手順

### サーバ側

* インストール
  ```
  apt update -y
  apt install -y nfs-kernel-server
  ```
* 必要に応じてファイル共有用ユーザー作成+権限付与
  ```
  adduser file_server
  chown nobody:nogroup /home/file_server/
  chmod 777 /home/file_server/
  ```
* 公開ディレクトリ設定を下記に記載
  * /etc/exports
    * /home/file_server 192.168.0.0/16(rw,sync)
* NFS kernel再起動し、ファイル共有開始
  ```
  exportfs -a
  systemctl restart nfs-kernel-server
  ```

### クライアント側

* nfs clientインストール
  ```
  apt update -y
  apt install -y nfs-common
  ```
* mount point作成
  * mkdir /media/tmp
* マウント設定
  * 一時的にマウントする場合
    * mount -t nfs (server IP):(server上のパス) (local mount path)
      * 例) mount -t nfs 192.168.12.100:/home/file_server /media/tmp
  * 恒久的にマウントする場合
    * /etc/fstab に下記記載
      * {IP of NFS server}:{folder path on server} /var/locally-mounted nfs defaults 0 0
        * 例) 192.168.12.100:/home/file_server /media/tmp nfs defaults 0 0