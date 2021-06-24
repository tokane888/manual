# dockerエラー対応

## エラーメッセージ毎の対応

* Release file for http://security.ubuntu.com/ubuntu/dists/focal-security/InRelease is not valid yet 
    * hostの時刻ずれが原因。Docker for Windows再起動

## その他

* docker build失敗時に、exit状態になったコンテナの状態を確認
    * docker commit (container ID) tmp
    * docker run -it tmp sh