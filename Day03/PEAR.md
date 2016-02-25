
## 1. PEARとは
- PHPのライブラリ
- このPEARというライブラリを利用してメールを送信してみる
- クラス(オブジェクト指向)を利用することになるがそんなに難しくない

## 2. PEARのインストール

```bash:PEARインストール
$ sudo yum install -y php-pear
$ sudo pear channel-update pear.php.net
$ sudo pear install -a Mail
$ sudo pear install -a Mail_MIME
```

## 3. メール送信
日本語メールの送信をしてみる

```php:mail1.php
<?php

require_once("Mail.php");


/*** SMTPのパラメータ設定 ***/
/*
$params = array(
  "host" => "xxx.xxx.xx", // SMTPサーバ名
  "port" => 587, // ポート番号
  "auth" => true, // SMTP認証を使用する
  "username" => "xxx@xxx.xxx.xx", // SMTPのユーザー名
  "password" => "xxxxx" // SMTPのパスワード
);
*/


// 例) Yahoo Mailの場合
$params = array(
    "host" => "smtp.mail.yahoo.co.jp",
    "port" => 587,
    "auth" => true,
    "username" => "xxx@yahoo.co.jp",
    "password" => "xxxxxxx",
);

/*** PEAR::Mailのオブジェクト作成 ***/
$mailObject = Mail::factory("smtp", $params);

$recipients = "xxx@xxx.xx.xx"; // 送信先メールアドレス

/*** 本文 ***/
// 日本語メールを送る場合はエンコードが必要
$body = "test" . date('His');
$body = mb_convert_encoding($body, "ISO-2022-JP", "auto");

// メールヘッダ情報
$headers = array(
  "To" => "xxx@xxx.xxx.xx", // ここで指定したアドレスには送信されない
  "From" => "xxx@xxx.xxx.xx", // メールの差出人
  "Subject" => mb_encode_mimeheader("テストメール") // 日本語の件名を指定する場合はエンコード
);



/*** メール送信 ***/
// send(送信先, メールヘッダ, 本文)
$mailObject->send($recipients, $headers, $body);


```
### 例えばこんなコードになります
`elites_camp_201512@yahoo.co.jp` で 自分から自分に送信する

```php:sample.php
<?php

// テキストメールの送信

require_once("Mail.php");

// $params = array(
//   "host" => "xxx.xxx.xx", // SMTPサーバ名
//   "port" => 587, // ポート番号
//   "auth" => true, // SMTP認証を使用する
//   "username" => "xxx@xxx.xxx.xx", // SMTPのユーザー名
//   "password" => "xxxxx" // SMTPのパスワード
// );

// Yahoo Mailの場合の情報
$params = array(
  "host" => "smtp.mail.yahoo.co.jp",
  "port" => 587,
  "auth" => true,
  "username" => "elites_camp_201512@yahoo.co.jp",
  "password" => "elites201512",
);

// 送信先メールアドレス
$recipients = "elites_camp_201512@yahoo.co.jp";

// PEAR::Mailのオブジェクト作成
$mailObject = Mail::factory("smtp", $params);

$body = "test" . date('His');
$body = mb_convert_encoding($body, "ISO-2022-JP", "auto");

// メールヘッダ情報
$headers = array(
  "To" => "elites_camp_201512@yahoo.co.jp",
  "From" => "elites_camp_201512@yahoo.co.jp",
  "Subject" => mb_encode_mimeheader("テストメール") // 日本語の件名を指定する場合はエンコード
);



// メール送信
// send(送信先, メールヘッダ, 本文)
$mailObject->send($recipients, $headers, $body);
```
