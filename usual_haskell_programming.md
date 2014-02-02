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

**関数の型の表し方**

    第1引数の型 -> 第2引数の型 -> ... -> 返り値の型

例：lines

    String -> [String]

例：unlines

    [String] -> String

例：length ([a]は型変数(どんな型と置き換えても良い))

    [a] -> Int

型変数を含む方を、多相型(polymorphic type)という。

例：take

    Int -> [a] -> [a]

**明示的な型宣言**

例：firstNLinesのソースは、

    fiestNLines n cs = ulines $ take n $ lines cs

これの明示的な型宣言は、

    firstNLines :: Int -> String -> String

型を宣言するメリットは、

- 意図した通りにチェックされていることが確実になること
- ソースコードを読む人に情報量が増えること
- (そもそも)型変数を大量に使った複雑なコードでコンパイルを通すために必要

### 高階関数

**値としての関数**

Haskellでは、関数名も変数であると考える。
例えば、

    square n = n * n

は、「nを2乗する関数」という「値」に、変数「square」が束縛されている、と考える。

関数を変数と考えると、関数を引数に取る関数の事を、高階関数という。

**map関数**

例

    map :: (a -> b) -> [a] -> [b]
    map square [1,2,3]

この結果は、[1,4,9]。すなわち、リストの各要素にsquare関数を適用したリストを返す。

**expand関数(@に置換)**

ソース

    main = do cs <- getContents
	          putstr $ expand cs
	
	expand :: String -> String
	expand cs = map translate cs

	translate :: Char -> Char
	translate c = if c == '\t' then '@' else c

注）バックスラッシュtが、タブとしてmarkdown上表現されているので、注意！どうやって直したらいいんだろ・・

if式

    if 条件式 then 式1 else 式2

- Haskellのifは文ではなく式
- if式全体が値を持つ
- elseは省略できないので、Javaでいうとif文よりも条件演算子と似ている(a ? b : c)
- 条件式の値はTrueかFalseでないといけない
- thenやelseの前後で改行しても構わない（1行で書く必要はない）

改行例1

    if c == '\t'
		then '@'
		else c

改行例2

    if c == '\t' then '@'
				 else c

**==演算子**

型宣言

    (==) :: a -> a -> Bool

**expand関数(タブをスペースに変える)**

    main = do cs <- getContents
	          putStr $ expand cs
	
	expand :: String -> String
	expand cs = concat $ map expandTab cs

	expandTab :: Char -> String
	expandTab c = if c == '\t' then "    " else [c]

**concat関数**

リストのリストを連結して一つのリストにする。
例えば文字列のリストにconcat関数を適用すると1つの文字列になる。

    concat :: [[a]] -> [a]

	concat [[1,2], [3], [4,5]]	-> [1,2,3,4,5]
	concat [[1,2], [], [3]]		-> [1,2,3]
	concat ["ab", "c", "de"]	-> "abcde"
	concat ["ab", "", "c"]		-> "abc"
	concat [[[1,1], [2,2]], [[3]]] -> [[1,1], [2,2], [3]]	←これがよくわかってない。なんでこうなる？
															  二番目のリストが無くなったように見えるが・・

