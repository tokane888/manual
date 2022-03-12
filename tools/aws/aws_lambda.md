# aws lambda

* 外部api呼び出し許可
  * デフォルトの設定では不可
  * マニュアルに従ってnat gateway等作成することで可能に
    * https://aws.amazon.com/jp/premiumsupport/knowledge-center/internet-access-lambda-function/
    * 概要は下記
      * 前提
        * lambda関数作成済み
      * 手順
        * elastic ip作成
          * 固定ip
          * 手順は下記
            * https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html#eip-pricing
        * nat gateway作成
        * privateな方のsubnetにlambdaを接続
        * TODO: 詳細追記