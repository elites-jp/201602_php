# MySQLの基本

## 1. データベースとは
ざっくり言うとデータの集合のこと。
初めのうちは、ExcelファイルのようなものというイメージでOKです。

データベースにはいくつかの種類がありますが、今回はMySQLというものを使用します。

![mysql参考.gif](https://qiita-image-store.s3.amazonaws.com/0/79919/08d719b4-51e2-3303-7d2a-7ee24447cabd.gif)
図1. データベース参考画像(http://www.hominid.co.jp/term/t/database)

### 1.1. 用語の整理
必要な用語は、次の4つです。

- `データベース`
    - `テーブル`
        - `カラム`
        - `レコード`


![データベース用語.png](https://qiita-image-store.s3.amazonaws.com/0/79919/ffe64d5c-7b76-86e9-4440-e9f6914698f0.png)
図2. データベース用語(http://www.pahoo.org/e-soul/webtech/php07/)


#### `データベース`
データベースの中に色々なテーブルが格納されます。
基本的に1つのプロジェクト(サービス)に対して1つのデータベースを作ります。
例えば、顧客管理サービスをつくるなら、customer_managementデータベースを1つ作ります。

#### `テーブル`
データベースの中にたくさんのテーブルが所属します。
例えば、顧客管理サービスなら、「顧客情報テーブル」「取引情報テーブル」「資料テーブル」などを作ります。

#### `カラム`
テーブル内の列(縦のライン)を示します。

#### `レコード`
テーブル内の行(横のライン)を示します。


### 1.2. MySQLの起動と終了
下記コマンドを実行する。
パスワードは入力しても特に見た目に変化がないが、ちゃんと入力されている。

```bash:mysqlの起動
[vagrant@localhost ~]$ mysql -u root -p
Enter password:
```

### 1.3. とりあえず使ってみる
基本的にターミナル上でSQLと呼ばれるコマンドを実行していきます。
SQLについてはとにかく慣れです！ どれだけコマンドを叩いたかで熟練度が定まるので、たくさん練習しましょう。

```sql:commnads_1.sql

-- データベースの作成
-- create database データベース名
create database test;

-- データベースの一覧表示
show databases;

-- データベースの削除
-- drop databae データベース名
drop database test;

-- MySQLの終了
exit;

```

### 1.4. もうちょい使ってみる

```sql:commands_2.sql
-- データベースの作成
create database nowall;

-- データベースの一覧表示
show databases;

-- 使用するデータベースを選ぶ
use nowall;

-- テーブルを確認
show tables;

-- この時点ではテーブルは存在しないので新規テーブル作成
-- create table テーブル名 (カラム名1 データ型, カラム名2 データ型 , ...)
create table members (id int, name varchar(32));

-- 再びテーブルを確認
show tables;

-- テーブルの構造を確認
desc members;

-- テストデータを突っ込む
insert into members (id, name) values (1, 'kashiwagi');
insert into members (id, name) values (2, 'hidaka');
insert into members (id, name) values (3, 'ohira');

-- membersテーブルのデータを全て表示
select * from members;

-- テーブルの削除
drop table members;
```

## 2. データ型
PHPのデータ型と同様にデータベースにもカラムのデータ型が存在する
※詳しくは公式のデータタイプを見ましょう

#### `数値型`
整数: int
小数: float

#### `文字列型`
固定長: char()
可変長: varchar()
テキスト(長い文字列): text

#### `日付・時刻型`
日付と時刻: datetime
日付: date
時刻: time


## 3. CRUD操作
CRUDとは、データベース管理システムに必要な4つの機能のこと。
`Create` データの作成
`Read`   データの表示
`Update` データの更新
`Delete` データの削除

### 3.1. レコードの追加と表示

```sql:commands_3.sql
-- テーブルをつくる
create table members (
  id int,
  name varchar(32),
  password char(4)
  );

-- insert文
-- 1つのカラムにレコードを入れるとき
-- insert into テーブル名 (カラム名) values (挿入するレコード);
insert into members (id) values (1);

-- 複数のカラムにレコードを入れるとき
-- insert into テーブル名 (カラム1, カラム2) values (レコード1, レコード2);
insert into members (id, name, password) values (2, 'user2', '2222');

-- レコードの表示
-- select文
-- テーブルの全てのレコードを見る
-- select * from テーブル名;
select * from members;

-- 特定のカラムのレコードを見る
-- select カラム名 from テーブル名;
select id from members;

```

### 3.2. 練習問題
1. MySQLを起動する
2. データベース`projects`を作る
3. そのデータベースで指定されたテーブル`products`を作る(「productsテーブルの構造」参照)
4. その構造を表示させる
5. レコードを挿入する(「productsテーブル内のレコード」参照)
6. レコードを表示する
7. テーブルを削除する
8. データベースを削除する

#### 作りたいテーブルの構造

```txt:productsテーブルの構造
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(32) | YES  |     | NULL    |       |
| price | int(11)     | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
```

#### レコード挿入後の表示結果

```txt:productsテーブル内のレコード
+------+-----------+-------+
| id   | name      | price |
+------+-----------+-------+
|    1 | りんご    |    10 |
|    2 | いちご    |    15 |
|    3 | みかん    |    20 |
+------+-----------+-------+
```

```sql:commands_4.sql
-- データベース`projects`を作る

-- そのデータベースで指定されたテーブル`products`を作る(「productsテーブルの構造」参照)

-- その構造を表示させる

-- レコードを挿入する(「productsテーブル内のレコード」参照)

-- レコードを表示する

-- テーブルを削除する

-- データベースを削除する

```

## 答え

```sql:

-- データベースを作る
create database projects;

-- そのデータベースで指定されたテーブルを作る
use projects;
create table products (id int, name varchar(32), price int);

-- その構造を表示させる
desc products;

-- レコードを挿入する
insert into products (id, name, price) values
(1, 'りんご', 10),
(2, 'いちご', 15),
(3, 'みかん', 20);

-- レコードを表示する
select * from products;

-- テーブルを削除する
drop table products;

-- データベースを削除する
drop database projects;


```
## 4. カラムの設定
カラムにはいろいろとオプションを設定することが出来る。



### 4.1. 主キー(Primary Key)
レコードのidは次のような特徴がある。
1. 1から始まる
2. 1ずつ増えていく
3. 同一のidは存在しない
4. 値が入らないことがない

ただ、手動でidを入力する場合、
1. 間違ったidを入力してしまう可能性がある
2. 同一のidを入力してしまっていても気づかない可能性がある
3. そもそも入力が面倒(1ずつ増えるとわかりきっているならいちいち打ちたくない)

それらを解決策が`主キー(Primary Key)`や`自動連番(Auto Increment)`


```sql:primary_key.sql
-- membersテーブルを一旦削除
drop table members;

create table members (
id int primary key,
name varchar(32),
password char(4)
);

-- テストレコードを入れる
insert into members (id ,name) values
(1, 'kashiwagi');


-- 同じidのレコードを入れようとしてみる
insert into members (id ,name) values
(1, 'hidaka');
-- ERROR 1062 (23000): Duplicate entry '1' for key 'PRIMARY'


-- NULLを入れようとしてみる
insert into members (id) values (NULL);
-- ERROR 1048 (23000): Column 'id' cannot be null
```

### 4.2. 自動連番(Auto Increment)
先述の「1ずつ増える」というのを自動でやってくれるオプション

```sql:auto_increment.sql
drop table members;

-- id に auto_increment を追加
create table members (
id int primary key auto_increment,
name varchar(32)
);

-- 構造が変わっているかの確認
desc members;

-- テストレコードを入れる(このとき id は指定していない！)
insert into members (name) values
  ('kashiwagi'),
  ('hidaka'),
  ('yamada');

-- 中身を確認(ちゃんとidが入っている)
select * from members;

```

```txt:membersテーブルの中身
+----+-----------+
| id | name      |
+----+-----------+
|  1 | kashiwagi |
|  2 | hidaka    |
|  3 | yamada    |
+----+-----------+
```


### 4.3. デフォルト値
カラムのデフォルト値を設定することもできる。
これにより、入力のミスを事前に防いだりできる。
例えば、勤続年数を追加してみる(勤続年数は最初は誰でも0)。

```sql:default.sql
drop table members;

-- 勤続年数(length_of_service)の追加
create table members (
  id int primary key auto_increment,
  name varchar(32),
  length_of_service int default 0,
  password char(4)
  );

-- 構造の確認
desc members;

insert into members (name, password) values ('kashiwagi', '1111');

-- 中身の確認(length_of_yearsに注目)
select * from members;

```


## 5. 特定のレコードを操作する
今までのSELECT文は"select * from members"のように、該当テーブルの全てのレコードを抽出していた。
しかし、当然ながら条件を指定したうえでのレコードの操作(表示/追加/編集/削除)も可能。そして、非常に良く使う。

例1) id が 3 のレコードを表示する
例2) 売上 が 150以上のレコードを 3件だけ 表示する
例3) staff_id が 5 のレコードの 売上 に 200 を挿入する
例4) 名前 が 柏木 のレコードの 年齢 を 22 に変更する
例5) 製造年月日 が 2012年以前のレコードを 削除する

