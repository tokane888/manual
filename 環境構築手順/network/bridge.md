## bridge

### ubuntuをbridge化する手順

- sudo apt install -y bridge-utils
- /etc/netplan/bridge.ymlを作成し、下記記載
  - interface名は適宜変更

    ```
    root@edge-br-a-2:/etc/netplan# cat bridge.yaml
    network:
    version: 2
    renderer: networkd
    ethernets:
        enp1s0:
        dhcp4: no
        enx18ece795cd22:
        dhcp4: no
    bridges:
        br0:
        dhcp4: yes
        interfaces:
            - enp1s0
            - enx18ece795cd22
    ```
- パケット転送が有効化されていること確認

    ```
    root@edge-br-a-2:/etc/netplan# sysctl net.ipv4.ip_forward
    net.ipv4.ip_forward = 1
    ```
  - 有効化されていなければ/etc/sysctl.confいじって有効化
  - 