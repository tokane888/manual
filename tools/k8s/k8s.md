# k8s

## 概要

* 各種まとまりの関係
  * コンテナ < pod < namespace < (node, context) < cluster
    * nodeは物理PCに近い
    * contextは要調査
* 構築ツール例
  * Kubeadm
  * Rancher

###  用語

* service
  * pod間の通信を容易にするためにdnsラベル発行する仕組み
  * 4種類ある
    * clusterIP
      * cluster内でpodが使用可能なIP
      * k8sの外からはアクセス不可
    * NodePort
      * 外部に公開されるport
    * LoadBalancer
      * NodePortを更に拡張したもの
      * upstreamとして外部サーバと通信可能
    * ExternalName
      * 外部サービスに公開する名前
* node
  * 2種類ある
    * k8s master
      * apiエンドポイント提供
        * RESTful APIとして実装されている
        * 各言語, curl等から操作することも可能
      * コンテナスケジューリング
      * コンテナスケーリング
      * kubectlは.yaml形式のマニフェストファイルの情報を元にk8s masterのAPIへリクエストを送信してリソースを登録し、k8sを操作
        * リソースについては後述
    * k8s node
      * コンテナが起動するノード
* リソース
  * k8sを操作するために登録
  * 5種類のカテゴリがある
    * Workloads API
    * Service API
    * Config & Storage API
    * Cluster API
    * Metadata API
* Workloads API
  * クラスタ上にコンテナを起動させるために利用するリソース
  * 内部的なものを除いて全8種類のWorkloads APIリソースがある
    * Pod
    * ReplicationController
    * ReplicaSet
    * Deployment
    * DaemonSet
    * StatefulSet
    * Job
    * CronJob
* Service API
  * コンテナのserviceディスカバリ、クラスタ外部からアクセス可能なエンドポイント提供などを行う
  * 内部的なものを除き、利用者が直接操作するのはServiceとIngressの2種類のカテゴリがある
    * Service
      * ClusterIP
      * ExternalIP
      * NodePort
      * LoadBalancer
      * Headless
      * ExternalName
      * None-Selector
    * Ingress
* Config & Storage API
  * 設定、機密情報をコンテナに埋め込む、永続ボリュームの提供等を行うリソース
  * key-valueのデータ構造
    * 保存するデータが機密情報なのか設定情報なのかよって使い分け
  * PersistentVolumeClaimはコンテナから永続ボリュームを要求する際に使用
* Cluster API
  * クラスタ自体の振る舞いを定義するリソース
  * セキュリティ周りの設定、ポリシー、クラスタ管理等
  * 下記が含まれる
    * Node
    * Namespace
    * PersistentVolume
    * ResourceQuota
    * ServiceAccount
    * Role
    * ClusterRole
    * RoleBinding
    * ClusterRoleBinding
    * NetworkPolicy
* Metadata API
  * クラスタ内の他のリソースの動作を制御するためのリソース
  * 下記が含まれる
    * LimitRange
    * HorizontalPodAutoScaler
    * PodDisruptionBudget
    * CustomResourceDefinition
* Namespace
  * 仮想的なk8sクラスタ分離機能
  * 初期は下記4つのnamespaceが作成されている
    * kube-system
    * kube-public
      * 全ユーザーが共通して利用する設定値等を保存
    * kube-node-lease
      * k8s nodeのハートビート情報を保存
      * クラスタ管理者以外は気にしなくて良い
    * default

## minikube

* ローカルの仮想マシン上にk8sをインストールし、シングルノード構成で使用
  * k8sの場合dockerコンテナを複数起動し、各コンテナをk8s nodeとして使用
    * 通常localでマルチノードクラスタを構成する場合はkindを使えば良い
    * kindは元々k8s自体の開発のために作成されたものなので高機能だが、通常はminikubeで十分