などなど
### まずはテストレコードの準備

```sql:sample_record.sql
-- テーブルの作成
create table sales (
 id int primary key auto_increment,
 name varchar(32),
 sale int,
 created_at datetime
 );

-- データの挿入
insert into sales (name, sale, created_at) values
  ('hokkaido', 150, '2015-01-01 12:00:00' ),
  ('tohoku',   200, '2015-02-01 12:00:00' ),
  ('kanto',    500, '2015-03-01 12:00:00' ),
  ('chubu',    300, '2015-04-01 12:00:00' ),
  ('kinki',    400, '2015-05-01 12:00:00' ),
  ('chugoku',  180, '2015-06-01 12:00:00' ),
  ('shikoku',  140, '2015-07-01 12:00:00' ),
  ('kyushu',   120, '2015-08-01 12:00:00' );
```


### 5.1 WHEREとLIMIT

#### `LIMIT`
表示する件数の指定ができる。

#### `WHERE`
検索条件の指定ができる。


```sql:where_limit.sql
-- 表示件数の指定
select * from sales limit 4;


-- 例えば、売上が◯◯より大きい
-- where と 比較演算子を使う
select * from sales where sale > 150;

select name, sale from sales where sale > 200;

-- whereとlimitの組み合わせ
select * from sales where sale > 150 limit 3;
```


