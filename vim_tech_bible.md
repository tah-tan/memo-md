# Vim テクニック バイブル

読みながら参考になった所をメモ。

# キーマッピング

**mapとnoremap**

よく理解できてはいないが・・

- mapはキーシーケンスを展開した後、さらに別のマップを展開しようとする
- noremapは一度だけ展開する

んでもって、使い分けに迷った場合は、通常はnoremap系を使って、
プラグインが提供しているような< Plug \>を含むキーシーケンスへマッピングする場合は
map系を使えば、概ね問題ないようだ。

**マップ系のオプション**

- < silent > : コマンドラインへの表示を抑制する。キーマッピングからコマンドを実行する場合に指定する
- < unique > : すでにキーマッピングが存在する場合に、エラーにする。通常は上書きされる。
- < buffer > : バッファローカルなキーマッピングを定義する。
- < expr > : マップ先の文字列をVimの式とみなして、評価した結果の文字列をマップ先とする。

# autocommand

Vim内でイベントが発生した際にあらかじめ登録しておいたコマンドを実行する機能。
フォーマットは以下らしい。

    augroup {groupname}
		autocmd!
		autocmd {event} {pattern} {command}
	augroup END

イベントの一覧は、ヘルプのautocommand-eventsの項を参照。書き方の詳細についてはautocommandを参照。

**保存時の構文チェック**

    augroup rbsyntaxcheck
		autocmd!
		autocmd BufWrite *.rb w !ruby -c
	augroup END

**vimgrepコマンド実行後結果一覧を自動で開くようにする**

結果を見るためには普通:copenをしないといけないが、以下を定義しておくと自動でQuickFixウィンドウを開くようになる。

    augroup grepopen
		autocmd!
		autocmd QuickfixCmdPost vimgrep cw
	augroup END

**テンプレートを読み込む**

    augroup templateload
		autocmd!
		autocmd BufNewFile *.html 0r ~/.vim/skeleton.html
		autocmd BufNewFile *.pl 04 ~/.vim/skeleton.pl
	augroup END



# project.vim

プラグインの開始は、:Project。:Project some.vimprojects とすると、some.vimprojectsで管理しているプロジェクトを開く。

プロジェクトを初めて開始した時はプロジェクト管理ファイルに何も設定されていないので、\Cと押して、チュートリアルに従う。

- Enter the Name of the Entry：プロジェクトの名前を入力する
- Enter the Absolute Directory to Load：プロジェクトのトップディレクトリまでの絶対パスを指定する
- Enter the CD parameter：プロジェクト内のファイルを開いた時に、どのディレクトリに移動するか。「.」に移動するのがおすすめ。
- Enter the File Filter：プロジェクトに含めるファイルをワイルドカードで指定する。空白文字で分けて複数の条件を指定可能。
例：\*.txt \*.task　など。

ここまで読んでいる感じ、プロジェクトの機種・ブランチ毎にプロジェクトファイルを作るイメージだろうか。

- \r カーソル下のフォルダのファイルを再帰的に取り込みなおせる。ファイルに変更があった場合に使う。
- \R サブフォルダも含めてファイルを再帰的に取り込み直せる。ファイルに変更があった場合に使う。
- \g カーソル下のフォルダに対してgrep
- \G サブフォルダも含めてgrep

**プロジェクトのウィンドウを素早く開閉できるようにするためには**

    let g:proj_flags="imstc"		" ファイルが選択されたらウィンドウを閉じる
	nmap <silent> <Leader>P <Plug>ToggleProject	" <Leader>Pで、プロジェクトをトグルで開閉する
	nmap <silent> <Leader>p :Project<CR>		" <Leader>pで、デフォルトのプロジェクトを開く

# ヘルプのgrep検索

    :helpgrep 検索ワード

# モードをステータスライン色で区別

    au InsertEnter * hi StatusLine guifg=DarkBlue guibg=DarkYellow gui=none ctermfg=Blue ctermbg=Yellow   cterm=none
    au InsertLeave * hi StatusLine guifg=DarkBlue guibg=DarkGray   gui=none ctermfg=Blue ctermbg=DarkGray cterm=none

使ってたら気持ちよくなるのかな？あんまり効果は分からなかったけど・・


# VimFiler





