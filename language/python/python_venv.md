# python venv

- python公式の仮想環境
  - python自体のversionは管理しない

## 使用方法

- 仮想環境作成
  - python3 -m venv .venv
    - .venv部分は正式には仮想環境名なので、他の名前でも良い
- 仮想環境アクティベート
  - . .venv/bin/activate
- インストール済みパッケージ書き出し
  - pip3 freeze > requirement.txt
- 書き出したインストール済みパッケージインストール
  - pip3 install -r requirement.txt
