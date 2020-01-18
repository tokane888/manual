## シェルスクリプトのオプション

### test用オプション

* 以下参照
    * https://qiita.com/wakayama-y/items/a9b7380263da77e51711

### その他

* スクリプト実行時にログ取得
    * ./(実行スクリプト) 2>&1 | tee log.txt
* set -u 時に変数が空かどうか確認
    * [[ -v (変数名) ]]
        * bash限定