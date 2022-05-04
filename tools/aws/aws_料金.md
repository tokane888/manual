# aws料金

* lambdaの予期しない挙動などで想定の10倍以上の料金が発生する場合がある

## 共通対策

* アラート参考マニュアル
  * https://docs.aws.amazon.com/ja_jp/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html
* 請求アラート設定
  * billing dashboardへ
    * https://console.aws.amazon.com/billing/home?#/
  * 左メニューから"請求設定"
  * "請求アラートを受け取る"押下
    * デフォルトで有効になっていた
  * "詳細設定を保存"押下
  * 初めて上記の設定を行った場合、下記の請求アラーム設定がが可能になるまで15分程度かかる
* 請求アラーム作成
  * cloud watchコンソールへ
    * https://console.aws.amazon.com/cloudwatch/
  * 必要に応じてリージョン変更
    * 当該リージョンに請求メトリクスデータが保存され、世界全体の請求額を示す
  * "アラーム" => "すべてのアラーム" => "アラームの作成"
  * "メトリクスの選択"
  * "請求"、"概算合計請求額"選択
    * 注) 請求アラートを初めて設定した場合、上記の選択項目が表示されるまで15分程度かかる