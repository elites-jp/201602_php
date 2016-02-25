
## 1. 課題:PEARを使ってお問い合わせメールフォームをつくる
下記の要件に沿ってお問い合わせフォームを作成してみてください

### サンプル・アプリケーション
<a href="http://spartacamp.s15.coreserver.jp/elites_camp/contact_form.php" target="_blank">サンプル(基本要件のみ)はこちら</a>

<a href="http://spartacamp.s15.coreserver.jp/elites_camp/contact_form_design.php" target="_blank">サンプル(応用要件+装飾)はこちら</a>




### 基本要件（必須）
- フォームの項目は、「お名前」「件名」「メールアドレス」「本文」の4項目。
- フォームの項目が1つでも未入力の項目があれば、メールは送信されない。同時に、未入力の項目を赤文字で表示する。(例: 「件名が未入力です」)
- フォームのデザインをCSSによってある程度整えること。

### 応用要件（余力がある人向け）
-  「正規表現」について調べて、メールアドレスの形式が正しいかどうかを判別する機能を付け加える。


# サンプルコード

```php:mail_form.php
<?php

require_once("Mail.php");

$errors = array();

if ($_SERVER['REQUEST_METHOD'] === 'POST') {

    // フォームに入力された値を変数に格納
    $name = $_POST['name'];
    $title = $_POST['title'];
    $email = $_POST['email'];
    $content = $_POST['content'];

    // バリデーション
    if ($name == '') {
      $errors['name'] = 'お名前を入力してください';
    }

    if ($title == '') {
      $errors['title'] = '件名を入力してください';
    }

    if ($email == '') {
      $errors['email'] = 'メールアドレスを入力してください';
    }

    if ($content == '') {
      $errors['content'] = '本文を入力してください';
    }

    // エラーがなければメール送信処理
    if (empty($errors)) {
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

        // 送信内容の整形
        $body = "お問い合わせ内容\n";
        $body.= "氏名: " . $name . "\n";
        $body.= "件名: " . $title . "\n";
        $body.= "メールアドレス: " . $email . "\n";
        $body.= "問い合わせ内容\n";
        $body.= "----------------\n";
        $body.= $content . "\n";
        $body.= "----------------\n";

        $body = mb_convert_encoding($body, "ISO-2022-JP", "auto");

        // メールヘッダ情報
        $headers = array(
          "To"      => "elites_camp_201512@yahoo.co.jp",
          "From"    => "elites_camp_201512@yahoo.co.jp",
          "Subject" => mb_encode_mimeheader("お問い合わせ")
        );

        // メール送信
        // send(送信先, メールヘッダ, 本文)
        $mailObject->send($recipients, $headers, $body);
    }
}

// エスケープ処理を関数にしておく
function h($s) {
  return htmlspecialchars($s, ENT_QUOTES, 'UTF-8');
}
?>
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>お問い合わせフォーム</title>
</head>
<body>
<h1>お問い合わせフォーム</h1>
<?php if ($errors) :  ?>
    <?php foreach ($errors as $error) : ?>
        <li style="color: red;">
            <?php echo h($error); ?>
        </li>
    <?php endforeach; ?>
<?php endif; ?>
<form action="" method="post">
    <p>お名前(必須)<br/>
    <input type="text" name="name"></p>

    <p>件名(必須)<br/>
    <input type="text" name="title"></p>

    <p>メールアドレス(必須)<br/>
    <input type="text" name="email"></p>

    <p>メッセージ本文(必須)<br/>
    <textarea name="content" cols="30" rows="10"></textarea></p>

    <p><input type="submit" value="送信"></p>
</form>
</body>
</html>

```