### 5.2 WHEREとLIKE
whereを使うと操作対象レコードを指定できる。
select文にかぎらず、insert, update, deleteでも使用可能。

```sql:where_like.sql
-- 特定条件で検索
select * from sales where name = 'kanto';
select * from sales where id = 3;

-- あいまい検索 like
select * from sales where sale like '2%';
select * from sales where sale like '15%';
select * from sales where name like 'k%';

-- 複数の条件指定
-- 売上が200以上かつ400未満(AND)
select * from sales where sale >= 200 and sale < 400;

-- 売上が200未満、または400以上
select * from sales where sale < 200 or sale >= 400;
```

### 5.3 ORDER BY
#### `ORDER BY`
表示順序の並べ替えができる。
例) 売上が大きい順、年齢の若い順 など

```sql:order_by.sql
-- 売上の小さい順(昇順)に表示
select * from sales order by sale asc;

-- 売上の大きい順(降順)に表示
select * from sales order by sale desc;

-- 表示するレコードの範囲を決める
select * from sales order by sale desc limit 2 offset 3;

select * from sales order by sale desc limit 2 offset 4;
```

### 5.4. count()
#### `count()`
レコード件数を調べる

```sql:count.sql
select count(sale) from sales;
select count(*) from sales;

```

## 6. CRUD操作その2
CRUD操作のうち、CRは既に勉強しました
`Create(データの作成): INSERT文`
`Read(データの表示): SELECT文`

