# task warrior

## aptでインストール

- apt update -y && apt install -y taskwarrior

## 最新版インストール(通常は上記のapt installでOK)

- 最新のtaskwarriorインストール
  - aptでは最新版が提供されておらず、後述のtaskwarrior-tuiは最新版のtaskwarriorに依存しているため
  - 参考) https://taskwarrior.org/download/
  - 依存パッケージインストール
    - apt install -y gnutls-dev uuid-dev
  - リンク先からダウンロード
    - https://taskwarrior.org/download/
  - 解凍+デプロイ
    ```
    tar -zxvf -.tar.gz
    cd task-
    cmake -DCMAKE_BUILD_TYPE=release .
    make
    make install
    ```
  - 実行ファイルデプロイ
    ```
    cd src
    cp task /usr/bin/
    ```

## 関連ツールインストール

- (option)tasksh
  - apt install -y tasksh
    - 専用のshellが開き、頭の"task"の入力を省略可能に
  - taskwarrior-tui等あれば不要かもしれない
- (option)taskwarrior-tuiインストール
  - vim感覚で操作可能に
  - 参考) https://kdheepak.com/taskwarrior-tui/installation/
    - snapでインストールする方法もあるが、systemdに依存しているため、WSL2上では非推奨
      - 出来なくはないがWSL2が公式にsystemdをサポートしていないため不安定。2022/2時点ではやらない方が良い
  - リンク先から最新の.debをダウンロードしてインストール
    - https://github.com/kdheepak/taskwarrior-tui/releases/latest


## 関連コマンド

- review
  - 全タスクを1つずつレビューし、不要なものは削除
  - 定期実行推奨
- taskwarrior-tui
  - 拡張。vim感覚で操作可能
  - 詳細ガイド
    - https://kdheepak.com/taskwarrior-tui/quick_start/
  - ? でhelp表示

## オプション

- recurタスク
  - until:(指定日時)
    - 指定日時になるとrecurのinstanceタスクが消える
      - templateのdueも更新されず二度とinstanceタスクが生成されない
  - due:(指定日時)
    - 期限
    - 期限が来るとinstanceが完了しているか否かに関わらず、新instanceが作成される
      - 旧instanceが完了していなければinstanceは2つになる
  - wait:(指定日時)
  - templateタスク仕様
    - Due
      - 最後に迎えた期限っぽい
      - 恐らく最後に迎えた期限からrecurrence時間経過した時点でdueが伸びてinstanceタスクが再生成される
  - 下記で試すと分かりやすい
    - task add "test" recur:5s due:30s

## 良さそうなオプション指定

- 週次タスクを週末に登録
  - task add "weekly recur test4(recur:weekdays due:monday until:eod wait:monday)" recur:weekdays due:monday until:eod wait:monday
    - task allで見るとinstanceタスク出来ているが、1日の終わりにdeleteされる
- 帰宅時に処理する想定の週次タスクを週末に登録
  - task add "weekly recur test5(recur:weekdays due:monday until:eod wait:monday +goHome)" recur:weekdays due:monday until:eod wait:monday +goHome
    - task allで見るとinstanceタスク出来ているが、1日の終わりにdeleteされる
