# vsftpd(very secure ftp daemon)

## 導入手順

### サーバ側

* インストール
  ```
  apt update -y
  apt install -y vsftpd
  ```
* 設定
  * 下記編集
    * /etc/vsftpd.conf
      ```
      # ローカルユーザがディレクトリやファイルを作成する際に使用されるumaskを指定する。
      local_umask=022
      # ファイルのアップロードとダウンロードをログファイルに記録する。
      xferlog_enable=YES
      ```
* ftp用ユーザー作成
  * adduser hoge
  