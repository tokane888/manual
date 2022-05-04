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
          * nat gatewayは起動しているだけで料金かかるので注意
        * privateな方のsubnetにlambdaを接続
        * 長いのであとはマニュアル見て作業。。
* 外部api呼び出しのコスト最適化
  * nat gatewayを使用する方式は金がかかる
    * nat gatewayは起動しているだけで料金かかるため
  * natインスタンスに置き換える方式がある？
* 調査
  * natとnat gatewayの比較
    * amazonはnat gateway推奨
    * nat gateway
      * メリット
        * 高可用性
        * 帯域
        * 管理の手間がかからない
      * デメリット
        * 金がかかる
    * 今からnat instance使うのは非推奨+結局内部的にec2立てるので金かかる
  * 非VPC設定のlambdaを使えば良い
    * nat不要なので
    * lambdaの設定画面から下記で非VPCに変更可能
      * 設定 => VPC => 編集 => VPCから"なし"を選択