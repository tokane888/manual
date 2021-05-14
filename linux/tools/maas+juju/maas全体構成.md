# maas全体構成

参考) https://maas.io/docs/snap/2.9/ui/maas-communication

* maasとの全通信はrack controller経由
* maasは内部的なDNSドメインを各subnet毎に自動で作成
  * 各subnetはIPを当該subnetに持つ全rack controllerを含む
  * PC起動時にsubnet DNS resourceがrack controllerの名前解決のために使用される
* rack controllerはbindをインストールし、forwarderとして使用
* maasはDHCPサーバも内包しており、PXE boot時にDHCPサーバからmachineのIPアドレスとブートローダのファイル名を取得
  * 参考) http://www.maruko2.com/mw/PXE%E3%83%96%E3%83%BC%E3%83%88%E7%94%A8%E3%82%B5%E3%83%BC%E3%83%90%E3%82%92%E6%A7%8B%E7%AF%89%E3%81%99%E3%82%8B
    * PXE bootの仕組みの説明。maasとは別

## 用語

* machine
  * OSデプロイ対象のPCのこと
  * boot後にcloud-init metadataをregion controllerからrackd経由で取得する
    * cloud-initはこのmetadataをmachineのresource情報収集に使用
      * 収集したデータはregiondへrackd経由で送信
        * 情報はregiondがpostgresに格納
          * postgresはresiondだけがアクセス可能
* rack controller(rackd)
  * machineを管理するサーバ
  * 下記の前提
    * machineへBMC accessが可能
      * PXE bootなどが可能という意味
  * DORA sequenceで、machineにIP割当て
    * machineの電源をonにするrack controllerと同一である必要はない
  * region controllerへRPCでkernel bootに必要な資材を取得させる
    * RPC responseはPXE configへ変換され、machineへ取得市に来るよう通知される
* region controller(regiond)
  * rack controllerを管理するサーバ
  * rack controllerをデプロイ
* MAAS
  * リクエストを受ける
    * Web UI, API request両方
  * 受けたリクエストをAPI requestに変換してregion controllerへ送信
* 参考) 全体構成図
  * https://discourse.maas.io/uploads/default/original/1X/02a7ca58b989c67c74421b9d5e0c8b32907a2de1.jpeg
* ネットワーク関連
  * 参考) https://maas.io/docs/snap/2.9/ui/installation#heading--spaces-fabrics-zones-and-subnets  
  * space
    * subnetの集合
    * 同じspaceに属していれば、直接ネットワーク的に繋がっていなくても通信可能
  * zone
    * 個別のnodeをグループ化するad-hocなnodeの集合
  * fabric
    * スイッチの集合？
    