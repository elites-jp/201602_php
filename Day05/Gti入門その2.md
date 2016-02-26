# Git入門その2

## 1. ブランチ
並行機能開発には欠かせない機能
Git習いたての時にはあんまり使わない

## 1.1. ブランチとは
- 超わかりやすい→[こわくないgit](http://www.slideshare.net/kotas/git-15276118?related=1)
- ブランチ(branch: 枝/支流)を使うと、別々の作業を平行して行える
- 最新のコードが複数あるようなイメージ
- あるブランチでの開発が終了したら、メインのブランチに合体(merge)させる
- ブランチはあくまでコミットの別名
- ブランチでそれぞれコミットして初めて枝分かれする
- 基本的に1つの機能を作るときには、1つのブランチ(トピックブランチ)を用意して作業していくことになる。
- 機能が完成した時に、masterブランチに合体(merge)させる。
まあ、よくわからないと思うので実際にやってみましょう

いろいろ言っても良う分からんので、使ってみましょう


## 1.2. ブランチの確認
ブランチを確認してみる
実はデフォルトで`master`ブランチが存在

```bash:
$ git branch
* master
```

## 1.3. ブランチをつくる/ブランチの切り替え

```bash:
$ git branch topic-A
$ git checkout topic-A
M   README.md
M   index.php
Switched to a new branch 'branch-a'
$ git brach
  master
* topic-A
```

この時点では特に何も変化がない

では、この topic-A というブランチでファイルを更新します。

```bash:
$ vi README.md
```

```md:README.md
# git_test

line3

line5

line7

line9

topic-A
```


```bash:
$ git status
$ git add .
$ git commit -m "Add topic-A"
```

別にいつもどおりだが、ここでブランチを切り替えると。。。。

```bash:
$ git checkout master
$ vi README.md
$ git log
```

topic-Aで行ったコミットの影響を受けていない！
→同時に複数の機能を開発できる

## 1.4. 別のブランチの変更内容を取り込む(マージ)
- 先ほど行ったtopic-Aでの編集をmasterに取り込むにはmergeする
- masterブランチはいつでも人に見せられるような完成状態であることが望ましい
- --no-ffをつけるとマージしたことが明確になる

```bash:
$ git branch
$ git merge --no-ff topic-A
Updating 8c8f9b6..6aedb4f
Fast-forward
 README.md |    3 +++
 index.php |    2 +-
 2 files changed, 4 insertions(+), 1 deletions(-)
$ git log --graph
```


## 1.5. 練習問題

```txt:練習問題
topic-Bブランチを作成し、同ブランチにおいてREADME.mdを下記の用に編集してください。
編集完了後、コミットして、masterブランチにmergeしてください
```

```md:README.md
# git_test

line3

line5

line7

line9

topic-A

topic-B
```

```bash:解答例
$ git checkout -b topic-B
$ vi README.md
$ git commit -am "Add topic-B"
$ git checkout master
$ git merge --no-ff topic-B
$ git log --graph
```


## 2. ブランチでのコミット操作
ここまでの内容ができていれば十分合格レベル
ここからは、「やっちまった！」ときに対処法について学びます

## 2.1. 過去に戻る

```bash;
$ git log
$ git reset --hard 47cf5a4a57b1d01b24139d61d7f9415b260017c1
HEAD is now at 47cf5a4 Merge branch 'topic-A'
$ git log
```

```bash:
$ git checkout -b topic-C
$ vi README.md
$ git commit -am "Add topic-C"
```

```md:README.md
# git_test

line3

line5

line7

line9

topic-A

topic-C
```

## 2.2. 未来に戻る(？)
- 最終的には、topic-A,topic-B,topic-C の全ての変更が取り込まれたかたちを目指す
- で、topic-Bをマージする前の状態に戻ってしまったので、復元する
- git reflog で現在のリポジトリで行われた作業ログをとれる

```bash:
$ git checkout master
$ git reflog
$ git reset --hard 339a0eb
HEAD is now at 339a0eb Merge branch 'topic-B'
$ git log
$ vi README.md
```

見事に戻っている


## 3. コンフリクト
topic-A topic-B topic-C の更新ができたのでマージしましょう。

```bash:
$ git branch ### masterブランチにいることを確認
$ git merge topic-C
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```


編集内容に矛盾が起きたのでコンフリクト
コンフリクト(confilict: 競合 葛藤)
README.mdをチェック

```md:README.md
# git_test

line3

line5

line7

line9
<<<<<<< HEAD
topic-A

topic-B
=======

topic-C
>>>>>>> topic-C

````


```txt:現在のHEADの内容
<<<<<<<< HEAD
topic-A

topic-B
===========
````

```txt:topic-Cブランチの内容
=======

topic-C
>>>>>>> topic-C
===========

````


最終的なかたちに修正すればOK

```md:README.md
# git_test

line3

line5

line7

line9

topic-A

topic-B

topic-C
```

この状態で改めてコミットすればOk

```bash:
$ git status
$ git add .
$ git status
$ git commit -m "コンフリクト解消"
```

## 4. コマンド

### `git branch`
### `git branch <branch_name>`
### `git checkout <branch_name>`
### `git checkout -b <branch_name>`
### `git merge <branch_name>`
### `git merge --no-ff <branch_name>`
### `git log --graph`
### ``
### ``
### ``
### ``
### ``
### ``
### ``










