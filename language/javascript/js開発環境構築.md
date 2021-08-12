# js開発環境構築

## eslint導入

* プロジェクトのディレクトリで下記実行
  * npm install --save-dev eslint
    * --save-dev: PJのみに導入
* このディレクトリの下記ファイルを当該PJディレクトリへコピー
  * .eslintrc.json
  * .prettierrc
* ブラウザ上に配置する.jsの場合は、.eslintrc.jsonでのenvで下記指定
  * "browser": true
* 下記のprettierについては動作未確認
  * prettier導入
    * npm install --save-dev prettier eslint-plugin-prettier
  * eslintのformatルール無効化
    * npm install --save-dev eslint-config-prettier

## eslintトラブルシュート

* rule一覧
  * https://eslint.org/docs/rules/
