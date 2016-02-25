# foreachの練習

## 1. 配列から要素を取り出すその1
下記のような配列が与えられたとき、foreach文を使って

月火水木金土日

と表示するプログラムを書いてください。

```php:
$days = array('月', '火', '水', '木', '金', '土', '日');
```

## 2. 配列から要素を取り出すその2
下記のような配列が与えられたとき、foreach文を使って

ELITES CAMPのスタッフは 柏木、日高、大平 です

と表示するプログラムを書いてください。


```php:
$members = array('柏木', '日高', '大平');
```

## 3. 連想配列のキーと要素を取り出す
下記のような配列が与えられたとき、foreach文を使って

MacOS 10.6 は Snow Leopard
MacOS 10.7 は Lion
MacOS 10.8 は Mountain Lion
MacOS 10.9 は Marvericks
MacOS 10.10 は Yosemite
MacOS 10.11 は El Capitan

と表示するプログラムを書いてください。

```php:
<?php
$macOS = array(
            '10.6' => 'Snow Leopard',
            '10.7' => 'Lion',
            '10.8' => 'Mountain Lion',
            '10.9' => 'Marvericks',
            '10.10' => 'Yosemite',
            '10.11' => 'El Capitan'
        );


?>
```
