
## HTMLへの埋め込み
PHPのコードはHTMLに埋め込むことが出来るので計算結果のHTMLタグで装飾ですることが可能

### 埋め込むときは
- <?php と ?> で囲う
- 実は、1行の埋め込みであれば文末のセミコロンはなくても実行できる。
- if文などを埋め込むときは `{}` ではなく `:` を利用した書き方のほうが見やすい場合が多い

### 1. 単純な埋め込み

```php:embed_1.php
<?php
    $message = '採点結果';
?>
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>埋め込み</title>
</head>
<body>
    <h1><?php echo $message; ?></h1>
</body>
</html>

```
### 2. 制御文の埋め込み

```php:embed_2.php
<?php

// テストの合計点により合否を判定するプログラム
// 各科目の内訳も確認することが出来る

$message = '採点結果';

$scores = array(
    '国語' => 70,
    '数学' => 50,
    '英語' => 30,
    );

$totalScore = array_sum($scores); // 配列の要素の合計値を算出

?>
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>埋め込み</title>
</head>
<body>
    <h1><?php echo $message; ?></h1>

    <?php if ($totalScore >= 300) : ?>
        <h2>合格</h2>
    <?php else : ?>
        <h2>不合格'</h2>
    <?php endif; ?>

    <h1>得点の内訳</h1>
    <p>合計: <?php echo $totalScore; ?>点</p>
    <?php foreach ($scores as $subject => $score) : ?>
        <li><?php echo $subject . " " . $score; ?>点</li>
    <?php endforeach; ?>

</body>
</html>
```



## 余裕がある人へ
上記のプログラムは得点を配列に直接指定していますが、それをフォームから各科目の得点を入力できるように変更してみてください。

<a href="https://elites-camp-201512.herokuapp.com/day02/embed/embed_advanced.php" target="_blank">サンプルプログラム</a>



