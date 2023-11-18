## license関連コマンド

* 個別パッケージのlicense含む詳細情報取得
    * `yum info (パッケージ名)`
* 全パッケージのライセンス一覧取得
    * `rpm -qa --qf "%{name}: %{license}\n"`
    