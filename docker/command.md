## dockerコマンド

### コンテナ掃除

* docker system prune -f
    * 停止しているコンテナ削除
    * dangling image削除
        * dangling image: docker build時に生成されるimage名`<none>`のimage
* docker system prune -af
    * 停止しているコンテナ削除
    * 使用されていないimage削除