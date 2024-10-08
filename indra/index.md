# なにこれ

indra という Mastodon API を叩く CLI ツールを作りました。

[GitHub indra](https://github.com/ponkotuy/indra/)

最近大量にアカウントを作ってぼくをフォローしてくる悪いユーザがいて、いい加減鬱陶しいので、MastodonAPI の勉強もかねて、自動で block するツールを作ってみました。

# 使い方

[indra Releases](https://github.com/ponkotuy/indra/releases/)

こちらから対応するバイナリ（Linux(x86_64)/mac(ARM)/Windows(x86_64)に対応しています）をダウンロードし、実行するだけです。あ、コマンドラインツールなので黒い画面からどうぞ。

初回実行時に接続先のサーバを指定してログイン処理とかやる必要があります。適宜キャッシュするので 2 度目は要りません。

とりあえず help

```
$ ./indra --help
```

コマンドグループを指定して help とかすれば詳細が分かるわけですが、arrow コマンドだけ解説すると、

```
$ ./indra arrow
```

インドラの矢を放ちます。嘘です。ブロック候補をリストアップし、表示します。

誤爆がなさそうなら実際に適用します。

```
$ ./indra arrow apply
```

簡単ですね。

# 仕組み

## アーキテクチャ

deno + TypeScript で作られています。コマンドラインまわりは Cliffy という Framework を使っていて、それなりに便利です。MastodonAPI へは普通に fetch で接続します。cache まわりは XDG Base Directory に則って保存しています。実は知らなかったんですけど結構ポータビリティの高いアプリ作るには便利な気がします。

deno が主要 OS 向けの 1 バイナリに対応しているので、その仕組みの上で 1 バイナリを実現しています。Release をすると GitHub Actions が動いてアセットに zip したバイナリを登録します。全部無料でいけます。便利。

## ブロックユーザの特定

最初は引数で細かく制御できるようしようとしたんですが、漏れなくブロックしようとするとそれなりに複雑になるため、コードを書き換える方式になりました。雑！

[https://github.com/ponkotuy/indra/blob/f463f7fb2bbf1635bad41e1b60724dcd4872925c/command/arrow.ts#L12-L23](https://github.com/ponkotuy/indra/blob/f463f7fb2bbf1635bad41e1b60724dcd4872925c/command/arrow.ts#L12-L23)

正規表現のデフォルトフィルターに加えて username フィルター（1 文字 username と特定 username）のどれかに引っ掛かったら Block 対象になります。

なんかのスクリプトで制御できるようにしたいですが、今後の課題ですかね…。

## 通報

自動通報は誤爆したときに管理者へのダメージと自分への風評被害がヤバいので今のところ自動で通報できる予定はありません。
