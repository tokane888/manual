# mongoDB導入

## 検証環境

- Ubuntu 22.04

## setup手順

- CLI導入
  - curl -LO https://downloads.mongodb.com/compass/mongodb-mongosh_2.0.0_amd64.deb
  - sudo apt install -y ./mongodb-mongosh_2.0.0_amd64.deb
  - 試験的に接続(ブラウザからAtlas setup済み前提)
    - mongosh "mongodb+srv://cluster0.komxr.mongodb.net/" --apiVersion 1 --username (user名) --password (pass)
