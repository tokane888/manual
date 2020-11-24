# gpg

## 導入

* apt install -y gnupg2

## 各種コマンド

* gpg鍵ペア生成
  * gpg2 --full-gen-key
    * 名前、メールアドレス、パス入力求められる
* 公開鍵一覧表示
  * gpg2 -k
* 秘密鍵一覧表示
  * gpg2 --list-secret-keys
* 公開鍵削除
  * gpg2 --delete-keys (鍵名)
    * 先に秘密鍵を削除する必要あり
* 秘密鍵削除
  * gpg2 --delete-secret-keys (鍵名)
* finger print確認
  * gpg2 --fingerprint
    * メールアドレス指定で検索も可能
      * gpg2 --fingerprint hoge@hoge.com
* 秘密鍵export
  * gpg2 --export-secret-keys --armor hoge@hoge.com > hoge.asc

## 鍵失行

* 失行証明書発行
  * gpg2 --output revoke.asc --gen-revoke (鍵名)
    * パス入力が必要
* 失効
  * gpg2 --import revoke.asc
    * gpg --fingerprint で実行結果確認可能
    ```
    # gpg --fingerprint
    /root/.gnupg/pubring.kbx
    ------------------------
    pub   rsa3072 2020-11-22 [SC] [失効: 2020-11-22]
          835D 9255 AEA6 6DE8 56B0  23EE 4D4D E795 3975 6C93
    uid           [  失効  ] hoge.com (hoge.com) <hoge@hoge.com>
    ```
  