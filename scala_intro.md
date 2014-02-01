# やさしいScala入門

## 概要

### コードの書き方

インタプリタ実行のみの場合は、そのまま書けば良い。

コンパイル実行する場合はやり方が2つあり、ひとつはmainを含める場合、もうひとつはApplicationをExtendする場合。
mainを含める場合は、

    object Hello {
		def main(args: Array[String]) {
			var msg = "Hello"
			println(msg)
		}
	}

ApplicationをExtendする場合は、

    object Hello extends Application {
		(実行コード)
	}

みたいな感じ。


### 実行・コンパイル

**実行（インタプリタ）**

    scala test.scala

**コンパイル＆実行**

test.scalaの中に、Hogeクラスがある場合。

    scalac test.scala
    scala Hoge

### 文法

**コメント**

C/C++と同じ。ただし*/はネスト可能。

**型**

整数型は全て符号付き。

- Byte
- Short
- Int
- Long
- Float
- Double
- Char（16bit符号なしUnicode文字）
- String
- Boolean

**数値→文字列変換**

toString。ただprintln時に以下のようにも書ける。

    println("x=" + x)

暗黙の型変換があるため。

**定数配列**

    val ai = new Array[Int](3)
	val ad = Array(1.0, 2.0, 3.0, 4.0)

**変数**

- val : 定数
- var : 変数


## 関数

### 演算子

演算子も関数。例えば

    a+b

は、Int型のaのメソッド+を呼び出す、という意味があるので、

    (a).+(b)

とも書ける。

### 書き方

    def twice (x: Int): Int = {
		x * 2
	}

みたいな感じ。呼び出しは

    var y = twice(3)

で出来る。yには6が入っている。

**可変長引数**

引数リストの最後に指定して、引数の型宣言の後にアスタリスクをつける。

    def average(n: Int, vars: Double*):Double = {
		var total = 0.0
		for (v <- vars.elements) total += v
		total / n
	}

使用例と結果

    average(2, 12.3, 23.4)
	-> Double = 17.85

	average(5, 10.0, 11.0, 12.0, 13.0, 14.0)
	-> Double = 12.0


**関数リテラル（名前なし）**

    (v1: Int, v2: Int) => v1 * v2

**関数リテラル（名前あり）**

    var mult = (v1: Int, v2: Int) => v1 * v2

複数行にまたがって書くことも可能。

    var prmult = (v1: Int, v2: Int) => {
		print(v1.toString + "*" + v2.toString + "=")
		println(v1*v2)
		v1 * v2
	}

**値を返さない関数**

Unit

## 制御構文

### 条件式

**if**

    if (x==0) {
		println("a")
	} else {
		println("b")
	}

ifは式でもあるので値を返せる。

    if (x > 0) x else -1 * x

**while, do-while**

Cと類似

**for**

    val names = Array("a", "b", "c")
	for (name <- names)
		println(name)

forループも値を返せる。ループ毎の値を覚えるためには、yieldを使う。

    val names = Array("a", "b", "c")
	for (name <- names) yield {
		println(name)
		name
	}

とした場合、コンソールには、

    a
	b
	c
	res4: Array[java.lang.String] = Array(a, b, c)

と出力される。

Cっぽいforループを書きたい場合は、

    for (i <- 0 to 2)
		println(i)

みたいにする。

**foreach**

    val names = Array("a", "b", "c")
	names.foreach(println)

**match-case**

    var msg = i match {
		case 0 => "Zero"
		case 1 => "One"
		case _ => "Other"
	}

_は、他の全て、という意味。



