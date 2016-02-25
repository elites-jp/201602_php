# MySQL練習問題
何度も練習問題をやってみてください！
講義で扱っていない内容も一部存在しますが、そういうときは「MySQL ◯◯」などで検索しましょう！

# 1. 銀行口座データベース
## bank_accounts(口座)テーブル
| カラム  | データ型 | オプション | 備考 |
|:-------------------:|:-----------------:|:-----------------:|:---------------:|
| id     |  int       |  primary key, auto_increment| レコードのID |
| number | char(7)    | not null | 口座番号(7ケタ) |
| name   | varchar(50)|not null | 口座名義 |
| type   | char(1)    | not null | 口座種別(1:普通 2:当座 3:別段)|
| balance| int         | not null | 口座残高 |
| updated_at| datetime | -| 最終更新日時 |

## bank_accounts(口座)テーブルの中身
| id  | number |   name          | type  | balance  | updated_at          |
|:---:|:------:|:---------------:|:-----:|:-------:|:-------------------:|
|  1  | 0391834| カシワギ ショウタ | 1      | 1000000 | 2013-10-01 00:00:00 |
|  2  | 0649183| ヒダカ アユミ     | 1     | 500000  | 2014-11-01 00:00:00 |
|  3  | 0119245| オオヒラ ミチスケ | 1      | 25000   | 2015-12-01 00:00:00 |

### 基本問題
1. bank_accountsテーブルを作成する。
2. bank_accountsテーブルに次の3つのレコードを登録する。
3. bank_accountsテーブルのすべての口座番号を抽出する。
4. bank_accountsテーブルのすべての口座番号と残高を抽出する。
5. bank_accountsテーブルのすべてのレコードを抽出する。
6. bank_accountsテーブルのすべての残高を99999、更新日を「2015-01-01 00:00:00」にする。


### 検索条件
1. bank_accountsテーブルから、口座番号が「0649183」のレコードを抽出する。
2. bank_accountsテーブルから、残高が0より大きいレコードを抽出する。
3. bank_accountsテーブルから、口座番号が「0400000」番より前のレコードを抽出する。
4. bank_accountsテーブルから、更新日時が「2015年6月」以前のレコードを抽出する。
5. bank_accountsテーブルから「ショウ」を含む名義のレコードを抽出する。
6. bank_accountsテーブルから、口座番号順に全てのレコードを抽出する。ただし、並び替えは昇順にすること。
7. bank_accountsテーブルから、残高の大きい順に全てのレコードを抽出する。



# 2.商店データベース

## items(商品)テーブル
| カラム  | データ型   | オプション | 備考 |
|:-------:|:------------:|:-----------------:|:-----------------:|
| id      | int        | primary key, auto_increment| 商品のID |
| code    | char(5)    | not null | 商品コード |
| name    | varchar(50)| not null | 商品名 |
| price   | int        | not null | 価格   |
| type    | char(1)    | not null | 分類(1:衣類 2:靴 3:雑貨 9:未分類)|

## items(商品)テーブルの中身
| id  | code |   name          | price  | type  |
|:---:|:----:|:---------------:|:-----:|:-------:|
|  1  | S0425| 春のさわやかコート | 20000 |  1    |
|  2  | A0301| おしゃれな時計    |  5000 |   3 |
|  3  | M0219| 冬のあったかブーツ | 15000 |  2   |

### 基本問題
1. itemsテーブルを作成する。
2. itemsテーブルに上記のレコードを登録する。
3. itemsテーブルのすべての商品名を抽出する。
4. itemsテーブルのすべてのレコードを抽出する。

### 検索条件
1. itemsテーブルから、商品コードが「S0425」のデータを抽出する。
2. itemsテーブルから、商品コードが「A0301」の商品の単価を2500円に変更する。
3. itemsテーブルから、単価が15000円以下のレコードを抽出する。
4. itemsテーブルから、単価が20000円以上のレコードを抽出する。
5. itemsテーブルから、分類が「衣類」でないレコードを抽出する。
6. itemsテーブルから、商品コードが「M」で始まるレコードを削除する。
7. itemsテーブルから、商品名に「コート」が含まれる商品について、商品コード、商品名、単価を抽出する。



