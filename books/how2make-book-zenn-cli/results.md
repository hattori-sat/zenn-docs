---
title: "zenn cli で本を作るとは"
free: true
---
# そもそもCLIとは
CLIというのは，Command Line Interface の略です．要はGUIでクリックやら何やらするのをCUIで完結しよう的なことです．
では，なぜ使っているのかというと，Githubと連携できるからです．以下のリンクが私のGithubの中でzennの記事を管理しているところです．

https://github.com/hattori-sat/zenn-docs

私はnoteやはてなブログをやってきましたが，web上で記事を変更してアップロードしてきました．それでもよかったのですが，変更履歴を参照したり，書きたいことをメモしておくことはなかなかできませんでした．zennでは，Githubの機能を使えることでより楽に記事を書く（というより開発しているイメージ）ことが出来ます．

ちなみにこの本を書いている画面はこんな感じ．

![](https://storage.googleapis.com/zenn-user-upload/01d57455c08d-20240318.png)

![](https://storage.googleapis.com/zenn-user-upload/3cc911546894-20240318.png)

図の挿入だけ少し面倒ですが，GUIから入れている（ほかの方法もあるが，今はこの方法を採ってる）

![](https://storage.googleapis.com/zenn-user-upload/1e02e9963b6f-20240318.png)

----

# 有料本の作り方
さて，本題に入りますが，有料本を作る方法は難しくありません．以下の記事を参照すれば作ることが出来ます．

https://zenn.dev/zenn/articles/zenn-cli-guide

適当にフォルダを作って，config.yamlファイルを作ります．そして，yamlファイルの中を以下のようにします．

```
title: "zenn cliで本を作る方法"
summary: "省略"
topics: ["markdown", "zenn", "react"] # トピック（5つまで）
published: true # falseだと下書き
price: 200 # 有料の場合200〜5000
chapters:
  - introduction # チャプター1
  - results # チャプター2
  - conclusion # チャプター3
```

これで後は，Chaptersとなる introduction.md，results.md，conclusion.md を作れば本を作れます．

しかし，このままの設定では問題があります．

----

# 問題
この本は，一応有料本であるため，zenn上では無料部分以外は見れなくなっている．すべて有料設定していると次のとおりである．

![](https://storage.googleapis.com/zenn-user-upload/b578087bf5d5-20240318.png)

しかし，github上では以下のとおりである．

![](https://storage.googleapis.com/zenn-user-upload/bb120f20d27a-20240318.png)

すべて見えてしまっている（異なるアカウントから見ているため，誰でも見れてしまう）

つまり，ここでの問題は，**zennの有料本がGithub上で読めてしまう**のである．

----

# 解決法
色々考えたが，1つしか方法がないように思われる．

有料本の内容は，**private repository** を新しく連携してそちらで本を書く．

zennとGithubの連携は，2つのリポジトリでもよいため，この方法が良いと思う．

----

# おわりに
これでこの本の内容は終わることにする．
大した内容がなくてすみません．

この本の内容は，

- Github連携することで本はとても書きやすい！
- 有料本を書くなら，privateのGithubでやること（Githubと連携している場合）


最初は頭が狂って，gitignoreに書けば良くね？と思っていましたが，それではデプロイできませんね．

もう一つの章は適当に文章を書いておきたいと思います．
なお，最後の章だけ ファイルの頭（ Front Matter ）に  free: true を入れていません．なので，本を買わないと見れなくなっていると思います．

ディレクトリ構造について疑問になったら，この本の構造を私のGithubから確認してみてください．
https://github.com/hattori-sat/zenn-docs/tree/main/books/how2make-book-zenn-cli

お疲れ様でした．もしお役に立てた場合は，本記事を購入してくださると幸いです．
なお，conclusionの章はGithubから見れますので，その章を見るためにこの本を買わないように注意してください．

以上

----