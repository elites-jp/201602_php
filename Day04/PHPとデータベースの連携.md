# PHPとデータベースの連携
今まではターミナル上で直接SQLを実行してデータベースをいじっていた。
ここでは、PHPのコードによってデータベースを操作する方法を学習します。

## 1. PDOクラスとは
データベースに接続する際には`PDOクラス`を利用します。
`PDOクラス`とはデータベース接続用のクラスで、要するにコレを利用することによってPHPのコードからデータベース操作が可能になります。

もちろん、クラスについては十分に学習していないので、意味不明な部分も多いと思いますが、
データベースの操作自体は、機械的な手順を踏むことで比較的カンタンに操作ができるので安心してください。

[ちなみにPDOの公式マニュアル](http://php.net/manual/ja/class.pdo.php)

## 1.2. データベースの作成
まずは、データベースを準備します。

```sql:commands.sql
-- データベースの作成
create database nowall;

-- 作業ユーザーとパスワードの設定
-- 「nowall」というデータベースの全てのテーブルの操作権限を「testuser」に付与。パスワードは「9999」という意味。
grant all on nowall.* to testuser@localhost identified by '9999';

-- 使用するデータベースの選択
use nowall

-- テーブルの作成
create table members (
id int primary key auto_increment,
name varchar(32),
email varchar(100),
password varchar(32)
);

-- テストレコードの挿入
insert into members (name, email, password) values
  ('kashiwagi', 'kashiwagi@example.com', '1111'),
  ('hidaka', 'hidaka@example.com', '2222');
```

### `GRANT文`
データベースの操作権限を設定するコマンド。

今まで使っていた`mysql -u root -p`というのは、「rootユーザー(全ての権限を持つ最強のユーザー)」でログインするという意味。

誰でも全てのデータベースを扱えるようにしてしまうと、意図しないデータベースのレコードを変更してしまったり、意図的にデータベースの改ざんなどができてしまう。

そこで、「どのユーザーが、どの程度の範囲の操作権限を持つのか」という設定を行うのが基本。それが`GRANT文`。

```sql:GRANT文の構文
-- 「データベース」の全てのテーブルの操作権限を「testuser」に付与します。
--「testuser」のログインパスワードは「パスワード」です。
grant all on データベース名.* to ユーザ名@localhost identified by 'パスワード'
```

## 1.3. データベースへの接続
実際にデータベースへの接続をしてみる。

```php:connect_db.php
<?php

// 接続に必要な情報を定義
define('DSN', 'mysql:host=localhost;dbname=nowall;charset=utf8;');
define('USER', 'testuser');
define('PASSWORD', '9999');


// try ~ catch 構文
// ここはコピペでOK
// 気になる人は「例外処理」で検索
try {
    $dbh = new PDO(DSN, USER, PASSWORD);
    echo '接続に成功しました！' . '<br>';
} catch (PDOException $e) {
    // 接続がうまくいかない場合こちらの処理がなされる
    echo $e->getMessage();
    exit;
}
/* try ~ catch 命令 *
try {
  // 例外発生する可能性がある処理
} catch(発生するかもしれない例外の種類 例外を受け取る変数名) {
  // 例外発生時の処理
}

*/

/* PDOクラスの書き方 *

new PDO($dsn, $user, $password);
$dsn     : データベース接続用の文字列 mysql:host=localhost;dbname=nowall;charset=utf8;
$user    : 接続するときのユーザー名
$password: 接続する際のパスワード

*/
```


## 2. PDOでCRUD処理
無事、データベースとの接続ができたので、CRUD処理をPHPのコードからやってみる。

### 基本的な手順
1. SQL文の組み立て
2. プリペアードステートメントの準備
3. 必要があれば、パラメータのバインド(プレースホルダへの代入)
4. プリペアードステートメントの実行
5. 結果の受け取り

### 2.1. レコードの表示

```php:select.php
<?php

// 接続処理は省略

// SQL文の組み立て
$sql = "select * from members";

// プリペアードステートメントの準備
$stmt = $dbh->prepare($sql);

// プリペアードステートメントの実行
$stmt->execute();

// 結果の受け取り
$row = $stmt->fetchAll(PDO::FETCH_ASSOC);

// var_dump($row);

foreach ($row as $member)
{
    echo $member['name'] . 'さん<br>';
}
```

### 2.1.2. SQLの実行結果の受け取り方

#### `fetchAll()`
`$stmt->execute()`とすることによって、指定したSQL(今回であればSELECT文)が実行されます。
その実行結果(membersテーブルの全てのレコード)を受け取りたいときに、`fetchAll()`を使います。
`fetchAll()`を使うと、実行結果を配列として受け取ることができます。

#### `PDO::FETCH_ASSOC`
`fetchAll()`を使うと、実行結果を配列として受け取ることができますが、これにオプションをつけることができます。
オプションに`PDO::FETCH_ASSOC`を指定すると、カラムをキーとした連想配列が返されます。

#### `fetch()`
ちなみに、1行分のレコードであれば`fetch()`という命令も使えるが、とりあえず今は`fetchAll(PDO::FETCH_ASSOC)`でOKです。

### 2.2. レコードの追加

```php:insert.php
<?php

// 接続処理は省略

// SQL文の組み立て
$sql = "insert into members (name, email, password) values (:name, :email, :password)";

// プリペアードステートメントの準備
$stmt = $dbh->prepare($sql);

// バインド(代入)するパラメータの準備
$name = "ohira";
$email = "ohira@example.com";
$password = "3333";

// パラメータのバインド(プレースホルダへの代入)
$stmt->bindParam(":name", $name);
$stmt->bindParam(":email", $email);
$stmt->bindParam(":password", $password);

// プリペアードステートメントの実行
$stmt->execute();
```

### 2.2.1. プレースホルダ
実はプレースホルダ( :name とか :email のこと)を使わなくても実行可能(placeholder.php)ではあるが、繰り返し処理するときにいちいちSQLを組み立てないといけなくなってしまう。


```php:placeholder.php
<?php

// 接続処理は省略

$sql = "insert into members (name, email, password) values ('test', 'test@test.com', '0000')";
$stmt = $dbh->prepare($sql);
$stmt->execute();
```

例えば、フォームに入力された値をデータベースに追加するときに、いちいちSQLの準備しなくてはならない。"insert ~ values " までは同じSQL文であり、違いがあるのは "values" 以降 なのだから、そこだけ可変にすれば良いですよね？ ということ。

### プレースホルダの命名について
`:`の後ろに好きな名前をつけることができる。
ただし、どういうパラメータを代入するかの予想がつく方が間違えにくいので、基本的にはカラム名をそのまま利用する。(:name, :email, :password など)


### 2.2.2. プリペアードステートメントとプレースホルダによる繰り返し処理
一度SQL文を用意しておけば使い回すことができる。
実はセキュリティ的にも強い(SQLインジェクション対策のひとつ)

```php:prepared_statement.php
<?php

// 接続処理は省略
$sql = "insert into members (name, email, password) values (:name, :email, :password)";
$stmt = $dbh->prepare($sql);

$stmt->bindParam(":name", $name);
$stmt->bindParam(":email", $email);
$stmt->bindParam(":password", $password);

$name = "ohara";
$email = "ohara@example.com";
$password = "4444";
$stmt->execute();

$name = 'yamada';
$email = 'yamada@example.com';
$password = '5555';
$stmt->execute();
```



### 2.3. レコードの更新
「どのレコードを更新するか？」という情報が必要なので、プレースホルダを利用してレコードのidを指定する。

```php:update.php
<?php

// 接続処理は省略

// 更新情報の準備
$id = 1;
$name = 'Kashiwagi Shouta';

// SQLの組み立て
$sql = "update members set name = :name where id = :id";

// プリペアードステートメントの準備
$stmt = $dbh->prepare($sql);

// パラメータのバインド
$stmt->bindParam(":id", $id);
$stmt->bindParam(":name", $name);

// プリペアードステートメントの実行
$stmt->execute();
```

### 2.4. レコードの削除
単純なDELETE文を実行すると全てのレコードが消えてしまうので、基本的にはWHERE句を用いてレコードを指定する。

```php:delete.php
<?php

// 接続処理は省略

// 削除対象のidの定義
$id = 1;

// SQLの組み立て
$sql = "delete from members where id = :id";

// プリペアードステートメントの準備
$stmt = $dbh->prepare($sql);

// パラメータのバインド
$stmt->bindParam(":id", $id);

// プリペアードステートメントの実行
$stmt->execute();
```

## 3. PDOによるデータベース操作手順のまとめ

```php:
// データベースへの接続
$dbh = new PDO(DSN, USER, PASSWORD);

// SQL文の組み立て
$sql = "insert into sample_table (column1, column2) values (:value1, :value2)";

// プリペアードステートメントの準備
$stmt = $dbh->prepare($sql);

// 必要があれば、パラメータのバインド(プレースホルダへの代入)
$stmt->bindParam(":value1", 値1);
$stmt->bindParam(":value2", 値2);

// プリペアードステートメントの実行
$stmt->execute();

// 結果の受け取り(指定の仕方いろいろ)
$data = $stmt->fetchAll(PDO::FETCH_ASSOC);
```










