# auditd

* 監査ログツール
* 参考) https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/7/html/security_guide/chap-system_auditing

## 導入

* Ubuntu
  * apt install -y auditd

## 設定

* 関連ファイル
  * 設定ファイル
    * /etc/audit/auditd.conf
      * 主な設定項目
        * log_file: ログ出力先
        * max_log_file: ログファイルの最大サイズ(MB)
        * max_log_file_action: 最大ログファイル数到達時の動作。ROTATEなど
        * num_logs: ログファイルの個数
        * space_left: space_left_actionを行う空き容量(MB)。デフォルト75MB
        * space_left_action
          * ディスクの空き容量がspace_leftになった際に実行する動作
            * SYSLOG: syslogへwarning出力
            * EMAIL: action_mail_acctで指定のアカウントへ警告メール送信
        * admin_space_left: admin_space_left_actionを行う空き容量(MB)。デフォルト50MB
        * admin_space_left_action
          * ディスクの空き容量がadmin_space_leftになった際に実行する動作
            * SUSPEND: ログ書き込み停止
            * SINGLE: OSをシングルモードに変更
        * disk_full_action: 空き容量0になった際の動作
        * flush
          * ディスク書き込み方式
            * incremental_async: freqに設定した間隔で、ディスクに非同期に書き込み
        * freq: ディスク書き込み間隔
  * ルール定義
    * /etc/audit/audit.rules
      * 下記から自動生成される
        * /etc/audit/rules.d/*
      * 主なルール
        * -D: 全Auditルール削除
        * -b: カーネルにおける既存のAuditバッファの最大値
        * -f: 重大エラー発生時のアクション
          * 0: 何もしない
          * 1: printk（メッセージをカーネルコンソールへ出力）
          * 2: カーネルパニック起動
        * --backlog_wait_time
          * auditdの処理待ちログのqueueに空きがない場合のkernelログのauditdへの送信待ち時間？
        * -w ~~
          * 監査対象のファイル又はディレクトリ指定
          * 下記参照
            * https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/7/html/security_guide/sec-defining_audit_rules_and_controls#bh-defining_file_rules_with_auditctl
* ルール確認
  * auditctl -l
* root権限で実行されたコマンド全て記録
  * /etc/audit/audit.rules に下記記載
    ```
    -a exit,always -F arch=b64 -F euid=0 -S execve
    -a exit,always -F arch=b32 -F euid=0 -S execve
    ```
    * audit.rules.dから自動生成される旨の記載がある場合はaudit.rules.d配下編集
  * systemctl daemon-reload
  * systemctl restart auditd

## 処理の流れ

* カーネルコンポーネント
  * ユーザー空間アプリケーションからシステムコールを受ける
    * user, task, fstype, exitのいずれかのフィルターで振り分ける
  * システムコールがいずれかのフィルターを通過するとexcludeフィルターに掛けられる
    * excludeフィルターはAuditルール設定に基づいて、システムコールをAuditデーモンに送信して処理
* Auditデーモン
  * ユーザー空間にある
  * カーネルから情報を収集
  * ログファイルを作成
* Auditデーモン以外のユーザー空間ユーティリティ
  * Auditデーモン、カーネルAuditコンポーネント又はAuditログ・ファイルと対話
  * audisp
    * Auditディスパッチャーデーモン
      * Auditデーモンと対話
      * イベントを他のアプリケーションに送信して更に処理
    * プラグインを提供し、リアルタイム分析プログラムがAuditイベントと対話可能に
  * auditctl
    * Audit制御ユーティリティ
    * カーネルAuditコンポーネントと対話
    * イベント生成プロセスの多くの設定やパラメータを制御
  * 残りのAuditユーティリティ
    * Auditログファイルを入力として受け取る
    * ユーザーの要件に基づいて出力を生成
    * 例
      * aureportユーティリティ
        * 記録された全イベントのレポートを生成

## ログ

* /var/log/audit/audit.log
  * type: 監査ログのタイプ
  * pid: プロセス番号
  * (時刻が見ずらい場合は後述のausearch使用)
* syslogなどへの出力も可能
* ログ検索
  * 特定typeのログ検索
    * ausearch -m (type)
      * 例) ausearch -m USER_LOGIN
* timestampを時刻に変換、user idをuser名に変換
  * ausearch -i
* パース対象のログファイル指定
  * ausearch -i -if /var/log/audit/audit.log
* 時間範囲指定
  * 直近10分
    * ausearch -i -ts recent
  * 時刻
    * ausearch -i -ts 09:48:00 -te 09:50:00
  * 昨日から今日まで
    * ausearch -i -ts yesterday -te today
  * 今月
    * ausearch -i -ts this-month
* service関連
  * service開始
    * ausearch -i -m SERVICE_START
  * service終了
    * ausearch -i -m SERVICE_STOP
* 成功/失敗
  * 成功
    * ausearch -i -sv yes
  * 失敗
    * ausearch -i -sv no
* 最初にヒットした1件のみ表示
  * ausearch -i --just-one

## aureport

* コマンド実行実行ログ
  * aureport -x
* 集計
  * aureport --summary
  * 他のオプションと組み合わせ可能
    * 例) コマンド実行ログ集計
      * aureport -x --summary
  * 全ユーザーの失敗した操作
    * aureport -u --failed --summary -i
    