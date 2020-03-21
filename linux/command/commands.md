## コマンド

各種linuxコマンド関連の解説。でかいのは分離

* grep
    * --exclude-dir=*.git
        * 特定の拡張子のディレクトリを検索対象から除外
    * オプション
        * -P：Perl-like regex。\Kへmatch position再設定
        * -o：一致部分だけ抽出
* sudo cdができない理由
    * sudoは指定された実行ファイルを実行
    * cdは実行ファイルではなく、組み込みコマンド
* 時刻整形して出力
    * date +%H:%M:%S
* historyコマンドに時刻付与
    * export HISTTIMEFORMAT='%F %T '
    * history
* history消去
    * history -c
* tar.gz
    * 圧縮
        * tar -zcvf ~~.tar.gz .
    * 解凍
        * tar -zxvf ~~.tar.gz
* tree
    * 表示階層制限
        * tree -L 2
            * 2階層下のディレクトリまで表示