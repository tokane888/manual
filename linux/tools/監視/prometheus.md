# Prometheus

## 検証環境

* raspberri pi 3B+
* OS: raspbian OS buster

## インストール

* 参考) https://prometheus.io/docs/prometheus/latest/getting_started/
* 下記から対象環境向けの.tar.gzダウンロード
  * https://prometheus.io/download#prometheus
    * アーキはプルダウンから選択
  * curl https://github.com/prometheus/prometheus/releases/download/v2.44.0/prometheus-2.44.0.linux-amd64.tar.gz | tar -zxv
* cd prometheus-*
* 自身を監視するよう設定
  * prometheus.ymlとして下記のページの設定を保存
    * https://prometheus.io/docs/prometheus/latest/getting_started/#configuring-prometheus-to-monitor-itself
  * 元々サンプルとして同等のものが解凍したディレクトリにある
* 試験的に起動
  * ./prometheus --config.file=prometheus.yml
* ブラウザから動作確認
  * http://(prometheusのIP):9090/

## node exporter導入

* 試験的に導入して手順
  * 必須かどうかは現状不明
* 参考 ) https://prometheus.io/docs/guides/node-exporter/
* downloadページからダウンロード
  * https://prometheus.io/download/#node_exporter
    * 例) curl -L https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz | tar -zxv
* cd node_exporter-1.3.1.linux-armv7

## alert manager導入

* 下記からダウンロード
  * https://prometheus.io/download/#alertmanager
* 標準設定だとalertの最低連続発報期間が長い
  * 関連設定項目
    * 参考) intervalの種類の違いば分かりやすい
      * https://www.techscore.com/blog/2017/12/07/prometheus-monitoring-setting/
    * 参考) 下記でintervalで検索
      * https://prometheus.io/docs/alerting/latest/configuration/
    * group_wait
      * 同種のalertを1つのグループとしてまとめる期間
      * default: 30s
    * group_interval
      * 同種のalertが発生したときに再度発報するまでの期間
      * default: 5m
    * repeat_interval
      * 正常に発報されたalertが再度通知されるまでの期間
      * default: 4h
  
## インストルメンテーション

* カウンタ
  * イベントの数又はサイズを追跡
  * 