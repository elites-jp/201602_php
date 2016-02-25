
## 0.はじめに
- 文字列を表示する方法
- コメントの書き方
- 変数
- 演算
- 定数


## 1. とりあえず Hello World してみる
- プログラミングといえばやっぱり Hello World
- ブラウザに文字列を表示してみる

### PHPの文法の基本
- ファイルの拡張子は「.php」
- <?php ?> で囲うと、コンピュータが囲まれた範囲をPHPのコードとして処理してくれる
- ただし、PHP単独のファイルは 閉じタグ(?>) をつけないようにすること！
- 文末には ;(セミコロン) をつける
- ソースコード内の改行は反映されない

```php:hello_1.php
<?php
echo 'Hello World';

```

- <a href="http://192.168.33.10/hello.php" target="_blank">ブラウザで確認(192.168.33.10/hello.php)</a>



### 1-2. いろいろとechoしてみる
- 下記の内容を出力してみましょう
    - 「今日の天気は晴れ」
    - 「私の名前は◯◯です」

```php:hello_2.php
<?php

echo 'Hello World';
echo '今日の天気は晴れ';
echo '私の名前は柏木です';

```

- 改行して出力するには？



### 1-3. 改行を出力
- 改行を意味するタグを出力してあげればよい！

```php:hello_3.php
<?php

echo 'Hello World';

echo '<br>';

echo '今日の天気は晴れ';

echo '<br>';

echo '私の名前は柏木です';

```

- echoの回数を減らすことはできないのか？ 文字列を繋げることはできないのか？

### 1-4. 文字列の連結
- 文字列を 「.」 によって繋げることが出来る
- 「.」の前後に半角スペースを入れているのは作法的なもの(人によっては入れない)

```php:hello_4.php
<?php

echo 'Hello World' . '<br>';

echo '今日の天気は晴れ' . '<br>';

echo '私の名前は柏木です';

```


## 2. コメントを使ってみる
- ソースコードにはコメントをつけることができる
- コメントをつけることで、どういう処理をするコードなのかを他人(将来の自分を含む)に伝えることができたりする
- コメントはプログラムとして処理されない
- ソースコードをコメントにすることを"コメントアウト"という

### コメントの付け方
- // 1行コメント
- #  これも1行コメント
- /* */ 複数行コメント

#### SublimeText では ⌘ + / でコメント化 できる！ (複数行でもいけちゃう)

```php:comment.php
<?php

echo 'Hello World' . '<br>'; // 1行用のコメント

# これもコメントなので表示されない

/*
複数行のコメント
echo '今日の天気は晴れ' . '<br>';


echo '私の名前は柏木です';
*/

```


## 3. 変数
- 変数とはデータを格納できる箱のイメージ
- しかも中身を変更することもできちゃう
- 結果を記憶したい時などに使える

### 3-1. とりあえず変数を使ってみよう
- 「$変数名 = 値」 とすると 値 を 変数 に "代入"できる！
- プログラミングの「=」は「等しい」という意味じゃない！

```php:variable_1.php
<?php

$message = 'Hello World'; // 'Hello World' という値を $message に格納している
echo $message . '<br>';

$name = '柏木';
echo '私の名前は' . $name . 'です';
```

変数宣言のルール

```txt:変数の命名ルール
・「$」で始める
・英数字 と _(アンダースコア)が利用可能
・ただし、数字で始めることは出来ない $1message は不可
・大文字と小文字は区別される $message と $MESSAGE は別のもの
・中身が推測できるような名前をつけるとGood!
```

### 3-2. 数値も格納できる
- 今までは文字列(''で囲っていた！)を使っていたけど、数値も扱える

```php:variable_2.php
<?php

$age = 21; // ''は不要！
$name = '柏木';

echo '私の名前は' . $name . 'です' . '<br>';
echo '今年で' . $age . '歳になります';

```


## 4. 数的な演算処理
- プログラミングで数的な演算処理ができる
- そして変数はデータを格納できるので、 計算 → 結果を保持する → その結果を利用して計算 → ... とすることができる

### 4-1. 四則演算をやってみよう
- 加減乗除と余りの計算

