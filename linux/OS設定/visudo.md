# visudo

* 各ユーザーの権限設定
* sudo時のパスワード確認
  * 有効化
    * "[user名] ALL=(ALL) PASSWD: ALL"
      * 例) tom ALL=(ALL) PASSWD: ALL
  * 無効化
    * "[user名] ALL=(ALL) NOPASSWD: ALL"
      * 例) tom ALL=(ALL) NOPASSWD: ALL
      