# go library

## config系

* viper
  * env, json, toml等各種設定ファイルを読み取り
    * envについては読み取り処理の記載が冗長になってしまうため、2022/3現在は他のlib使った方が良さそう
      * 例) https://gitlab.com/home_life_management/light_management_morning/-/blob/main/config/config.go#L42
* default tagの値をstructにマップ
  * 一番単純なのは、jsonなどをunmarshalする前に自分でstructの各項目にdefault値を設定すること
    * ただ下記の欠点があるのでlib導入もあり
      * struct定義をひと目見てdefault値が分からないの
      * 複雑な構造のmappingは、記載がややこしい
  * creasty/defaults
    * starは多分一番多い
    * slice型等の複雑な構造のmapもサポート
    * https://github.com/creasty/defaults
  * go-defaults
    * 下記のようなdefaultタグをstructの項目にmapするライブラリ
      * `default:"hoge"`
    * slice型のmapがgo mod tidyで導入可能な最新版でサポートされていない
      * ソース上は1.2.0があるが、もう開発する気ないっぽい
    * https://github.com/mcuadros/go-defaults/tree/v1.2.0
