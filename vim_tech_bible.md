# Vim テクニック バイブル

## キーマッピング

### mapとnoremap

よく理解できてはいないが・・

- mapはキーシーケンスを展開した後、さらに別のマップを展開しようとする
- noremapは一度だけ展開する

んでもって、使い分けに迷った場合は、通常はnoremap系を使って、
プラグインが提供しているような<Plug>を含むキーシーケンスへマッピングする場合は
map系を使えば、概ね問題ないようだ。

### マップ系のオプション

- < silent > : コマンドラインへの表示を抑制する。キーマッピングからコマンドを実行する場合に指定する
- < unique > : すでにキーマッピングが存在する場合に、エラーにする。通常は上書きされる。
- < buffer > : バッファローカルなキーマッピングを定義する。
- < expr > : マップ先の文字列をVimの式とみなして、評価した結果の文字列をマップ先とする。

### autocommand

Vim内でイベントが発生した際にあらかじめ登録しておいたコマンドを実行する機能。

**定義**

    autocmd [group] {event} {at} [nested] {cmd}