```php:cal.php
<?php

$a = 6;
$b = 2;

$sum  = $a + $b; // 和: summation
$diff = $a - $b; // 差: difference
$pro  = $a * $b; // 積: product
$quo  = $a / $b; // 商: quotient
$rem  = $a % $b; // 余: remainder

echo $a . " + " . $b . " = " . $sum . '<br>';
echo $a . " - " . $b . " = " . $diff . '<br>';
echo $a . " × " . $b . " = " . $pro . '<br>';
echo $a . " ÷ " . $b . " = " . $quo . '<br>';
echo $a . " ÷ " . $b . " の余りは " . $rem . '<br>';

```

## 5. データ型
- 今まで "文字列" と 数値 を扱った
- プログラミングには「データ型」という概念が有り、そのデータがどういう種類のデータなのかという情報がある(データの種類が分かると便利だから)

データ型の種類
- 文字列型(String)
- 整数型(Int)
- 論理値型(Bool)
- 小数(float)
- 他にもあるけど、とりあえず String と Int でOK

### 5-1. データ型を見てみよう
- var_dump()という命令を使えばOK
- variable(変数) を dump(ダンプ:中身を表示) する命令

```php:data_type.php
<?php

$name = 'Kashiwagi';
var_dump($name);

echo '<hr>';

$age = 21;
var_dump($age);

```

```txt:実行結果
string(9) "Kashiwagi" // String型(文字列型) 9バイト 中身は"Kasiwagi"
int(21)               // Int型(整数型) 中身は 21
```



## 6. 定数
- 変数があるので、もちろん定数もある
- 定数は一度決めたら変更することができない
- 裏を返すと、外部からの変更をガードするのに役立つ
- 変更することがないデータには定数を使う(固定された値に名前を付けられる！)

### 6-1. 定数を使ってみる
- define()を利用する
- define(定数名, 中身) という書き方をする
- 定数名は一般に全て大文字で宣言する(定数であることがわかりやすいから)

```php:define.php
<?php

define('ADDRESS', '東京都新宿区西新宿6-15-1 セントラルパークタワー ラ・トゥール新宿 605'); // 定数は $ 不要

echo 'NOWALLの住所は' . ADDRESS;

define('ADDRESS', '住所変更'); // 変更しようとすると警告が出る

```

```txt:実行結果
NOWALLの住所は東京都新宿区西新宿6-15-1 セントラルパークタワー ラ・トゥール新宿 605
Notice: Constant ADDRESS already defined in /var/www/html/index.php on line 8
```


## 7. ここまでのエラー集
- エラーコードを読めるようになるのが上達への一歩
- エラーが表示されたら "ソースコードを確認する前に" エラー文を読むこと！


### 7-1. エラーコードの意味
- いろいろあるけどとりあえずこの2つ(要するに文法ミス)
    - Parse error
    - syntax error

### 7-2. セミコロン抜け
- とても良くあるミス
- unexpected echo とあるのは 「echo自体が間違っている」という意味ではなくて、予期せぬ位置にechoがある → 直前の文がおかしい みたいな流れ


```php:error.php
<?php

// このコードは正しいコードではありません
$message = 'hello'
echo $message;
```

```txt:実行結果
Parse error: syntax error, unexpected 'echo' (T_ECHO) in /var/www/html/index.php on line 4
```

### 7-3. 「.」のつけ忘れ
- セミコロン忘れと同じパターン
- エラー文にどう直せばよいかも書いてある

```php:error.php
<?php

// このコードは正しいコードではありません
$name = 'kashiwagi';
echo '私の名前は' $name;

```

```txt:実行結果
Parse error: syntax error, unexpected '$name' (T_VARIABLE), expecting ',' or ';' in /var/www/html/index.php on line 5
```


### 7-4. 変数名の付け方が間違っている
- そんなに起こらないと思う

```php:error.php
<?php

// このコードは正しいコードではありません
$1name = 'kashiwagi';

```

```txt:実行結果
Parse error: syntax error, unexpected '1' (T_LNUMBER), expecting variable (T_VARIABLE) or '$' in /var/www/html/index.php on line 3
```

### 7-5. 同じ名前の定数を宣言した
- エラー文の通り

```php:error.php
<?php

// このコードは正しいコードではありません
define('ADDRESS', '東京都新宿区西新宿6-15-1 セントラルパークタワー ラ・トゥール新宿 605'); // 定数は $ 不要

echo 'NOWALLの住所は' . ADDRESS;

define('ADDRESS', '住所変更'); // 変更しようとすると警告が出る

```

```txt:実行結果
Notice: Constant ADDRESS already defined in /var/www/html/index.php on line 8
```
