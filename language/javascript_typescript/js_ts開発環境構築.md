# js,ts開発環境構築

## Biome導入

- Biomeとは？
  - Prettier projectがパフォーマンス問題の解消に賞金を出した結果作成されたrust製の新linter, formatter
    - 参考: https://prettier.io/blog/2023/11/27/20k-bounty-was-claimed
- 導入手順
  - 参考
    - https://biomejs.dev/ja/guides/getting-started/#installation
  - 導入先のgit repository
    - package.json未作成の場合、下記で適当に作成
      - npm init
    - Biomeインストール
      - npm install --save-dev --save-exact @biomejs/biome
    - Biomeの設定ファイル(biome.json)生成
      - デフォルト設定に満足している場合は作成不要とのこと
      - npx @biomejs/biome init
  - vscode
    - Biome extensionインストール
    - .vscode/settings.jsonに下記記載でデフォルトのフォーマッターをbiomeに

      ```
      {
          "editor.formatOnSave": true,
          "[javascript]": {
            "editor.defaultFormatter": "biomejs.biome"
          },
          "[javascriptreact]": {
            "editor.defaultFormatter": "biomejs.biome"
          },
          "[typescript]": {
            "editor.defaultFormatter": "biomejs.biome"
          },
          "[typescriptreact]": {
            "editor.defaultFormatter": "biomejs.biome"
          }
        }

      ```

## vscodeにlive server導入

- 拡張機能で"liveserver"検索して導入
