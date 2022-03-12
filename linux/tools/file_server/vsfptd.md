# vsftpd(very secure ftp daemon)

## 導入手順

### サーバ側

* インストール
  ```
  apt update -y
  apt install -y vsftpd
  ```
* 設定
  * sftp用user追加
    * adduser sammy
  * ftrp用ディレクトリ追加
    * mkdir /home/sammy/ftp
  * 権限設定
    * chown nobody:nogroup /home/sammy/ftp
    * chmod a-w /home/sammy/ftp
    * mkdir /home/sammy/ftp/files
    * chown sammy:sammy /home/sammy/ftp/files
  * 必要に応じて下記編集
    * /etc/vsftpd.conf
      ```
      # ローカルユーザがディレクトリやファイルを作成する際に使用されるumaskを指定する。
      local_umask=022
      # ファイルのアップロードとダウンロードをログファイルに記録する。
      xferlog_enable=YES
      # 書き込み可能に
      write_enable=YES
      # home directory以外へアクセス不可に
      chroot_local_user=YES
      # ログイン時のディレクトリ調整
      user_sub_token=$USER
      local_root=/home/$USER/ftp
      # パッシブモードのport範囲制限
      pasv_min_port=40000
      pasv_max_port=50000
      # userが明示的にuser listに記載された場合のみアクセス許可
      userlist_enable=YES
      userlist_file=/etc/vsftpd.userlist
      userlist_deny=NO
      ```
  * user listを使用する場合は下記のようなコマンドで許容するuser記載
    * echo "sammy" | sudo tee -a /etc/vsftpd.userlist
  * 設定反映
    * systemctl status vsftpd
* ftp用ユーザー作成
  * adduser hoge
