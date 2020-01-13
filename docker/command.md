## dockerコマンド

### コンテナ掃除

* コンテナ指定して削除
    * docker rm [-f] (コンテナ名)
        * -fを付けると起動中のコンテナでも削除
* docker system prune -f
    * 停止しているコンテナ削除
    * dangling image削除
        * dangling image: docker build時に生成されるimage名`<none>`のimage
* docker system prune -af
    * 停止しているコンテナ削除
    * 使用されていないimage削除

### その他

* コンテナを動かしたままコンテナを抜ける
    * ctrl+p, ctrl+q
* dockerのCPU、メモリ等使用状況
    * docker container stats
* docker情報確認
    * docker info
        * ストレージドライバ等出力
* version確認
    * docker version
        * clientとserverのバージョン出力