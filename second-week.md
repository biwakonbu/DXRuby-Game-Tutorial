

### Map の作り方

Map の作成方法を理解する。
Map という名前がついてる為キャラクタとは異なるモノを連想しがちではあるが、
基本的に Map も画像を並べているだけ (障害物や人等の意味合いをもたせるのはあくまでもプログラムの仕事) なので、
自分が表現したいように画像を並べる方法を覚えると、Map を生成することが出来る。

実際に、画像を並べる方法として、今まで行った方法のみを使い Map を生成すると、半端な数しか生成できていないが以下の様なプログラムになる。


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
gray11 = Sprite.new(200,0,image[5])

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
書いたプログラムの行数に比例した量のブロックのみ
表示されていることが分かる。
これでは、本格的なゲームだけでなく、
簡単なゲームを作るだけでもかなりの労力を伴う。
その為、プログラミングの世界では、繰り返しという概念を用いて、法則性の在る作業を自動化する。

上記プログラムを自動化すると下記のようになる。

```ruby
require 'dxruby'

blocks = Image.load_tiles("../image/colorbox.png", 6, 1)


# 全て大文字で書かれたプログラムは、定数として扱われる
# 定数は、一度だけ代入することが出来る変数で、値に名前をつけておきたい時に使う
# 値が変動することのないパラメータで使用する

# ブロックのサイズ定数
X = 20
Y = 20

# ゲーム画面の端から空きスペースを確保する定数 (バッファー)
X_BUFFER = 30
Y_BUFFER = 20

GRAY = 5

# Map の形を作っている配列
# 0 が背景用のブロック、1 が背景として描画する
map_tmp = [
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,1,1,1,1,1,1,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,1,1,1,1,1,1,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,0,0,0,0],
       [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
      ]

map = []

# 繰り返し文
# for 変数 in 条件式 ~ end の形で書く
# 配列を条件式の部分に書くと、中身を１つずつ取り出して
# 変数にセットし、~ の部分を実行する
# end に処理が到達すると、次の変数をセットして配列の中身を全て取り出すまで繰り返す

# 数字..数字 という .. で数字をつなぐ書き方をすると、左辺から右辺まで (右辺含む) の数値を
# 配列にまとめる
# 例) 0..10  #=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# .length は配列の後ろ (配列用変数でも可能) につける事で配列の要素の数を返す
# 下記のプログラムでは二次元配列になっている配列の一次元目の長さを調べている
for i in (0..(map_tmp.length - 1))
  for j in (0..(map_tmp[0].length - 1))
    # 以下二行は式が長い為、変数を作り、後で一行に書く文字数を減らしている
    x_block = X_BUFFER + X * j
    y_block = Y_BUFFER + Y * i

    # map 配列の全ての値を 1 つずつ調べて値が 0 の位置にスプライトデータを代入する
    if map_tmp[i][j] == 0
      map.push(Sprite.new(x_block, y_block, blocks[GRAY]))
    end
  end
end

Window.loop do
  Sprite.draw(map)
end
```

今回のプログラムでは、Map を作成しているのだが、その Map の表現に
配列が使われていることが分かる。
さらに、繰り返しを用いる事で、配列にて用意された Map データを容易に再現できている。
今回 map, map_tmp と二つの配列を作成した。

map はゲーム上に実際に表示する為に使用する Sprite データを配列に入れている。
衝突判定の話で、`===` を使用すると判定を取れると説明したが、
衝突判定は配列を対象にしてもとることが出来る。
ただし、配列の中身が全て Sprite データで在ることが条件である。
一つのキャラクタと Map の衝突を判定したい場合はそれぞれを比較することで可能となる。
map_tmp は Map データの下書きみたいなもので、描画したい形と同じように配列のデータを設定する。
配列内の数値に関しては、後に使用しているため配列定義時点では特に意味は無い。
今回は配置するブロックに灰色のブロックを使用するため、定数を用いて設定を行った。

