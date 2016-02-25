# PHP基礎その2

## 目次
- echo と 文字列
- 演算子について

## 1. echo や 文字列 についてもうちょっと詳しく

### 1.1. シングルコーテーションとダブルコーテーション

- 今まで文字列といえば、 '' で囲っていた
- でも実は "" でもいける

```php:string.php
<?php

echo 'シングルコーテーション';

echo "ダブルコーテーション";
```

じゃあ '' と "" の違いは？

### 1.2. 変数展開その1
- ダブルコーテーションを使うと変数を展開できる

```php:string.php
<?php

$service = 'ELITES';

echo "NOWALLの教育サービス $service";
```

### 1.3. 変数展開その2
- 変数名の直後に文字列をつけるとエラーになる(後に続く文字列も変数名と判断される)
- そういうときは {} で囲う

```php:string.php
<?php

$service = 'ELITES';

echo "ELITES_CAMPだけじゃなくて{$service}もやっています"; // これは大丈夫

echo "ELITES_CAMPだけじゃなくて$serviceもやっています";   // これはエラー
```

```txt:エラーメッセージ
Notice: Undefined variable: serviceもやっています in /var/www/html/index.php on line 8
```


### 1.4. echoのまとめ
- 現時点で完全に理解する必要はない
- わからなくなった時に調べればOK

```php:echo.php
<?php

// 基本の使い方
echo 'Hello World'; // 出力した文字列を '(シングルコーテーション)で囲う

echo "Hello World"; // "(ダブルコーテーション)でもOK


// 文字列の連結
echo 'elites' . 'camp';
echo 'elites' , 'camp';

echo '<hr>';

// 改行
echo 'Line break1' . '<br>';
echo 'Line break2<br>';

echo 'Line break3' , PHP_EOL; // ソースコード上では改行が起きている
echo 'Line break4' , "\n";    // ソースコード上では改行が起きている
echo 'Line break5';

// echo の中で変数を使用できる
$name = 'kashiwagi';
$company = 'nowall';

echo $name;
echo $company;

// 変数と文字列の共存
echo "代表取締役{$name}";
echo '<br>';
echo "$company Inc";
// echo "$companyInc"; // これはエラー
echo "{$company}Inc";  // これはエラーではない
echo '<br>';
```

### 1.5. エスケープ文字
文字列として入力を行えない特殊な文字のことをエスケープ文字という。
例えば、改行はEnterキーを入力してもエディタ上のテキストが改行されるだけで、改行を表す文字とはならない。

```php:escape.php
<?php

echo "改行前\n改行後"; // ソースコードを表示して確認

echo "バックスラッシュ\\";

// echo "バックスラッシュ\"; これはエラー
```

## 2. 演算子をもうちょっと詳しく
- 既に学んだ演算子(+-*/%)は代数演算子と呼ぶ
- 演算子は他にもいくつかある

### 2.1 複合代入演算子
- 計算した結果を代入するときに使う
- これを使わなくても書けるけど、ソースコードを短く出来るので使うことが多い

```php:operator.php
<?php

$a = 0;
$b = 0;

$a = $a + 1;
$b += 1;

echo "a = $a b = $b" . '<br>'; // どちらも1が出力される

$a = $a - 1;
$b -= 1;

echo "a = $a b = $b" . '<br>'; // どちらも0が出力される

// $a = $a + 1' と "$a += 1" は同じ意味
```

- どちらも a に 1 を加算してから a に代入している
- どの代数演算子でも同じように書ける

### 2.2. ところで ++ と --
- プログラミングにおいて、1を足す処理というのは良く使う
- 良く使う処理であれば、より簡略して書ける方法を考案するのがプログラマ

```php:operator.php
<?php

// $i = $i + 1 と $i += 1 と $i++ は 全て同じ意味

$i = 0;

echo $i; // 0

$i++;

echo $i; // 1

$i++;

echo $i; // 2

$i++;

echo $i; // 3
```






