# python 

## 一般的な記法

* コーディング規約(PEP8)
  * https://pep8-ja.readthedocs.io/ja/latest/
  * pythonの標準ライブラリはPEP8に従っている
    * 基本的にpythonのソースはPEP8に従っているものが多い
* help
  * 下記のように各項目のhelp参照可能
    * help(json)

## 仮想環境

- 作成
  - python3 -m venv .venv
- 起動
  - source .venv
- 起動確認
  - `sys.prefix`と`sys.base_prefix`の値が異なっていれば仮想環境

  ```
  (.venv)  ~/learn/pytest/code git:(master) ✗ python3
  Python 3.10.12 (main, Mar 22 2024, 16:50:05) [GCC 11.4.0] on linux
  Type "help", "copyright", "credits" or "license" for more information.
  >>> import sys
  >>> print(sys.prefix)
  /home/tom/learn/pytest/code/.venv
  >>> print(sys.base_prefix)
  /usr
  ```
- 下記のような感じでextra_requiresにpytestとか開発に必要なパッケージ記載

  ```
  """Minimal setup file for tasks project."""

  from setuptools import find_packages, setup

  setup(
      name="tasks",
      version="0.1.0",
      license="proprietary",
      description="Minimal Project Task Management",
      author="Brian Okken",
      author_email="Please use pythontesting.net contact form.",
      url="https://pragprog.com/book/bopytest",
      packages=find_packages(where="src"),
      package_dir={"": "src"},
      install_requires=["click==7.1.2", "tinydb==3.15.1", "six"],
      extras_require={"mongo": "pymongo", "dev": ["pytest", "pytest-cov"]},
      entry_points={
          "console_scripts": [
              "tasks = tasks.cli:tasks_cli",
          ]
      },
  )
  ```

- 仮想環境内で下記のようにインストール
  - pip install -e ".[dev]"
