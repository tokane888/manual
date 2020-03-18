## Bridging方式でのOpenVPN接続

### 概要

* 2つの方法で認証
  * 各serverとclientが鍵ペアを持って認証
  * master認証局の証明書とkey
    * 各serverとclientの証明書で署名される
* serverとclientが双方向に認証
