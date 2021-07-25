# ラズパイへbluetoothでssh

* 注意) ペアリング以降は検証未完了
* 検証環境
  * 接続先
    * raspberry pi zero HW
    * raspbian OS buster
  * Windows10から上記へ接続

## 手順

* bluetoothctl
* bluetoothctl内で下記実行
  ```
  power on
  discoverable on
  ```
* windowsからペアリングを試みると、選択肢としてラズパイのhost名が表示されるので選択
* ペアリング成功するとラズパイ側で下記の出力が出るので確認
  ```
  [CHG] Device F0:1D:BC:94:DD:0F ServicesResolved: yes
  [CHG] Device F0:1D:BC:94:DD:0F Paired: yes
  [CHG] Device F0:1D:BC:94:DD:0F ServicesResolved: no
  [CHG] Device F0:1D:BC:94:DD:0F Connected: no
  ```
* apt install -y bluez-tools
* 下記設定記載
  * /etc/systemd/network/pan0.netdev
    ```
    [NetDev]
    Name=pan0
    Kind=bridge
    ```
* 下記設定記載
  * /etc/systemd/network/pan0.network
    ```
    [Match]
    Name=pan0

    [Network]
    Address=172.20.1.1/24
    DHCPServer=yes
    ```
    * Addressは任意
  * TODO: 下記参考に追記
    * https://memo.appri.me/iot/rpi0-bluetooth-ssh