定数とは、値が変化しないことを約束された変数の事で、基本的には変化させてはいけない。
Ruby では警告が出るものの二度以上代入することが出来る。

ここまでを理解すると、テトリスのワンブロックを積み上げる部分迄を作成することができる。

### ブロックの積み上げ機能

先ほど作成した Map に先週行ったキャラクタの作成と移動を使用し、
ブロックを積み上げられるようにする。
また、ブロックを新しく作成する機能と、色をランダムに選択する機能を更に新しくつける。

```ruby
require 'dxruby'

# def 名前(変数, 変数, ...) 
# ~
# end
# 変数の定義と同じで、メソッドにも好きな名前を指定することができる
# また、( ) かっこ内に変数を指定でき、そこで指定した変数は
# メソッドを呼び出す際に必ず対応した値を入れる必要がある
# 例) 名前(変数, 変数) 
# この処理を行った時、メソッドで最後に実行した行が返す値が
# メソッドを実行した行に対して置き換わる
# また、return という文を使用することで、メソッドを強制的に終了させ、
# 指定した値を返すことも出来る
def create(block, x=X_BUFFER+X+50, y=Y_BUFFER)
  Sprite.new(x, y, block)
end

Window.caption = 'OneBlockTetris'
Window.width = 500
Window.height = 600

X = 20
Y = 20

X_BUFFER = 30
Y_BUFFER = 100

NEXT_PRINT_X = X_BUFFER + X * 14 + 10
NEXT_PRINT_Y = Y_BUFFER + Y * 17 + 10
NEXT_FONT = Font.new(19)

RED = 0
BLUE = 1
GREEN = 2
ORANGE = 3
YELLOW = 4
GRAY = 5

clear = false
current_block = nil
next_block = rand(5)
next_block_view = nil

blocks = Image.loadTiles('../image/colorbox.png', 6, 1)

map_tmp = [
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,1,1,1,1,1,1,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,1,1,1,1,1,1,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,0,0,0,0],
       [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
      ]

map = []

for i in (0..(map_tmp.length-1))
  for j in (0..(map_tmp[0].length-1))
    map.push(Sprite.new(X_BUFFER + X * j, Y_BUFFER + Y * i, blocks[GRAY])) if map_tmp[i][j] == 0
  end
end

Window.loop do
  # unless 文は if 文の反対で、条件式が false, nil を返す時だけ中の処理を実行する
  # また、この条件文内では、current_block という変数を定義して、
  # next_block (次のブロック候補) からブロックをコピーする
  unless current_block
    current_block = create(blocks[next_block])
    next_block = rand(5)
    next_block_view = create(blocks[next_block], NEXT_PRINT_X+10, NEXT_PRINT_Y+30)
  end

  if Input.keyDown?(K_H)
    current_block.x -= 2
    if current_block === map
      current_block.x += 2
    end
  end

  if Input.keyDown?(K_L)
    current_block.x += 2
    if current_block === map
      current_block.x -= 2
    end
  end

  if Input.keyDown?(K_J)
    current_block.y += 2
    if current_block === map
      current_block.y -= 2
    end
  end

  if current_block === map
    current_block.y -= 1
    map.push(current_block)
    clear = clean(map)
    current_block = nil
  else
    current_block.y += 1
  end

  if current_block
    current_block.draw 
    Sprite.draw(map)
  end
end
```

上記プログラムでは、ブロックの作成、次に作成するブロックの作成、表示を行っている。
そして、新しく自分自身でメソッドを定義している。
今までは既に定義されているメソッドを使用していたが、
ブロックを生成するメソッドは存在しないため、自分で作る必要がある。
作らず、同様の処理を必要としている場所に書くという方法もあるが、
それをしてしまうと、同様の処理を何度も書く必要がでて無駄が生じる。
その為、処理に名前をつけて記録する必要がある。


### Clear 処理