ここでは残りのUDを勉強します。
`Update(データの更新): UPDATE文`
`Delete(データの削除): DELETE文`


### 6.1. レコードの更新
更新するレコードを指定しない場合、全てのレコードに影響が出てしまう！
なので、WHERE句と組み合わせるのが基本。
ここで、idが主キー(同一IDがない)という特性が効果を発揮する。

```sql:update.sql
-- UPDATE テーブル名 SET カラム名 = 値

-- idが3のレコードのnameをtokyoに変更
update sales set name = 'tokyo' where id = 3;

-- idが17のレコードのnameをHOKKAIDOに変更
update sales set name = 'HOKKAIDO' where id = 17;

-- 注意！: 全てのレコードを変更してしまう！
update sales set name = 'japan';

```

### 6.2. レコードの削除
レコードを削除を安易に実行するのは危険なので、気をつけること！
WHERE句と組み合わせて使うのが基本。

```sql:delete.sql
-- レコードが全て消えてしまうので注意！
delete from sales;

-- 再びテストレコード挿入
insert into sales (name, sale, created_at) values
  ('hokkaido', 150, '2015-01-01 12:00:00' ),
  ('tohoku',   200, '2015-02-01 12:00:00' ),
  ('kanto',    500, '2015-03-01 12:00:00' ),
  ('chubu',    300, '2015-04-01 12:00:00' ),
  ('kinki',    400, '2015-05-01 12:00:00' ),
  ('chugoku',  180, '2015-06-01 12:00:00' ),
  ('shikoku',  140, '2015-07-01 12:00:00' ),
  ('kyushu',   120, '2015-08-01 12:00:00' );

-- 条件を指定して削除
-- idが12のレコードを削除
delete from sales where id = 12;

-- saleが200未満のレコードを削除
delete from sales where sale < 200;

```

## 7. 他のコマンド
上記のコマンド以外にもいろいろなコマンドがあります(例えば、カラムを新たに追加するなど)。
それらのコマンドは記憶するというよりも、必要があったときに調べればOKです。

## 8. 基礎コマンドまとめ
一部紹介していないものもあります。

### データベース操作
データベースをつくる

```
create database データベース名;
```

データベース一覧を表示する

```
show databases;
```

データベースを削除する

```
drop database データベース名;
```

使用するデータベースを選択する

```
use データベース名
```

作業ユーザーを設定する

```
grant all on データベース名.* to ユーザー名@localhost identified by 'パスワード';
```

### テーブル操作
テーブルの一覧を表示する

```
show tables;
```

テーブルをつくる

```
create table テーブル名 (
カラム名 データ型 オプション,
カラム名 データ型 オプション
);
```

テーブルの構造を表示する

```
desc テーブル名;
```

テーブルを削除する

```
drop table テーブル名;
```

テーブル名の変更

```
alter table テーブル名 rename as 新テーブル名;
```

既存のカラムの構造の変更

```
alter table テーブル名 change 既存のカラム名 新しいカラム名 データ型 オプション;
```

新規カラムの追加

```
alter table テーブル名 add 新規カラム名 データ型 オプション;
```


### レコード検索

```
select * from テーブル名;
select * from where カラム名 = 値;
```

### レコード追加

```
insert into テーブル名 (カラム名1, カラム名2,...) values (値1, 値2,...);
```

### レコード更新

```
update テーブル名 set カラム名 = 値;
```

### レコード削除

```
delete from テーブル名;
delete from テーブル名 where カラム名 = 値;
```












