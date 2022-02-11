# task warrior

## 関連ツール含めてインストール

* 最新のtaskwarriorインストール
  * aptでは最新版が提供されておらず、後述のtaskwarrior-tuiは最新版のtaskwarriorに依存しているため
  * 参考) https://taskwarrior.org/download/
  * 依存パッケージインストール
    * apt install -y gnutls-dev uuid-dev
  * リンク先からダウンロード
    * https://taskwarrior.org/download/
  * 解凍+デプロイ
  ```
  tar -zxvf *.tar.gz
  cd task*
  cmake -DCMAKE_BUILD_TYPE=release .
  make
  make install
  ```
  * 実行ファイルデプロイ
    * 何故か下記で動いたが、正直2箇所に配置する意味が分からない(2箇所に配置しないと実行時エラー)
      ```
      cd src
      cp task /usr/local/bin/
      cp task /usr/bin/
      ```
* (option)tasksh
  * apt install -y tasksh
    * 専用のshellが開き、頭の"task"の入力を省略可能に
  * taskwarrior-tui等あれば不要かもしれない
* (option)taskwarrior-tuiインストール
  * vim感覚で操作可能に
  * 参考) https://kdheepak.com/taskwarrior-tui/installation/
    * snapでインストールする方法もあるが、systemdに依存しているため、WSL2上では非推奨
      * 出来なくはないがWSL2が公式にsystemdをサポートしていないため不安定。2022/2時点ではやらない方が良い
  * リンク先から最新の.debをダウンロードしてインストール
    * https://github.com/kdheepak/taskwarrior-tui/releases/latest


## 関連コマンド

* review
  * 全タスクを1つずつレビューし、不要なものは削除
  * 定期実行推奨
* taskwarrior-tui
  * 拡張。vim感覚で操作可能
  * 詳細ガイド
    * https://kdheepak.com/taskwarrior-tui/quick_start/
  * ? でhelp表示