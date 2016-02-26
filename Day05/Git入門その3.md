# Git入門その3

## .gitignore
- いままでローカルリポジトリ以下にあるファイルは全てgitの管理下だったけど、管理対象から外すこともできる
- 管理対象から外すのは、logファイルとかね

```bash:
$ mkdir log
$ touch log/error.log
$ touch sales.csv
$ git status
$ vi .gitignore
$ git status
$ git add .
$ git commit -m "Add gitignore"
```

```txt:.gitignore
# コメント
log/

*.csv
````

## エイリアス
- とりあえずここまでで、ローカルリポジトリのgitはOK
- エイリアスを使って、良く使うコマンドを簡単に入力できるように使用


add -> ad
commit -> co
branch -> br
checkout -> ch
status -> st


```bash:
$ git config --global alias.ad add
$ git config --global alias.co commit
$ git config --global alias.br branch
$ git config --global alias.ch checkout
$ git config --global alias.st status
$ git config --list
```

## rebase

```bash:
$ git checkout -b topic-D
$ vi README.md
```

```md:README.md
# git_test

line3

line5

line7

line9

topic-A

topic-B

topic-C

toipc-d
```

topic が toipc になっているのに気づかずコミット

```bash:
$ git commit -am "Add topic-D"
```

で、ここでタイポに気づく
直して、コミットしてみる

```bash:
$ vi README.md
$ git commit -am "Fix Typo"
```

まあ、これでもいいんだけど、これくらいの修正ならコミットを増やしたくないよね。
そこで rebase
要は、fix typo と Add topic-D を一つのコミットにまとめてしまうということ

```bash:
$ git rebase -i HEAD~2
```

HEAD(最新のコミット)を含めた2つまでの歴史を対象としてエディタが立ち上がる

```txt:
pick 134bce8 topic-D追加
pick b0f4efe Fix typo

# Rebase a17f258..b0f4efe onto a17f258
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted.
#
````

下記のように修正

```txt:
pick 134bce8 topic-D追加
fixup b0f4efe Fix typo

~ 以下、省略 ~
````

```bash:
[detached HEAD 49d472e] topic-D追加
 1 files changed, 2 insertions(+), 0 deletions(-)
Successfully rebased and updated refs/heads/topic-D.
$ git log
```

めでたし。
じゃあmasterにmergeする

```bash:
$ git checkout master
$ git merge topic-D
$ git log
```
