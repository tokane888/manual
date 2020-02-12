## dockerアクセス失敗時チェックリスト

curlでAPI叩いて下記のようなエラーが返された場合の対応
Failed to connect to localhost port 8081

### 確認項目

1. コンテナ内でAPIが叩けるか
    * docker exec -it (コンテナ名) bash
2. コンテナに割り振られているIPを確認
    * docker inspect (コンテナID)
    * これでcurlが通れば、下記のいずれかの問題
3. DockerfileにExpose命令が入っているか
4. docker run時に-p指定が入っているか
    * -p (コンテナ外port):(コンテナ内port)

[参考](https://qiita.com/amuyikam/items/01a8c16e3ddbcc734a46)