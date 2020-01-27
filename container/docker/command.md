## dockerコマンド

### コンテナ、image掃除

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
* 未使用image削除
    * docker image prune

### その他

* コンテナを動かしたままコンテナを抜ける
    * ctrl+p, ctrl+q
* alpineコンテナへrootでログイン
    * docker exec -it --user root (コンテナID) sh
* dockerのCPU、メモリ等使用状況
    * docker container stats
* docker情報確認
    * docker info
        * ストレージドライバ等出力
* version確認
    * docker version
        * clientとserverのバージョン出力