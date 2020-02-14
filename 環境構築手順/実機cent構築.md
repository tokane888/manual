## CentOS実機環境構築手順

### OSダウンロード、インストール

* 下記からダウンロード
  * 最新版
    * https://www.centos.org/download/
  * 他version
    * https://wiki.centos.org/Download
* rufasへ焼付
  * 基本的に標準設定でOK
    * 焼くだけなのでFAT32のままでもOK
* インストール
  * (USBからのCentOS7インストールで検証)
  * (USB刺してPC起動した時点から検証)
  * "Install Cent OS"
  * 言語選択
  * ディスク選択
  * 容量確認してSSD押下
  * "パーティションを自動構成する"
  * "領域の確保"
    * 既に別のOSインストール済みの場合に出る
  * "すべて削除"
    * TODO: デュアルOSにする場合の手順確立
  * "領域の再利用"
  * "ネットワークとホスト名"
    * wifiのパスなど設定
  * "ソフトウェアの選択"
    * 必要に応じてデスクトップ環境追加(未検証)
    * デフォルトの"最小限のインストール"だとCUI環境になる
  * "インストールの開始
  * "ROOTパスワード"
    * 適宜設定
  * "ユーザーの作成"
    * "このユーザーを管理者にする"押下
      * sudo可能になる
  * (完了後)再起動
  * (再度centOSインストールの画面が出る)
  * 電源ボタン押してシャットダウンして再起動でOS起動

[参考](https://eng-entrance.com/linux-centos-install)

### 後からネットワーク有効化

注）wifiでの接続方法は調査中
　OSインストール時にwifiの設定をしてもつながらなかった

* sudo su
* インターフェース名確認
  * nmcli d
    * wifiだとunmanagedになっていた
* ネットワーク設定
  * nmtui
  * "Edit a connection"
  * "Add"
  * wifi追加時
    * "wifi"
    * (SSID等設定)
  * sudo reboot
    * TODO: どのservice restartすればよいか確認
      * systemctl restart NetworkManager ?
  * NetworkManagerのstatus確認
    * systemctl status NetworkManager
      * TODO: wifiだとこの時点でつながらなかったので確認
        * 有線だとつながった
  