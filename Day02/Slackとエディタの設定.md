# 環境構築その2
Slack
GitHubの登録
SublimeTextの設定

## GitHubアカウントの登録
GitHubのアカウント登録を行う
[ここから登録しましょう](https://github.com/)

## Slack
[todo]アカウント招待をGitHubでやる方法？
- チャットツール
- 講義内容についての質問はこちらからどうぞ

初期設定

- チーム名は`elites-camp-201602`
- チーム参加への招待メールを送ります。メールBOXを確認して下さい
- メールのリンクからチームへ参加する
- username:「 kashiwagi 」 のように名前で登録をお願いします。
- デフォルトアイコンが似ていて紛らわしいので手持ちの写真でアイコンを切り替える
- スマホアプリもあります
- チャネル別運用ポリシー
  - \#general：運営から皆さんへの通知
  - \#random：雑談

[サインインはこちらから](https://slack.com/signin)

- チーム名、メールアドレス、パスワードでログインする

## エディタ
- プログラミングをする上でエディタ(要はメモ帳)は必須
- 使いやすいエディタであるほどプログラミングも捗る！
- おすすめは SublimeText か Atom
- vim使いやemacs使いもいる

## 今回使用するエディタ
- Sublime Text 3
- [ダウンロード](http://www.sublimetext.com/3)


## フォントの導入
デフォルトでもいいけど、プログラミングにオススメなフォントを導入する

[todo]Rictyの設定方法とリンク挿入



## Package Control
- 便利なPackageをカンタンにインストールできる設定を行う
- 「 View > Show Console 」と進み、導入コードを貼り付ける
- 終わったらSublimeTextを再起動する

```txt:PackageControl
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

```

SublimeText2の場合は下記

```txt:SublimeText2の場合
import urllib2,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')
```


## Packageのインストール
「 command+shift+P 」を同時に押した後、「 install 」と入力する
「 Package Control: Install Package 」と表示されるのでこれを選択する
パッケージはいろいろなサイトで解説されているので気になれば調べるとよい


## おすすめパッケージ

### 必須系
- AutofileName
パスも含めたファイル名の入力補助

- Bracket Highlighter
カッコの対応関係が見やすくなる

- TrainingSpace
不要なスペースの削除

- All Auto Complete
入力補完の強化

- emmet
爆速HTMLコーディング
→ ~/Library/Application Support/Sublime Text 3/Packages/Default をつくって、Defalutのキーバインド編集
→ Emmetの設定ファイルを開くには
メニュー>Preferences>Package Settings>Emmet>Settings - User

convertUTF8
- 文字コードをUTFに統一してくれる

### 入力補助
- HTML5
- AndyPHP
- CSS Snippets
- jQuery

### あると便利かも
- SideBarEnhancements
サイドバーの強化

- boundkeys
キーバインド(ショートカット)の確認

- Abacus → com+shift+p → Abacusでいいかも
コードの整形

- color picker
カラープロパティの補助

- Browser Refresh
エディタからブラウザの更新ができる

## Sublimeの設定
おすすめ設定

```json:Preferce-Setting-User
{
    "auto_indent": true,
    "bold_folder_labels": true,
    "default_encoding": "UTF-8",
    "disable_formatted_linebreak": true,
    "disable_tab_abbreviations": true,
    "draw_white_space": "all",
    "enable_tab_scrolling": false,
    "font_face": "Ricty Diminished Regular",
    "font_size": 14,
    "highlight_line": true,
    "ignored_packages":
    [
        "Vintage"
    ],
    "match_tags": true,
    "tab_size": 4,
    "translate_tabs_to_spaces": true,
    "trim_trailing_white_space_on_save": true,
    "word_wrap": true
}
```

```json:Preferce-KeyBindings-User
[
    // Browse Refreshの設定
    {
      "keys": ["command+shift+r"], "command": "browser_refresh", "args": {
        "auto_save": true,
        "delay": 0.0,
        "activate": false,
        "browsers" : ["chrome"]
      }
    },

// カーソル移動系
{
    // ctrl+d でデリート
        "keys": [
            "ctrl+d"
        ],
        "command": "right_delete"
},
{
    // ctrl+e で行末へジャンプ
    "keys": [
        "ctrl+e"
        ],
        "command": "move_to",
        "args": {
            "to": "hardeol"
        }
},

{
    // ctrl+e で展開しないようにする
    "disabled_keymap_actions": "expand_abbreviation"
},
{
    // ctrl+i で展開可能にする
        "keys": [
            "ctrl+i"
        ],
        "args": {
            "action": "expand_abbreviation"
        },
        "command": "run_emmet_action",
        "context": [
            {
                "key": "emmet_action_enabled.expand_abbreviation"
            }
        ]
},


{
    // ctrl+s で サイドバーを呼び出す
     "keys": ["ctrl+s"],
     "command": "toggle_side_bar",
},

// インデントショートカット
{ "keys": ["ctrl+shift+i"], "command": "reindent" , "args": { "single_line": false } },

]

```











