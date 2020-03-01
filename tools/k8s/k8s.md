## minikube

* minikube cluster生成
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

### Deployment

  * Podは1つ以上のコンテナの集合
    * k8sの"Deployment"がpodのhealth checkを行い、自動的にpodを再起動
      * "Deployment"はpodの生成及びスケールを行う推奨の方法
  * Podの管理を行うDeployementを生成
    * 例) kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node
  * Deployment一覧取得
    * kubectl get deployments
  * Pod一覧取得
    * kubectl get pods
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