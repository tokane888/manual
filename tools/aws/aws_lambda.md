# aws lambda

* 外部api呼び出し許可
  * VPC環境ではデフォルトの設定では不可
    * 非VPC環境であれば特殊な設定なしで可能
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
  * natインスタンスに置き換える方式はあるが、amazonがnat gateway推奨しているため、基本使用しない
  * 非VPCのlambdaを使用する方式が良い
    * lambdaの設定画面から下記で非VPCに変更可能
      * 設定 => VPC => 編集 => VPCから"なし"を選択
* timeout
  * lambdaはデフォルト3秒でtimeoutする
    * lambda画面で"設定" => "一般設定" => "編集"からタイムアウト編集可能