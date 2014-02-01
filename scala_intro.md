# やさしいScala入門

日向　俊二、カットシステム

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

_は、他の全て、という意味。関数定義でも使える。例えば、

    def how(i:Int) = i match {
		case 0 => "Zero"
		case 1 => "One"
		case _ => "Other"
	}

**return**

    def twiceabs(x:Int): Int = {
		if (x < 0)
			return -2 * x
		x * 2
	}

**例外**

    val m = 4
	val n = 0
	var x: Int = 0
	try {
		x = m / n		// ここで例外発生する
	} catch {
		case ex: ArithmeticException => println("ぜろわり")		// これが表示される
		case ex: Exception => println("れいがい")
	} finally {
		println("継続")
	}

	println("end")


## 演算子

**加減乗除、関係演算子**

等しくない、は!=。Cとほぼ同じ。

**ビット演算子**

これまたCとほぼ同じ。>>>で符号なし右シフトが可能。


## クラスとオブジェクト

### オブジェクト

Scalaでは全てのものがオブジェクト。「2」という値そのものも、Intクラスのインスタンス。

### クラス

- メソッド
- フィールド：メンバ変数・定数

例

    class Dog {
		def bark():Unit = println("bowwow")
	}

使うには、

    val pochi = new Dog
	pochi.bark

**クラスパラメータ**

    class Dog(name:String, age:Int) {
		def printProp():Unit = println("name=" + name + " age=" + age.toString)
	}

### Anyクラス

全てのクラスは、Anyクラスからの派生。
Anyクラスは以下のメンバを持つ。

- !=
- ==
- asInstanceOf[x]：オブジェクトを強制的にx型に型変換する
- isInstanceOf[x]：オブジェクトがx型ならtrueを返す
- equals(x)：このオブジェクトとxが同じならtrueを返す
- hashCode()：オブジェクトのハッシュ値を返す(Int)
- toString

**関数のオーバーライド**

    class Dog (name:String, age:Int) {
		override def toString(): String = {"name = " + name + " age=" + age.toString}
	}

### trait

クラス同様メソッドとフィールドを定義出来るけれども、オブジェクトを生成することは出来ない。
クラスに合成（ミックスイン）して使う。

    trait Life {
		def eat() {println("Mogu mogu") }
	}

ミックスインする方法その1
このようにミックスインすると、他のクラスを継承出来ない。

    class Dog1 () extends Life {
		def twitter(): Unit = println("bowwow")
	}

ミックスインする方法その2

    class Dog2() extends Animal with Life {
		def twitter(): Unit = println("bowwow")
	}

### シングルトンオブジェクト

Scalaのクラスには、静的メソッドを定義出来ない。
ただしアプリケーションを作成する時にはアプリケーションのオブジェクトを作成する必要があり、
ひとつのアプリケーションオブジェクトの中でmainメソッドを一つにする必要があるので、
classではなくobjectを使って、シングルトンオブジェクトを定義する。

    object SimpleApp {
		def main (args: Array[String] ) {
			for (arg <- args)
				println(arg)
		}
	}

コンソールからの実行の仕方

    > :load SimpleApp.scala
	> val msg = Array("a", "b", "c")
	> SimpleApp.main(msg)

引数を指定しない時

    > test.main( new Array[String](0) )

コンパイルしての実行

    > scalac SimpleApp.scala
	> scala SimpeApp a b c

ただしアプリケーションを作成する場合、というのはシングルトンオブジェクトの一つの例で、
main関数以外の関数を持つシングルトンオブジェクトを作ることも可能。例：

    object Circle {
		val PI:Double 3.14
		def calcArea(ra:Double) = {
			println("area=" + (ra * ra * PI).toString)
		}
	}

	class Circle (r: Double) {
		def getArea(): Double = {
			r * r * Circle.PI
		}
	}

使用例

    > :load Circle.scala
	> val c1 = new Circle(2.0)
	> c1.getArea
	res3: Double = 12.5664
	> Circle.calcArea(3.0)
	Area=28.2744


## Scalaライブラリ

### List

Arrayに似ているが、要素のソート(sort)、逆転(reverse)、削除(remove)など様々な操作が可能。
使い方

    val names = List("a", "b", "c", "d")
	names.length			// 4
	names.apply[0]			// "a"
	val reversednames = names.reverse

	if ((names exists(_ == "c")) == true) {		// "c"はあるか？
		println("c lives.")
	} else {
		println("c be absent.")
	}

### Map

値とキーのペアを保存するコンテナ。

    var Dogs = Map[String, Int]()		// 宣言のみ
	var Cats = Map("Kitty" -> 1, "Tama" -> 2)		// 宣言＆初期化
	for (cat <- Cats)		// Catsを出力する
		println(cat)		// 例えば、(Kitty,1) と出力される
	Dogs += ("Pochi" -> 3)	// 要素の追加の仕方
	Dogs += ("Kenta" -> 1)
	if ((Dogs contains ("Pochi")) == true) {
		println(Dogs("Pochi").toString)		// 3
	} else {
		...
	}

### Random

乱数

### Console

- print
- println
- printf：C言語ライク
- format
- readLine
- readBoolean
- readByte
- readShort
- readChar
- readInt
- readLong
- readFloat
- readDouble
- readf
- readf1
- readf2
- readf3

### 他

**javaライブラリの利用**

    import java.io._

	val f: File = new File("sample.dat")
	val fos = new FileOutputStream(f)
	val dos = new DataOutputStream(fos)

	dos.writeDouble(123.45)
	dos.writeLong(100000)
	
	dos.close()
	fos.close()
	
読み込み時

    val fis = new FileInputStream(f)
	val dis = new DataInputStream(fis)

	var d = 0.0
	d = dis.readDouble()
	var l = 0L
	l = dis.readLong()

	dis.close
	fis.close

**Threadとコールバック**

    object tensec {
    	var sec: Int = 0
    	def tentimes(callback: () => Unit) {
    		while (sec < 10) {
    			callback()
    			Thread.sleep(1000)
    		}
    	}
    	def printtime() {
    		sec += 1
    		print (sec.toString + " ")
    	}
    	def main(args: Array[String]) {
    		tentimes(printtime)
    		println()
    	}
    }

実行結果

    > :load tensec.scala
	> tensec.main(new Array[String](0))
	1 2 3 4 5 6 7 8 9 10

## GUIプログラミング

### 初歩

2.8.0以降は、SimpleGUIApplicationではなく、SimpleSwingApplicationになっているので注意。
単に、Hello Scalaと表示するだけのアプリ。

    // GUIHello.scala
    import scala.swing._
    
    object GUIHello extends SimpleSwingApplication {
    	def top = new MainFrame {
    		title = "Hello"
    		val label = new Label{ text = "Hello, Scala" }
    		contents = label
    	}
    }


## 参考リソース

1. [The scala programming language](http:www.scala-lang.org/)
2. [Developper resources for Java Technology](http://www.oracle.com/technetwork/java/java-sun-com-138872.html)
3. Scala スケーラブルプログラミング, Martin Odersky, Lex Spoon, Bill Venners, 長尾　高弘、羽生田　栄一、インプレスジャパン
4. コンピュータサイエンス入門、日向　俊二、カットシステム
5. 知らないと恥をかくプログラミングの常識、日向　俊二、アスキー・メディアワークス


