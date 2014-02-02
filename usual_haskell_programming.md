# ふつうのHaskellプログラミング

## はじめよう

### Haskellのメリット

- 関数を値として渡せるので柔軟
- コンパイル時の値チェックが協力
- 再代入がないので読みやすい
- 遅延評価がある

## 関数とリスト

### hello.hs

    main = putStrLn "Hello, world!"

mainは関数ではなく、変数（アクション）。右辺は値。
なので上記は、「mainはputStrLn "Hello, world!"である」、と言っている。

putStrLnは文字列を1つ受け取って、その文字列と改行を出力するアクションを返す関数。
ここで、

    putStrLn "Hello world!"

を、Haskellでは「"Hello world!"に、putStrLn関数を適用(apply)する」という。

### catコマンド

**ソース**

    main = do cs <- getContents
              putStr cs

**Tips**

- getContentsは、標準入力を全て読み込む。
- putStrは文字列を標準出力にそのまま出力する。putStrLnと違って末尾に改行はつかない
- 上記レイアウトでは、doの後のcsとputStrのそれぞれの式の桁が揃っている事に意味がある。
これは、「cs <- getContents」と「putStr cs」の2つの式を揃えている。
do式は複数の式を束ねる構文（Javaのブロック構文に相当）。
- <- は、アクションの結果を得る構文。cs <- getContentsで、getContentsアクションで入力した文字列が
変数csに結び付けられる。このように変数と値を結びつける事を、「変数を値に束縛する(bind)」という。

また、

    main = putStr getContents

とするとエラーになる。putStrは引数に文字列を要求するのに対して、getContentsの値はアクションのため。

**ポイント**

- レイアウト（インデント）が意味を持つ
- アクションの実行順序を指定したい場合はdo式を使う

### countlineコマンド

**ソース**

    main = do cs <- getContents
	          print $ length $ lines cs

カッコを使った書き方

    main = do cs <- getContents
	          print (length (lines cs))

**Tips**

- Haskellでは文字列もリスト
- $は式を区切るために使われる。$の右側にある lines cs は、ひとまとまりで左側にある length の引数となる。
- linesは、文字列を行のリストに分解する。
- lengthは、リストの長さを返す。
- printは、値を文字化して標準出力に出力するアクションを返す。

### 変数名の慣習

- 整数に束縛される変数がn
- 整数のリストに束縛される変数はns
- 文字に束縛される変数はc
- 文字列に束縛される変数はcs

### headコマンド

**ソース**

    main = do cs <- getContents
	          putStr $ firstNLines 10 cs
	
	firstNLines n cs = unlines $ take n $ lines cs

**Tips**

- headコマンドはファイルの数行を出力する
- firstNLinesは、4行目に定義してある関数(自作)
- unlinesは、linesの逆。行のリストを文字列に結合する。
- takeは、リスト先頭からn要素をとってリストで返す。

関数定義は以下のように書かれる。

    関数名 仮引数1 仮引数2 ... = 本体

### tailコマンド

**ソース**

    main = do cs <- getContents
	          putStr $ lastNLines 10 cs
	
	lastNLines n cs = unlines $ takeLast n $ lines cs

	takeLast n ss = reverse $ take n $ reverse ss

### countbyteコマンド

    main = do cs <- getContents
              print $ length cs

### countwordコマンド

    main = do cs <- getContents
              print $ length $ words cs

## 型と高階関数

### 型と値