* minikube cluster生成
  * root以外で下記実行
    * minikube start
        * minikube start --vm-driver=none
          * driverは入れた方が良いらしいが、これでも最低限動く
          * driverについては下記参照とのこと
            * https://minikube.sigs.k8s.io/docs/reference/drivers/none/
  * 立ち上がってすぐ終了するが、エラーが出ていなければ問題ない
* ブラウザ上でkubernetes dashboard閲覧可能に
  * minikube dashboard
    * コンソールは占拠される
    * Deployment, Pod, Service等の一覧
    * TODO: 他のPCからはアクセス出来なかった。portは開放されており、要調査
      * port開放はss -l で確認済み

### 関連コマンド

  * Podは1つ以上のコンテナの集合
    * k8sの"Deployment"がpodのhealth checkを行い、自動的にpodを再起動
      * "Deployment"はpodの生成及びスケールを行う推奨の方法
  * Podの管理を行うDeployementを生成
    * 例) kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node
  * Deployment一覧取得
    * kubectl get deployments
  * Pod一覧取得
    * kubectl get pods
  * cluster一覧表示
      * kubectl config get-clusters
  * cluster event取得
    * kubectl get events
      ```
      LAST SEEN   TYPE     REASON                    OBJECT                             MESSAGE
      4m24s       Normal   Scheduled                 pod/hello-node-7676b5fb8d-qh972    Successfully assigned default/hello-node-7676b5fb8d-qh972 to tom-pc-lz550nsb4m23s       Normal   Pulling                   pod/hello-node-7676b5fb8d-qh972    Pulling image "gcr.io/hello-minikube-zero-install/hello-node"
      3m27s       Normal   Pulled                    pod/hello-node-7676b5fb8d-qh972    Successfully pulled image "gcr.io/hello-minikube-zero-install/hello-node"
      ...
      ```
  * kubectl設定確認
    * kubectl config view
      ```
      apiVersion: v1
      clusters:
      - cluster:
          certificate-authority: /root/.minikube/ca.crt
          server: https://192.168.11.100:8443
          ...
      ```
* node一覧取得
  * kubectl get nodes
* kubectlの接続先一覧表示
  * kubectl config get-contexts
* kubectlの接続先切り替え
  * kubectl config use-context minikube
  * kubectl config use-context docker-desktop
* kubectlの現在の接続先表示
  * kubectl config current-context

### service生成

* デフォルトでは、Podはk8s clusterの内部IPでしかアクセスできない
  * 外部からアクセス可能にするには、Podをk8s serviceをしてexposeする必要がある
* 公開
  * kubectl expose deployment (deployment名) --type=LoadBalancer --port=(port番号)
    * 例) kubectl expose deployment hello-node --type=LoadBalancer --port=8080
* service確認
  * kubectl get services
    ```
    NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
    hello-node   LoadBalancer   10.106.78.253   <pending>     8080:30070/TCP   2m7s
    kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          53m
    ```
  * load balancerをサポートするcloudプロバイダでは、外部IPがserviceに割り当てられる
  * MinikubeではLoadBalancer typeによってminikube serviceコマンドを通してアクセス可能になる
* service起動
  * minikube service hello-node
    ```
    |-----------|------------|-------------|-----------------------------|
    | NAMESPACE |    NAME    | TARGET PORT |             URL             |
    |-----------|------------|-------------|-----------------------------|
    | default   | hello-node |             | http://192.168.11.100:30070 |
    |-----------|------------|-------------|-----------------------------|
    ...
    xdg-open: no method available for opening 'http://192.168.11.100:30070'

    �  open url failed: http://192.168.11.100:30070: exit status 3

    �  minikube is exiting due to an error. If the above message is not useful, open an issue:
    �  https://github.com/kubernetes/minikube/issues/new/choose
    ...
    ```
    * sshから実行すると上記のようにエラーが出るが、curlで叩くとレスポンスが帰ってくる
      * 実機で実行したら出力が何もなかった。。
        * TODO: 調査
  * 動確
    ```
    # curl http://192.168.11.100:30070
    Hello World!
    ```

