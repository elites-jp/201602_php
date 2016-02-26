# GitHub入門
リモートリポジトリの話ですね。
集団での開発には欠かせません。

## 1. GitHubとは
- みんなソースコードを共有できる
- こやつのおかげでスムーズに共同開発できるわけですよ

## 1.1. GitHubアカウントの登録
既に完了しています

## 1.2. 公開鍵の登録
[ここの解説ページが分かりやすい](http://monsat.hatenablog.com/entry/generating-ssh-keys-for-github)

ちなみに、公開鍵暗号という方式を用いるのですが、分からなくてOK(気になる方は質問してください)

公開鍵と秘密鍵の生成

```bash:
$ ls ~/.ssh
$ ssh-keygen -t rsa -C "your_email@example.com"
$ cat ~/.ssh/id_rsa.pub
```

## 2. リモートリポジトリ操作
## 2.1. リモートリポジトリの追加
- githubでリポジトリ作成
- terminalでリモートリポジトリを追加

```bash:
$ git remote add origin git@github.com:mohira/git_test.git
$ git remote -v
$ git push -u origin master
```

できた！


## 2.2. リモートリポジトリへの反映(push)
- ちなみに、 -u をつtけておくと、ローカルリポジトリの現在のブランチのupstreamはoriginリポジトリのmasterブランチであることを設定している。これにより、pullするときに、オプションあんしでも、ローカルリポジトリのブランチは、originのmasterから取得するように鳴る


```bash:
$ git checkout -b topic-E
$ vi README.md
$ git commit -am "Add topic-E"
$ git checkout master
$ git merge topic-E
$ git push origin master
```

## 2.3. masterブランチ以外のブランチへpush
```bash:
$ git checkout -b topic-F
$ touch index.php
$ git add .
$ git commit -m "Add topic-F"
$ git push origin topic-F
```

リモートのブランチができていることを確認
PullRequestは後で説明するので今はとりあえずマージしてOK

## 2.4. リポジトリの複製(clone)
- cloneしてみる
- 別の開発者の気持ちになる

```bash:
$ cd ..
$ mkdir another
$ cd another
$ git clone git@github.com:mohira/git_test.git
$ ls
```

できた！
- ブランチを確認
- masterしかない(topic-XXXはどこに？？？)

```bash:
$ git branch
$ git branch -a
```

リモートのブランチにチェックアウトする

```bash:
$ git ch -b add-branch origin/add-branch
```

- 変更してみる
- ローカルのブランチにコミット

```bash:
$ vi add-branch.txt
$ git diff
$ git commit -am "Add remote"
$ git push
```

これで、
/var/www/html/git_test
と
/var/www/html/another/git_test
に差分ができた

## 2.5. リモートリポジトリから差分を取得
- pull により最新のリモートリポジトリブランチを取得

```bash:
$ cd /var/www/html/git_test
$ git pull origin add-branch
```

## 3. GitHubの使い方
- ソースコードへのコメント機能
- コミットへのコメント
- ブランチの切り替え
- コミットを確認