# 答え
# 1. 銀行口座データベース
## bank_accounts(口座)テーブル
### 基本問題
bank_accountsテーブルを作成する

```
create table bank_accounts(
id int primary key auto_increment,
number char(7) not null,
name varchar(50) not null,
type char(1) not null,
balance int not null,
updated_at datetime
);
```


bank_accountsテーブルに次の3つのレコードを登録する。

```
insert into bank_accounts (number, name, type, balance, updated_at) values
('0391834', 'カシワギ ショウタ', '1', 1000000, '2013-10-01 00:00:00'),
('0649183', 'ヒダカ アユミ'    , '1',  500000, '2014-11-01 00:00:00'),
('0119245', 'オオヒラ ミチスケ', '1',   25000, '2015-12-01 00:00:00');
```

bank_accountsテーブルのすべての口座番号を抽出する。

```
select number from bank_accounts;
```

bank_accountsテーブルのすべての口座番号と残高を抽出する。

```
select number, balance from bank_accounts;
```

bank_accountsテーブルのすべてのレコードを抽出する。

```
select * from bank_accounts;
```

bank_accountsテーブルのすべての口座種別を2にする。

```
update bank_accounts set type = 2;
```

### 検索条件
bank_accountsテーブルから、口座番号が「0649183」のレコードを抽出する。

```
select * from bank_accounts where number = '0649183';
```

bank_accountsテーブルから、残高が0より大きいレコードを抽出する。

```
select * from bank_accounts where balance > 0;
```

bank_accountsテーブルから、口座番号が「0400000」番より前のレコードを抽出する。

```
select * from bank_accounts where number < '0400000';
```

bank_accountsテーブルから、更新日時が「2015年1月1日」以前のレコードを抽出する。

```
select * from bank_accounts where updated_at <= '2015-01-01';
```

bank_accountsテーブルから「ショウ」を含む名義のレコードを抽出する。
```
select * from bank_accounts where name like '%ショウ%';
```

bank_accountsテーブルから、口座番号順に全てのレコードを抽出する。ただし、並び替えは昇順にすること。

```
select * from bank_accounts order by number;
```

bank_accountsテーブルから、残高の大きい順に全てのレコードを抽出する。

```
select * from bank_accounts order by balance desc;
```



# 2.商店データベース

## items(商品)テーブル
### 基本問題
itemsテーブルを作成する。

```
create table items (
id int primary key auto_increment,
code char(5) not null,
name varchar(50) not null,
price int not null,
type char(1) not null
);
```

itemsテーブルに、レコードを登録する。

```
insert into items (code, name, price, type) values
('S0425', '春のさわやかコート', 20000, '1'),
('A0301', 'おしゃれな時計'    ,  5000 ,'3'),
('M0219', '冬のあったかブーツ', 15000 ,'2');
```

itemsテーブルのすべての商品名を抽出する。

```
select name from items;
```

itemsテーブルのすべてのレコードを抽出する。

```
select * from items;
```

### 検索条件
itemsテーブルから、商品コードが「S0425」のデータを抽出する。

```
select * from items where code = 'S0425';
```

itemsテーブルから、商品コードが「A0301」の商品の単価を2500円に変更する。

```
update items set price = 2500 where code = 'A0301';
```

itemsテーブルから、単価が15000円以下のレコードを抽出する。

```
select * from items where price <= 15000;
```

itemsテーブルから、単価が20000円以上のレコードを抽出する。

```
select * from items where price >= 20000;
```

itemsテーブルから、分類が「衣類」でないレコードを抽出する。

```
select * from items where type <> '1';
```

itemsテーブルから、商品コードが「M」で始まるレコードを削除する。

```
select * from items where code like 'M%';
```

itemsテーブルから、商品名に「コート」が含まれる商品について、商品コード、商品名、単価を抽出する。

```
select code, name, price from items where name like '%コート%';
```







