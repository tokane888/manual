# fluentd

* 公式解説: https://docs.fluentd.org/

## 導入

### raspberry pi

* 動確環境
  * OS: raspbian OS buster
  * raspberry pi 3B+
* 参考
  * https://docs.fluentbit.io/manual/installation/linux/raspbian-raspberry-pi
* 手順
  * curl https://packages.fluentbit.io/fluentbit.key | sudo apt-key add -
  * /etc/apt/sources.list の末尾に下記追記
    * deb https://packages.fluentbit.io/raspbian/buster buster main
  * apt update -y
  * apt-get install td-agent-bit
  * systemctl enable td-agent-bit
    * ubuntuなどと異なりservice名がtd-agent-bit
  * systemctl start td-agent-bit
  * /var/log/syslogへcpu使用率等が出力される

## 概要

* install手順
  * ubuntuのver毎に若干変わるので公式の解説参照
    * https://docs.fluentd.org/installation/install-by-deb
* ライフサイクル
  * sourceタグで入力元Event設定
  * 下記はオプション
    * filterタグ
      * @grepなどで、ログ出力対象絞り込み
      * chain可能
  * matchタグで出力先指定
* Event構造
  * tag: event入力元。message routingで使用される
    * td-agentがpostリクエストを受ける場合、urlのパスがtagに相当
      * in_http
  * time
  * record
  * td-agent.confは上から下に順にデータが流れるようになっている
    * 可読性の問題が生じる場合はLabelを使用
* Label
  * td-agent.confで、上から下へデータ処理の流れを記述せず、link的なことを可能とする
  * source配下で@label指定で、データを渡す先のlabelを指定可能
* Buffers
  * s3などへログを送信する際に使用される
  * flush条件を満たした時点でデータ送信
* System
  * システム全体の設定
    * log_levelなど

## ログ出力

* td-agentのログファイルへ出力
  * td-agent起動、終了など関係ないログと混ざって出力される
```
<match myapp.access>
  @type stdout
</match>
```
* 任意のファイルへ出力
  * 下記の例だと、accessディレクトリの下にログファイル及びmetaファイルが出力される
```
<match myapp.access>
  @type file
  path /var/log/fluent/access
</match>
```
* Log Level
  * 下記6種類がある
    * fatal
    * error
    * warn
    * info
    * debug
    * trace
  * デフォルトはinfo(info, warn, error, fatalを出力)
  * global, pluginの2つのレイヤがある
    * globalはsystemタグで、pluginはsourceタグで指定
* flush
  * TODO: 下記のデフォルト値及び確認
    * flush_interval
    * total_limit_size
    * chunk_limit_size
* buffer
  * @type
    * オプション
    * 指定なしだとmemory buffer pluginが使用される？
  * bufferの引数で、eventをどのようにchunkに分けるか指定
    * 引数は省略可能
      * 省略時は単一のchunkになる

    ```
    <buffer (arg)>
    ```

## デバッグ

* .confファイルのフォーマット確認
  * td-agent --dry-run -c /etc/td-agent/td-agent.conf
    * 出力なしなら正常
* ログファイル末尾欠損時
  * 1行の最大文字数は2048なので、message_length_limitを設定