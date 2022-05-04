# dockerエラー対応

## エラーメッセージ毎の対応

* Release file for http://security.ubuntu.com/ubuntu/dists/focal-security/InRelease is not valid yet 
  * hostの時刻ずれが原因。Docker for Windows再起動

## その他

* docker build失敗時に、exit状態になったコンテナの状態を確認
  * docker commit (container ID) tmp
  * docker run -it tmp sh
* docker logsの表示に非常に時間がかかる場合
  * 参考
    * https://www.code-mogu.com/2020/12/12/docker-logging/
  * 下記で末尾の指定行数のログ表示表示
    * docker logs (コンテナID) --tail=100
  * 上記でも時間がかかる場合
    * 下記でログファイルのパス特定
      * docker inspect (コンテナ名) | grep -i log
    * ログファイル削除
      * truncate -s 0 (logファイルパス)
  * コンテナごとにログファイルサイズ上限等設定する場合
    * docker-compose.yamlのservice名配下に下記のように記載
      ```
      logging:
        driver: "json-file"
        options:
          max-size: "10m"
          max-file: "5"
      ```
    * docker-compose down
    * docker-compose up -d