## シェルスクリプトのオプション

### test用オプション

* [一覧](https://qiita.com/wakayama-y/items/a9b7380263da77e51711)

### その他

* スクリプト実行時にログ取得
    * ./(実行スクリプト) 2>&1 | tee log.txt
* set -u 時に変数に値が設定されていることを確認
    * [[ -v (変数名) ]]
        * bash限定
* 複数行出力
    ```
    cat << 'EOF' >> ~/.bashrc
    export TZ=Asia/Tokyo
    export LESSCHARSET=utf-8
    EOF
    ```
* $()内でexit 1しても終了しない問題
  * ()内はサブシェルなので、exit 1しても終了しない
  * ${command;}を変わりに使っておけば終了する
    * 呼び出し元と同じシェル環境を使用
    * ";"又はwhite spaceは必須
    * groupコマンドと呼ばれる
* sh -
  * -: - オプションの末尾。以降はファイル名等とみなされる。--と同等
  * [参考](https://unix.stackexchange.com/questions/423501/what-does-sh-mean)
* nohup挙動
  * ssh接続時
    * nohup内のコマンドはsshdの子プロセスになる
    * nohup内でsleep実行中にssh切断した場合、nohup内プロセスはsystemd直下へ移動され、親(main.sh)は死亡
      * main.sh内で`nohup bash -c (sleep含む関数)`を実行し、Conemuのウィンドウ閉じてssh切断した場合
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
  * trapでは、sshdが死んだ際にmain.shが死んでしまう関係で、nohupの代替にはならない