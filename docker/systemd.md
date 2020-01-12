## dockerコンテナでsystemd使用

* 通常docker上ではsystemdは使用不可
* 下記条件を満たす場合に使用可能
    1. /tmpをtmpfsとしてマウント
        * --tmpfs /tmp --tmpfs /run
    2. /runをtmpfsとしてマウント
        * -v /sys/fs/cgroup:/sys/fs/cgroup:ro
    3. /sys/fs/groupが読み込み権限ありでマウントされている
    4. シャットダウンシグナルがSIGRTMIN+3として定義されている
        * --stop-signal SIGRTMIN+3
            * これを指定しないとシャットダウン処理が正常に行えないとのこと
    5. コンテナ起動時に/sbin/init指定
* EC2のCentOS7上で実行してsystemctl動くことを確認済みのコマンド
    ```
    docker container run --privileged -itd --name test --tmpfs /tmp --tmpfs /run -v /sys/fs/cgroup:/sys/fs/cgroup:ro -e container=docker centos /usr/sbin/init
    docker exec -it test bash
    ```
