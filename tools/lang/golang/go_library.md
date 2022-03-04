# go library

## config系

* viper
  * env, json, toml等各種設定ファイルを読み取り
    * envについては読み取り処理の記載が冗長になってしまうため、2022/3現在は他のlib使った方が良さそう
      * 例) https://gitlab.com/home_life_management/light_management_morning/-/blob/main/config/config.go#L42