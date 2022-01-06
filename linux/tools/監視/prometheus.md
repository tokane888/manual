# Prometheus

## 検証環境

* raspberri pi 3B+
* OS: raspbian OS buster

## インストール

* 参考) https://prometheus.io/docs/prometheus/latest/getting_started/
* 下記から対象環境向けの.tar.gzダウンロード
  * https://prometheus.io/download#prometheus
    * アーキはプルダウンから選択
  * curl -LO https://github.com/prometheus/prometheus/releases/download/v2.32.1/prometheus-2.32.1.linux-armv7.tar.gz
* 解凍
  * tar -zxvf prometheus-2.32.1.linux-armv7.tar.gz
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
    * 例) curl -LO https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-armv7.tar.gz
* 解凍
  * tar -zxvf node_exporter-*
* cd node_exporter-1.3.1.linux-armv7
* 