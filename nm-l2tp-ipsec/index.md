# NetworkManagerでl2tp over IPsec
## なにこれ
LinuxのNetworkManager向けにl2tp over IPsecしたかったので色々弄ってみたら、ちょっとだけ面倒くさかったので忘備録。2021年3月のArchLinuxで動作確認。NetworkManagerのGUI内で完結したのでそのやり方を書く。(CUIは調べたらあるので調べて)

## 必要な情報
- サーバアドレス
- IPSecの共有キー
- VPNのユーザ名
- VPNのパスワード

## 必要なパッケージ
- networkmanager
- networkmanager-l2tp
- networkmanager-strongswan

## やりかた
NetworkManagerにはnm-connection-editorというのがあるので起動する。GNOME系のデスクトップ環境ならタスクバーのネットワークアイコンからVPN設定の追加があるのでそれでもいい。(どうせnm-connection-editorが起動する)

あとはnew connectionでl2tpタイプのVPNを追加し、VPNタブにVPN設定を書きつつ、IPsec Settingsに共有キーを書けばうごくはず。
