## Docker in Docker

* 参考
  * 公式) https://hub.docker.com/_/docker
  * 公式推奨ブログ) https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/

### 概要

* そもそもDocker in Dockerは非推奨
* Docker in Dockerの第1の使用目的はDocker自体の開発
* CI又はテスト環境のために使用されることが多い
  * 問題が多発するが、多くはDocker socketをJenkins containerへ接続することで回避可能？とのこと

### 欠点 

* --privileged フラグ有効時
  * LSM(Linux Security Modules)
    * コンテナ内dockerが外のdockerと競合するsecurity policyを適用しようとする場合がある
* file system
  * 外のdockerは通常のfile system(EXT4, BTRFS等)上で動作
  * コンテナ内dockerはcopy-on-write system(AUFS, BTRFS, Device Mapper等)上で動作
  * コンテナ内と外のfile systemの組み合わせによっては動作しない場合がある
* コンテナの内外でdocker imageを共有できない
  * /var/lib/dockerは元々1つのdocker daemon以外は触らない想定で設計されている

### 欠点への対応方法

* コンテナ上のCIからdockerコンテナを操作したい場合は、外にdockerコンテナを作ってDocker socketを-vでマウント
