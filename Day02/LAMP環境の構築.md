# LAMP環境の構築

## 1. はじめに
- 環境構築とは「自分のPCでPHPの実行環境を作成すること」
- ここではLAMP(Linux/Apache/MySQL/PHP)環境を構築することを目指します
- 広い意味で言えば、エディタなども含まれる
- ターミナルでのUNIXコマンドを多用するので慣れていないと意味不明かもしれないけど大丈夫
    - ディレクトリの概念
    - Tabによる候補
    - cd
    - mkdir
    - vi
    - sudo

## 2. 使用するツール
### Vagrant
- カンタンに開発用の仮想マシン環境を構築できるツール
- [Vagrantダウンロード](https://www.vagrantup.com/downloads.html)

### VirtualBox
- 仮想マシンをタダで作れるツール
- みなさんのMacBookの中に、パソコンを追加するイメージ
- [VirtualBoxダウンロード](https://www.virtualbox.org/wiki/
Download_Old_Builds_4_3)
- ※ 最新版は5系だが、今回は4.3系を利用

### Cyberduck
- 自分のPCからWebサーバにファイルを転送するためのツール
- FTP(File Transfer Protocol)ソフトとも言ったりする
- [Cyberduckダウンロード](https://cyberduck.io/index.ja.html?l=ja)

### ターミナル(Mac標準ソフト)
- コマンドと呼ばれる命令文を用いてMacの操作や設定を行うアプリケーション
- キーボードでMacと対話するイメージ

## 3. ざっくり手順
1. ソフトのダウンロードおよびインストール
2. Vagrantの初期化
3. 仮想マシンの設定
4. リポジトリの追加
5. PHPの設定
6. MySQLの設定
7. CyberDuckの設定
8. おしまい！

こりゃ大変！
環境構築はプログラミング学習における最初で最大の難関。

というわけでラクをします！


## 一気に環境構築
何をやっているかわからないけど、何をやっているか分かる必要もありません(今は)。
PHPをある程度学習した時に環境構築の勉強をしましょう。


```bash:
$ cd
$ mkdir elites_camp_201602
$ cd elites_camp_201602
$ vagrant init bento/centos-6.7
$ vi Vagrantfile
-- 編集開始
矢印キーで移動
#の上でxを押す
:wq
-- 編集終了
$ vagrant up
$ vagrant ssh
$ sudo yum -y install git
$ git clone https://github.com/dotinstallres/centos65.git
$ cd centos65
$ ./run.sh
```

しばらく時間がかかるので待機します。
その間にエディタの設定などを行っていきます。
[Slackやエディタの設定はこちら](http://qiita.com/mohira/private/e38f38be561a331d8569)

## 終わったら

```bash:
$ vagrant ssh
$ php -v
$ mysql --version
$ sudo service httpd status
```


## Cyberduckの設定
・Cyberduckを起動する
・ファイル→新規ブラウザ
・左上の「新規接続」
・下記の内容を入力

```txt:接続情報
接続方式: SFTP
サーバ: 192.168.33.10
ユーザ名: vagrant
パスワード: vagrant
パス: /var/www/html
```

- 外部エディタの設定をSublimeTextにする
- ダブルクリックするとエディタが立ち上がるようにする
- 他の設定はお好みで

## PHPをコードを試してみる
- 実際にPHPが動くか試してみる

```bash:
$ sudo chmod -R 777 /var/www/html ### パーミッションの変更
$ cd /var/www/html
$ vi index.php
```

```php:index.php
<?php
    phpinfo();
```

ここまでできたら 192.168.33.10/index.php にアクセス！



# 以下、手動で環境を作ってみたい人向け

## 1. ソフトのダウンロードとインストール

### Vagrantのインストール
- インストーラを起動して「続ける」をポチポチするだけ
- <a href="http://weblabo.oscasierra.net/install-vagrant-onto-macosx/" target="_blank">参考ページ</a>

### VirtualBoxのインストール
- これもインストーラの流れに沿って「続ける」をポチポチするだけ
- <a href="http://pc-karuma.net/mac-virtualbox-install/" target="_blank">参考ページ</a>

### Cyberduckとインストール
- あとでやります

### ターミナルのインストール
- 最初から入ってます

## Vagrantの初期化

### ディレクトリの作成
- elitesという新規フォルダを作って、そのフォルダをダブルクリックする動きをコマンドでやっているだけ

ターミナルを起動する(1か2のどちらかの方法でOK)
1. [アプリケーション] → [ユーティリティ] → [ターミナル]
2. 右上の虫眼鏡アイコン(control + スペース) → 「ターミナル」と入力して、return

```bash:ディレクトリの作成
$ cd
$ mkdir elites_camp_201602
$ cd elites_camp_201602
```

### Vagrantの初期化

```bash:Vagrantの初期化
$ vagrant init bento/centos-6.7
````

### Vagrantfileの修正

```bash:Vagrantfileの修正
$ vi Vagrantfile
```

```bash:Vagrantfileの修正
config.vm.network "private_network", ip: "192.168.33.10" ### 先頭の「#」を削除するだけ
```


## 仮想マシンの設定

```bash:仮想マシンの設定

$ vagrant up   ### 仮想マシンの起動コマンド
$ vagrant ssh  ### 仮想マシンにssh接続

$ sudo yum update -y ### 色んなファイルなどを最新版にアップデート

$ sudo service iptables stop  ### ファイアウォールの停止(本番環境では必須)
$ sudo chkconfig iptables off ### ファイアウォールが常に停止するように設定

$ sudo yum install -y httpd   ### Webサーバーのインストール

$ sudo service httpd start ### Webサーバーの立ち上げ
$ sudo chkconfig httpd on  ### 常に立ち上げるように設定
```


## リポジトリの追加

```bash:リポジトリの追加その1
### epelのダウンロード
$ wget https://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

### remiのダウンロード
$ wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm

### リポジトリの追加
$ sudo rpm -Uvh epel-release-6-8.noarch.rpm
$ sudo rpm -Uvh remi-release-6.rpm
```

```bash:/etc/yum.repos.d/epel.repo
$ sudo vi /etc/yum.repos.d/epel.repo

--
「enable=1」を「enable=0」に編集
--
```


```bash:リポジトリの追加その2
$ yum info --enablerepo=remi php
```

### PHP関連のいろいろインストール

```bash:いろいろインストール
$ sudo yum --enablerepo=remi install -y php php-devel php-mysql php-mbstring php-gd
```

## PHPの設定
- php.ini(PHPの設定ファイル)を修正していく
- 設定ファイルを修正したら再起動！

```bash:php.iniの編集
$ sudo vi /etc/php.ini
```

```txt:
コマンドモードで /log_errors = On で検索
error_log = /var/log/php.log を追記

コマンドモードで /display_errors = Off で検索
display_errors = Off を
display_errors = On に変更


コマンドモードで /mbstring.language = で検索
;mbstring.language = Japanese の セミコロンを削除


コマンドモードで /mbstring.internal_encoding = ECU-JP で検索
mbstring.internal_encoding = ECU-JP を
mbstring.internal_encoding = UTF-8 に変更


コマンドモードで /;mbstring.http_input = auto で検索
;mbstring.http_input = auto の セミコロンを削除


コマンドモードで /;mbstring.detect_order = auto で検索
;mbstring.detect_order = auto の セミコロン削除


コマンドモードで /expose_php = On で検索
expose_php = On を
expose_php = Off に変更


コマンドモードで /date.timezone = で検索
date.timezone = Asia/Tokyo に変更
```

```bash:apacheの再起動
$ sudo service httpd restart
```


## MySQLの設定
- MySQL(データベース)のインストールと設定を行う
- MySQLの設定ファイルは my.cnf

```bash:MySQLのインストール
$ sudo yum install -y --enablerepo=remi mysql-server
$ sudo vi /etc/my.cnf
```

```txt:/etc/my.cnf

---- これを4行目に追記 ----
character_set_server=utf8
default-storage-engine=InnoDB
innodb_file_per_table
[mysql]
default-character-set=utf8
[mysqldump]
default-character-set=utf8
[mysqld]
default-time-zone='+9:00'
---- ここまで ----
```

![mysql3.gif](https://qiita-image-store.s3.amazonaws.com/0/79919/16a7427e-da99-8d83-4861-5f4b37247dbf.gif)


```bash:MySQLの起動
$ sudo service mysqld start ### MySQLの起動
```

```bash:MySQL設定
$ /usr/bin/mysql_secure_installation
```

設定に関する質問が来るので、下記のように回答する。

```
Enter current password for root (enter for none):
Enterを入力

Set root password? [Y/n]
Yを入力

New password:
rootを入力
※入力しても画面上に変化がないので注意！

Re-enter new password:
rootを入力
※再度入力する

コレ以降の質問は全てEnter入力でOK！

----
```

```bash:MySQL自動起動設定
$ sudo chkconfig mysqld on
```

