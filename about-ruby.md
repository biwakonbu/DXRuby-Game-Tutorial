Ruby について
============

この章では、
Ruby という言語について解説すると共にプログラミング言語の扱いに
ある程度慣れて貰う事を想定しています。
(想定時間: 1 時限程度)

## Ruby を使う意味

この授業では、
数ある言語の中で、何故 Ruby という言語を使用するのか、
という疑問があるかと思います。
その疑問を紐解きながら Ruby という言語への理解と、
プログラミング (という行為) の理解を深めていきます。

## Ruby を使う

Ruby を使ってプログラムを書く場合、非常に単純に書く事が出来ます。

例えば、色々な所で見られる書き方には、下記の様ものがあります。
```ruby
print('Hello World')
```

このプログラムを書いたファイルを実行する事で、
"Hello World" という文字列をプロンプト (ターミナル部分) に表示する事が
出来ます。

"print" という言葉は "表示" や "印刷" という意味 (和訳) を持っており、
実際、その和訳通りの動作をします。
プログラミングは英語を元に言葉を決めている事が多いので、
使いたいメソッド等は和訳すると何をするか理解できるかもしれません。

' (シングルクォート) もしくは " (ダブルクォート) によって挟まれた
文字 (もしくは文字列) は、
プログラムの中で命令としてでは無く、
人間に読んで貰う為の、メッセージとしての "文字列" として扱われます。
そして、print 文は、その "文字列" を受け取る事を想定しています。

このようにメソッドには受け取るものが決まっている為、
何を受け取る事が出来るのかを常に考えながら書く事が大事です。

"文字列" の他には "配列"、"ハッシュ"、"数値" 
等の属性を持ったものが存在します。
この属性の事を Ruby ではクラスと呼びます。

### 様々な機能

プログラミング言語には様々な機能があります。
機能には、そのプログラミング言語が特別に持っている機能や、
他のプログラミング言語が持っている様な一般的な機能まで存在します。

### 一般的な機能

一般的な概念から説明していきます。

先ず変数という言葉から覚えて下さい。

変数は、プログラミングでは不可欠といえる要素で、
データをプログラムの上で名前をつけて保存する為に使用します。
名前を付けて保存する事で、
必要な時に必要なデータを使用する事が出来る様になります。

この変数を登録 (プログラミングの世界では定義と言う。以後定義を使用) するには、
先頭の文字が小文字のアルファベットで始まる英数字と特定の記号の文字列に何か値を代入する。

変数への代入は下記の様に行なう。
```ruby
num = 10
string = "String"
```

代入には = という記号を使う。左辺 = 右辺と書く事で、
右辺のデータを左辺の名前として定義する。
数学の = (イコール) とは意味が異なる為、注意が必要。

上記の変数を使用する場合、下記の様な書き方が出来る。
```ruby
puts(num)    #=> 10
puts(string) #=> String
```

puts メソッドは print メソッドのように出力するが、
指定文字列の出力後に改行を入れる。
この書き方から分かる様に、変数は定義後であれば、
使用したい変数名をプログラム中に記述する事で、
変数名とデータを置き代えて実行する。

\#=> という記号は期待される出力を指します。
10 と String という文字が出力された事から、
先程代入した変数を、正しく扱えている事が分かります。

プログラミングを行なっていく上で、
どの変数にどの様な値が代入されているのかを理解する事は
非常に重要です。

#### 条件分岐 if 文

プログラミングには、条件分岐という文を扱う機能があります。
ここで言う文とは、文章の事だと思って貰って大丈夫です。
プログラミングには、文と式という物があり、
この 2 つを組み合わせる事で、複雑な処理を実行可能にします。

因みに、式とは、先程の変数の代入や、数値の計算、
メソッドの実行等が挙げられます。

if 文の使い方に関しては、[プログラミングについて](./about-programming.md)
で解説した通りです。
卵の話を、
妻の求める通りにプログラミングとして書くと下記の様になります。

```ruby
buy(milk, 1)
if egg_selling? == true
  buy(egg, 6)
end
```

Ruby には buy というメソッドはありませんが、
ここでは既に定義されている事とします。
buy メソッドは 2 つの引数を受け取ります。
前から順番に買いたい物を文字列で書き、
次の要素に買いたい数を記述します。
この順番は定義する時に決めるので、
どれが正解であるというのはありません。

また、== や true といったキーワードが出てきていますが、
順番に説明していきます。

先ず、== は比較を行なっています。
数学の等号と同様、左右の式の結果を比較し、
結果が同じであれば "真" を返します。
また、結果が異なる場合は "偽" を返します。
if 式はこの "真" と "偽" を用いて条件分岐を行ないます。
さらに、ここでは使用していませんが、
不等号は数学で使用する物と同様の "<" や ">" を使用します。
境界を含む場合は、"<=" や ">=" という書き方も出来ます。

ここまでで、学習内容をまとめる為に問題を問いて貰います。

#### 問題 3.
