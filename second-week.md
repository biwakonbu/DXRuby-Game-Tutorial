DXRuby でゲーム作成
=============

### キャラクタの操作

キャラクタの操作を行うために、
キーボード入力の受け付けを行なう必要が出てきます。

DXRuby では、キーボード、マウス、
ゲームパッドのそれぞれのアクション
(要するにボタンを押したりする行為) を受け付けています。
その為、ユーザーからの入力は簡単に扱えます。
そして、この機能を使ってキャラクタの移動の機能を実装します。

今回はキャラクタの移動については、
キーボードの入力を受け付ける形にします。
キーボード入力の受け付けは、
``Input.keyDown?`` というメソッドを使用する事で出来ます。
メソッドの () 内で指定したキーの入力
(押し続けを含む) を判定し、指定したキーを押していれば true
という、"真" をあらわす値が返されます。
指定したキーが押されていなければ false
という "偽" をあらわす値が返されます。

このメソッドだけでは押しているのかどうかが分かるだけで、
処理がゲームに反映されない
(特定のボタンを押した時にキャラクタが移動しない) ため、
条件分岐を使います。

キーボードの特定のキーを押すと
キャラクタが移動するという機能が欲しい場合、
キーを押していると移動する、
何も押していない場合は動かない、といった処理にする必要があります。

下記の様な記述を行なう事で、入力に対して動作を割りあてる事が出来ます。
```ruby
Window.loop do
  # K_L と言うのは、キーボードの L キーという意味
  # つまりキーボードの L のキーを押して (押し続けて) いたら true (真) である
  # true が返ってくると、if `条件式` ~ end の間に書いた処理が実行される
  # この場合は sample_sprite.x += 1 が実行される
  # 因みに、移動速度を変更したい場合は数値を変更すると良い
  if Input.keyDown?(K_L)
    sample_sprite.x += 1
  end

  # Sprite.draw メソッドを使うと指定した Sprite データを画面に表示 (描画) することが出来る
  Sprite.draw(sample_sprite)
end
```

使いたいキーのコードを調べたい場合、
[DXRuby キーコードリファレンス](http://dxruby.sourceforge.jp/DXRubyReference/2009823193120640.htm)
を参照して下さい。

#### 問題 7.

矢印キーを使って、
上下左右に移動出来る様にプログラムを作成して下さい。

### 衝突判定

衝突判定とは、
オブジェクトとオブジェクトが衝突を起こしていることを判定する事です。
衝突判定に穴があると、
キャラクタが重なったり、壁のすり抜け等のバグに繋ります。

本来、衝突判定は非常に面倒くさい処理を必要としますが
(数学で、行列やベクトル等が必須)、
簡単に判定できる仕組みがありますのでそちらを使用します。

まず、当たり前ですが、衝突には二つ以上のオブジェクトが必要ですので、
Sprite データとして ２ つの画像 (オブジェクト) を用意します。
簡単に判定を行うには Sprite データとして扱わないと行けないので、
必ず Sprite データとして画像を用意して下さい。

衝突判定を行なうには、``===`` という比較を使用します。
``==`` と間違えない様に注意して下さい。
``===`` を挟んだ、
左辺のオブジェクトと右辺のオブジェクトが
重なっている場合に true を返します。
重なっていない場合は false を返します。

サンプルコードを実行して衝突する事と確認して下さい。
```ruby
image_chara = Image.load_tiles("../image/character.png", 4, 4)
chara = Sprite.new(400, 200, image_chara[0])

image_box = Image.load_tiles("../image/colorbox.png", 6, 1)
box = Sprite.new(200, 200, image_box[0])

Window.loop do
  if Input.keyDown?(K_L)
    chara.x += 2
    # === を使って左辺と右辺を比較する事で衝突の判定をとることが出来る
    # この時両方のデータは Sprite データを持った変数か、配列であること
    # それ以外ではエラーとなる
    # 衝突が行われた場合は true が返ってくるため、この場合、
    # 直前にキャラクタの座標を x+2 しているため、その時に衝突した場合は
    # x+2 の処理を取りやめている
    # 結果的に、キャラクタは移動せずに壁にぶつかって止まっているように見える
    if chara === box
      chara.x -= 2
    end
  end

  # 画面の更新を行っている
  # この処理が実行されるまでは、オブジェクトにどれだけ変化が起こっていても
  # 見た目には反映されない
  Sprite.draw(chara)
  Sprite.draw(box)
end
```

#### 問題 8.

上記プログラムでは衝突の判定は box に対して右にのみ存在しています。
その為、
キャラクタが箱に当たった時に移動しないという操作は右からのみ有効です。
これを、全ての方向から判定を行ない、すり抜けが発生しない様にして下さい。

#### 問題 9.

配列は下記プログラムの様にも宣言する事が出来ます。

```ruby
array = [1, "test"]

print(array)    #=> [1, "test"]
print(array[0]) #=> 1
print(array[1]) #=> test
```

この様な記述の方法を使う事で、
Sprite データを画面に表示する時のプログラムを纏める事が出来ます。
これを利用し、
問題 8. で作成したプログラムに存在している Sprite.draw の部分を 1 行に纏めて下さい。

### Map の作り方

Map という名前がついてる為、キャラクタとは異なるモノを連想しがちですが、
基本的に Map も画像を並べているだけ
(障害物や人等の意味合いをもたせるのはあくまでもプログラムの仕事) で、
自分が表現したいように画像を並べる方法を覚えると、
簡単に Map を生成することが出来ます。

Map を作成するには、既に習った配列と繰り返しを使用する事で出来ます。
手作業で Map を作成する場合、
下記の様なプログラムを作成する事になります。

```ruby
require 'dxruby'

image = Image.load_tiles("../image/colorbox.png", 6, 1)

gray1 = Sprite.new(0,0,image[5])
gray2 = Sprite.new(20,0,image[5])
gray3 = Sprite.new(40,0,image[5])
gray4 = Sprite.new(60,0,image[5])
gray5 = Sprite.new(80,0,image[5])
gray6 = Sprite.new(100,0,image[5])
gray7 = Sprite.new(120,0,image[5])
gray8 = Sprite.new(140,0,image[5])
gray9 = Sprite.new(160,0,image[5])
gray10 = Sprite.new(180,0,image[5])

Window.loop do
  Sprite.draw(gray1)
  Sprite.draw(gray2)
  Sprite.draw(gray3)
  Sprite.draw(gray4)
  Sprite.draw(gray5)
  Sprite.draw(gray6)
  Sprite.draw(gray7)
  Sprite.draw(gray8)
  Sprite.draw(gray9)
  Sprite.draw(gray10)
end
```

上記プログラムを実行してみると、
少しばかりのブロックが生成されている事が分かります。

これでは本格的なゲームだけでなく、
簡単なゲームを作るだけでもかなりの労力を伴います。
その為、繰り返しを使う訳です。

上記のプログラムを繰り返しを使用して書き直すと下記の様になります。

```ruby
require 'dxruby'

block_x = 0
block_y = 0
count = 0
sprites = []

images = Image.load_tiles("../image/colorbox.png", 6, 1)

loop do
  sprites[count] = Sprite.new(block_x, block_y, images[5])
  if 180 < block_x
    break
  end
  block_x = block_x + 20
  count = count + 1
end

Window.loop do
  Sprite.draw(sprites)
end
```
