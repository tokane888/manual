## シェルスクリプトのオプション

### test用オプション

* 以下参照
    * https://qiita.com/wakayama-y/items/a9b7380263da77e51711

### その他

* スクリプト実行時にログ取得
    * ./(実行スクリプト) 2>&1 | tee log.txt
* set -u 時に変数が空文字又は未定義か確認
    * [[ -v (変数名) ]]
        * bash限定
* $()内でexit 1しても終了しない問題
  * ()内はサブシェルなので、exit 1しても終了しない
  * ${command;}を変わりに使っておけば終了する
    * 呼び出し元と同じシェル環境を使用
    * ";"又はwhite spaceは必須
    * groupコマンドと呼ばれる
* nohup
  * ssh接続時
    * nohup内のコマンドはsshdの子プロセスになる
    * nohup内でsleep実行中にssh切断した場合、nohup内プロセスはsystemd直下へ移動され、親(main.sh)は死亡
      * ssh実行中のpstree結果(抜粋)
        ```
        systemd───sshd─┬─sshd───sshd───bash───pstree
                       ├─sshd───sshd───bash───main.sh───bash───sleep
                       └─sshd───sshd
        ```
      * ssh切断時のpstree結果
        ```
        systemd─┬─bash───sleep
                └─sshd─┬─sshd───sshd───bash───pstree
                       └─sshd───sshd
                ```