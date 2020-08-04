# パフォーマンス計測

* cpu使用率
  * 簡易的に計測
    * top
      * 実行userで絞って出力
        * u => (user名) => enter
      * CPU使用率の高いプロセスのみ出力
        * top -i
  * 一定時間計測
    * pidstat -p (processID) 1 30 -l -r -u
      * 1秒間隔で30回計測し、平均を取得する例
      * -l: 実行コマンド名を引数付きで表示
      * -r: メモリ使用率計測
      * -u: cpu使用率計測
  * k8sの場合
    * kubectl top pod --containers
