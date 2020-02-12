## dockerアクセス失敗時チェックリスト

curlでAPI叩いて下記のようなエラーが返された場合の対応
Failed to connect to localhost port 8081

### 確認項目

* コンテナ内でAPIが叩けるか
  * docker exec -it (コンテナ名) bash
* DockerfileにExpose命令が入っているか
* docker run時に-p指定が入っているか
  * -p (コンテナ外port):(コンテナ内port)