# home bridge

* 検証環境
  * raspberry pi 3B+
  * OS: raspbian buster
* 公式
  * https://github.com/OpenWonderLabs/homebridge-switchbot-ble

## 導入手順

* sudo npm install -g --unsafe-perm homebridge homebridge-config-ui-x
  * TODO: これがPermission denied でfailする原因調査
* sudo hb-service install
