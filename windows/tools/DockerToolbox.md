## DockerToolbox

### 遭遇した問題

* port forwardして、localhostへhttpリクエスト投げてもレスポンスが返されない
    * tool boxはlocalhostではなく、tool box起動時のコンソールに表示されるIPを使用している
        ```
        docker is configured to use the default machine with IP 192.168.99.101
        ```
        * 上記IPへリクエスト投げる
