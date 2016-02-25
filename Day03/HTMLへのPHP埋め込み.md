# HTMLへのPHP埋め込み

## HTMLへの埋め込み
PHPのコードはHTMLに埋め込むことが出来る

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

### 2. 制御文の埋め込み_if
- `{}`を使うと見づらいので`:`を使うのが基本

```php:embed_2.php
<?php

// 年齢を入力すると成人かどうかの判定をするプログラムを
$age = 20;

?>
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>埋め込み</title>
</head>
<body>
    <?php if ($age >= 20) : ?>
        <h2>成人です</h2>
    <?php else : ?>
        <h2>未成年です</h2>
    <?php endif; ?>

</body>
</html>
```

### 3. 制御文の埋め込み_foreach
- ifと同様で`{}`を使うと見づらいので`:`を使うのが基本
- 結構良く使う

```php:embed_3.php
<?php

// 配列の中身をリスト表示する
$members = array('柏木', '日高', '大平');

?>
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>埋め込み</title>
</head>
<body>
<h1>講師リスト</h1>
<?php foreach ($members as $member) : ?>
    <li>
        <?php echo $member; ?>
    </li>
<?php endforeach; ?>

</body>
</html>
```

### 4. ちょっと複雑な場合
- if と foreach の混合

```php:embed_????????.php
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

    <?php if ($totalScore >= 210) : ?>
        <h2>合格</h2>
    <?php else : ?>
        <h2>不合格</h2>
    <?php endif; ?>

    <h1>得点の内訳</h1>
    <p>合計: <?php echo $totalScore; ?>点</p>
    <?php foreach ($scores as $subject => $score) : ?>
        <li><?php echo $subject . " " . $score; ?>点</li>
    <?php endforeach; ?>

</body>
</html>
```


## 余裕がある人へ(フォームが扱えるようになったらやってみましょう)
上記のプログラムは得点を配列に直接指定していますが、
フォームから各科目の得点を入力できるように変更してみてください。

![サンプル](https://i.gyazo.com/cd3be6190ad0158812f96fac17350be6.gif)

```php:
<?php

// テストの合計点により合否を判定するプログラム
// 各科目の内訳も確認することが出来る
// 各科目の得点をフォームから入力出来る

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $scores = array(
        '国語' => $_POST['japanese'],
        '数学' => $_POST['math'],
        '英語' => $_POST['english']
        );

    $totalScore = array_sum($scores); // 配列の要素の合計値を算出
}
?>
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>埋め込み</title>
</head>
<body>

<?php if (isset($scores)) : ?>
    <?php if ($totalScore >= 210) : ?>
        <h2 style="color: red">合格</h2>
    <?php else : ?>
        <h2 style="color: gray">不合格</h2>
    <?php endif; ?>

    <p>合計: <?php echo $totalScore; ?>点</p>
    <?php foreach ($scores as $subject => $score) : ?>
        <li><?php echo $subject . " " . $score; ?>点</li>
    <?php endforeach; ?>
    <hr>
<?php endif;?>
<form action="" method="post">
    <p>国語: <input type="text" name="japanese"></p>
    <p>数学: <input type="text" name="math"></p>
    <p>英語: <input type="text" name="english"></p>
    <p><input type="submit" value="送信"></p>
</form>

</body>
</html>
```

