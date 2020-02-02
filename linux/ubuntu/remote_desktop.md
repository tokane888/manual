## ubuntuでリモートデスクトップ接続環境構築

[参考リンク](https://qiita.com/ryo-endo/items/00f3ec125917acf4cec7)

### 動確環境

Ubuntu 18.04実機

### ubuntu 18.04(server)側作業

* コピペ用
  ```
  sudo apt install -y ubuntu-desktop xrdp
  sudo systemctl enable xrdp
  sudo systemctl start xrdp
* 依存パッケージインストール
  * sudo apt install -y ubuntu-desktop xrdp
* xrdpサービス起動+OS起動時に自動起動するように設定
  ```
  sudo systemctl enable xrdp
  sudo systemctl start xrdp
  ```
* 警告回避設定
  ```
  cat <<EOF | sudo tee /etc/polkit-1/localauthority/50-local.d/xrdp-color-manager.pkla
  [Netowrkmanager]
  Identity=unix-user:*
  Action=org.freedesktop.color-manager.create-device
  ResultAny=no
  ResultInactive=no
  ResultActive=yes
  EOF
  ```
* 再起動？
  * TODO: 再起動必要か確認

### Windows10(client)側セットアップ&接続作業

* TightVNCダウンロード
  * https://www.tightvnc.com/download.php
* Windowsメニューから"TightVNC Viewer"起動
* RemoteHostにドメイン+port入力して接続
  * ex) ~~.mydns.jp:5900
* TODO: 実際に接続できること確認