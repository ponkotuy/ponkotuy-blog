# ponkotuy-blog

GitHub Pagesで公開するブログ記事用のリポジトリです。

## ローカルで表示確認する

Docker Composeを使うと、ローカルにRubyやBundlerを入れずにJekyllの表示確認ができます。

```sh
docker compose up
```

初回起動時は必要なgemをインストールするため、少し時間がかかります。起動後、ブラウザで次のURLを開きます。

```text
http://localhost:4000
```

ファイルを編集するとJekyllが自動で再ビルドします。終了するときはターミナルで `Ctrl-C` を押してください。

## Dark modeの確認

このサイトはOSやブラウザの `prefers-color-scheme: dark` に合わせてdark mode表示になります。

Chromeで確認する場合は、DevToolsの `Rendering` パネルから `Emulate CSS media feature prefers-color-scheme` を `dark` に変更すると、OS設定を変えずに確認できます。

## 生成物

ローカル確認時に `_site/` や `.jekyll-cache/` などが生成されます。これらはGit管理対象外です。