ゲームを作成する上で最終目標を設定する事がよくある。
勿論最終目標が無く、サバイバル的に続けられる分だけ続けるゲームも存在する。
今回の 1 ブロックのテトリスでは、一行を揃えるだけでクリアとしたいので、
クリア用のプログラムを作成する。

```ruby
require 'dxruby'

def create(block, x=X_BUFFER+X+50, y=Y_BUFFER)
  Sprite.new(x, y, block)
end

def clean(map)
  20.downto(0) do |height|
    if map.count {|block| (block.x < X_BUFFER + X * 11) && (block.y == Y_BUFFER + Y * height)} == 11
      return true
    end
  end
  false
end

Window.caption = 'OneBlockTetris'
Window.width = 500
Window.height = 600

X = 20
Y = 20

X_BUFFER = 30
Y_BUFFER = 100

NEXT_PRINT_X = X_BUFFER + X * 14 + 10
NEXT_PRINT_Y = Y_BUFFER + Y * 17 + 10
NEXT_FONT = Font.new(19)

RED = 0
BLUE = 1
GREEN = 2
ORANGE = 3
YELLOW = 4
GRAY = 5

clear = false
current_block = nil
next_block = rand(5)
next_block_view = nil

blocks = Image.loadTiles('../image/colorbox.png', 6, 1)

map_tmp = [
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,1,1,1,1,1,1,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,1,1,1,1,1,1,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,0,0,0,0],
       [0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,0,0,0,0],
       [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
      ]

map = []

for i in (0..(map_tmp.length-1))
  for j in (0..(map_tmp[0].length-1))
    map.push(Sprite.new(X_BUFFER + X * j, Y_BUFFER + Y * i, blocks[GRAY])) if map_tmp[i][j] == 0
  end
end

Window.loop do
  # ゲームを終了する用のキーを追加
  # Q キーを押すと終了
  # break 文は普通繰り返しを強制的に終了する為に使用する
  # この場合では、ゲームを動かしているループを終了するため
  # ゲームが終了する
  if Input.keyPush?(K_Q)
    break
  end

  # clear 変数が false の時はクリア状態を満たしていないため else の部分を実行する
  # true になると if clear の内部を実行する
  if clear
    Window.drawFontEx(100, 300, 'CLEAR!!!', Font.new(70), { font: [255, 255, 255] })
    if Input.keyPush?(K_RETURN)
      break
    end
  else
    unless current_block
      current_block = create(blocks[next_block])
      next_block = rand(5)
      next_block_view = create(blocks[next_block], NEXT_PRINT_X+10, NEXT_PRINT_Y+30)
    end

    if Input.keyDown?(K_H)
      current_block.x -= 2
      if current_block === map
        current_block.x += 2
      end
    end

    if Input.keyDown?(K_L)
      current_block.x += 2
      if current_block === map
        current_block.x -= 2
      end
    end

    if Input.keyDown?(K_J)
      current_block.y += 2
      if current_block === map
        current_block.y -= 2
      end
    end

    if current_block === map
      current_block.y -= 1
      map.push(current_block)
      clear = clean(map)
      current_block = nil
    else
      current_block.y += 1
    end

    current_block.draw if current_block
    Window.drawFontEx(NEXT_PRINT_X, NEXT_PRINT_Y, 'Next', NEXT_FONT, { font: [255, 255, 255] })
    Sprite.draw(next_block_view)
    Sprite.draw(map)
  end
end
```
今回のプログラムが正常に動作すれば演習は終了である。
最後の追加項目は、`clean` メソッドの追加とゲームのシーンの設定である。
`clean` メソッドは、ブロックが、一行揃っているかを調べ、揃っていれば true、
揃っていなければ false を返す。

そして、ゲームのシーンの決定に if 文 (条件分岐) を使い、
clean メソッドの実行結果が true になった時に、Clear の文字を入れるようにしている。

実際に動作が確認出来たなら、クリアの文字が表示できるか確かめてみよう。