# gorm

* go用のORマッパー
* ver1, ver2があり、互換性が内
  * 見分け方(import文の違い)
    * ver1
      * "github.com/jinzhu/gorm"
    * ver2
      * "gorm.io/gorm"
* インストール
  * go get -u gorm.io/gorm
  * DB毎のドライバ導入
    * postgresの場合
      * go get -u gorm.io/driver/postgres
    * 