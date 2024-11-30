# DB接続失敗

## 想定状況

1. docker-compose.ymlでDBと、DBへ接続するclient起動
2. 接続失敗

## 切り分け手順

1. 認証情報に明確な誤りがないか列挙して確認
2. host側terminalからDBへ接続可能か確認
   1. 接続成功時
      1. DBの設定自体に問題がないことが確定
   2. 接続失敗時
      1. port forward設定に問題がないか確認
         1. host側は0.0.0.0でないと接続不可
            1. 127.0.0.1だとhost側からは接続出来ない
3. clientコンテナ起動時のソースの頭にsleep 5などを入れて接続成功するか確認
   1. 成功時
      1. コンテナ起動順序の問題
         1. docker-compose.ymlにdepends_on指定追記で対応
            1. health_checkも追記
            2. mysqlでは起動時に毎回DB初期化処理を行うと、healthcheck後にclientを起動しても0.2秒程度clientの起動がmysqlより早くなってしまう
               1. mysql build時にデータ初期化することで回避可能
               2. DB側とclient側のログをミリ秒単位で出力し、時系列を整理することで切り分け可能
   2. 失敗時
      1. 起動順序の問題ではないことが確定
4. clientコンテナが接続失敗してもしばらく終了しないよう、例外発生箇所等にsleepを入れてterminalからコンテナ内へ
   1. DBコンテナへpingが通ること確認
   2. DBコンテナのnameが名前解決可能であること確認
      1. docker commitし、再度runする方法だとnetwork周りの設定が保持されないので注意
   3. mysqlコマンドなどで当該DBへ接続可能であるか確認
   4. mysqlでは起動時に毎回DB初期化処理を行うと、healthcheck後にclientを起動しても0.2秒程度client
