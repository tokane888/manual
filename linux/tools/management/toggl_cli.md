# toggl cli

* 注意) 2byte文字関連関連エラーが出ており、セットアップ未完了
* 検証環境
  * raspberry pi 3B+
  * OS: raspbian OS buster
* 公式: https://github.com/AuHau/toggl-cli

## インストール手順

* apt update -y && apt install -y python-pip
* pip install togglCli
* toggl
  * 下記のエラーが出るので、~/.togglrcにtokenとtimezone(Asia/Tokyo)記載
    * raise IOError("Missing ~/.togglrc. A default has been created for editing.")
* toggl
  * TODO: 下記の2byte文字関連エラーが出ているので対応方法調査
    * UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
