# jq

## バイナリ取得

* ver多分最新のやつ
  * wget 'http://stedolan.github.io/jq/download/linux64/jq'
  * chmod 755 jq
* ver見てdownloadする場合は下記参照
  * https://stedolan.github.io/jq/

## コマンド例

* json array要素数カウント
  * cat (file) | jq '.(配列型項目) | length'