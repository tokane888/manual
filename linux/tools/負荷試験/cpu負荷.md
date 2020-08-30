## cpu負荷試験

### ツール

* stress
  * 導入
    * Ubuntuの場合
      * sudo apt install -y stress
    * Centの場合(未動確)
      * epelリポジトリ追加していなければ追加
        * yum -y install epel-release
      * sudo yum -y install stress
  * 使用方法
    * stress -c (プロセス数) &
  * 使用終了方法
    * kill (stressプロセスのPID)
      * 何故か"-c 1"指定でも2プロセス作成される。1つは管理用で負荷をかけていない？

### 計測

* cpu使用率
  * top
    * 3行目の"id"が全コア平均のアイドル時間の割合
      * 下記でcpu使用率計算可能
        * cpu使用率 = (100-アイドル時間)%
    * TODO: コア毎のcpu使用率取得
  * pidstat -p (process id) (計測間隔) (計測回数) -l -r -u
    * -p: プロセスID
    * -l: コマンド名を引数付きで表示
    * -r: ページフォールトとメモリの使用率を表示する
    * -u: CPU使用率を表示
    * 例) pidstat -p 7000 1 30 -l -r -u
      * 1秒間隔で30回プロセスID 7000番のプロセスのCPU, メモリ使用率を計測し、平均を出す
* cpu情報取得
  * cat /proc/cpuinfo