### addon

* Minikubeのaddon一覧表示
  * minikube addons list
    ```
    |-----------------------------|----------|--------------|
    |         ADDON NAME          | PROFILE  |    STATUS    |
    |-----------------------------|----------|--------------|
    | dashboard                   | minikube | enabled ✅   |
    | default-storageclass        | minikube | enabled ✅   |
    | efk                         | minikube | disabled     |
    ...
    ```
* addon有効化
  * minikube addons enable (addon名)
* addon無効化
  * minikube addons disable (addon名)
* 生成したPodとserviceを表示
  * kubectl get pod,svc -n kube-system
    ```
    NAME                                          READY   STATUS    RESTARTS   AGE
    pod/coredns-6955765f44-8qlww                  1/1     Running   0          112m
    pod/coredns-6955765f44-ll6q5                  1/1     Running   0          112m
    pod/etcd-tom-pc-lz550nsb                      1/1     Running   0          112m
    pod/kube-apiserver-tom-pc-lz550nsb            1/1     Running   0          112m
    pod/kube-controller-manager-tom-pc-lz550nsb   1/1     Running   0          112m
    pod/kube-proxy-mtrgc                          1/1     Running   0          112m
    pod/kube-scheduler-tom-pc-lz550nsb            1/1     Running   0          112m
    pod/metrics-server-6754dbc9df-d8kvw           1/1     Running   0          34s
    pod/storage-provisioner                       1/1     Running   0          112m

    NAME                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)                  AGE
    service/kube-dns         ClusterIP   10.96.0.10     <none>        53/UDP,53/TCP,9153/TCP   112m
    service/metrics-server   ClusterIP   10.108.19.55   <none>        443/TCP                  34s
    ```

### cleanup

* service削除
  * kubectl delete service (service名)
* deployment削除
  * kubectl delete deployment (deployment名)
* (通常は不要) Minikube Virtual Machine停止
  * minikube stop
* (通常は不要) Minikube Virtual Machine削除
  * minikube delete

## kubectl

* 設定ファイル  
  * ~/.kube/config
    * kubectlコマンドで設定変更すると上記設定ファイルも自動で変更される

## マニフェスト

* 参考) kubernetes完全ガイド第2版
* リソース作成
  * kubectl create -f ***.yaml
    * リソース作成にも後述のapplyが使用可能で有り、そちらを使うべき
      * CI/CDなどでの使い分けが不要になる
      * apply時に差分が検証しきれないことがあるため
        * 
* リソース削除
  * kubectl delete -f ***.yaml
* リソース更新
  * kubectl apply -f ***.yaml
    * yamlに変更差分がある場合には変更し、差分がない場合には何もしない
    * createで作成したリソースへの初回applyの際には下記の警告が出る場合があるが、特に影響はない
      ```
      $ kubectl apply -f sample-pod.yaml
      Warning: resource pods/sample-pod is missing the kubectl.kubernetes.io/last-applied-configuration annotation which is required by kubectl apply. kubectl apply should only be used on resources created declaratively by either kubectl create --save-config or kubectl apply. The missing annotation will be patched automatically.
      ```
      * 過去の設定(manifest)は現在の設定(manifest)の"annotation"に保存される
        * createで新規作成するとannotationの無い設定が作成される
      * kubectl createに--save-configを付けると、現在の状態の設定をannotationに保存する
      * リソース作成時もapplyを使用しておけばこの警告は出ない
* podの設定情報をyamlで出力
  * kubectl get pod (pod名) -o yaml
* 特定リソースの全pod restart
  * 例) kubectl rollout restart deployment sample-deployment
    * 上記の例ではリソース名sample-deploymentとしている。".yaml"は不要
    * podに対してはrestartは行えない
* 指定する状態になるまで待機
  * kubectl wait --for=condition=Ready pod/sample-pod --timeout=30s