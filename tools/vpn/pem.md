## pem

* 証明書になることも鍵になることもある
  * ファイル形式が決まった入れ物
    * 証明書、鍵を複数含めることが可能
    * 例
      ```
      -----BEGIN RSA PRIVATE KEY-----
      鍵ファイルのｂase64エンコード
      -----END RSA PRIVATE KEY-----
      -----BEGIN CERTIFICATE-----
      証明書ファイルのｂase64エンコード
      -----END CERTIFICATE-----
      ```

### 内容確認

* opensslインストール
  * apt install -y openssl