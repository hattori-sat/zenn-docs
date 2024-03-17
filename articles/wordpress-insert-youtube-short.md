---
title: "wordpressでyoutubeのshort動画を挿入する方法"
emoji: "📚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["wordpress"]
published: true
---
# 背景
研究室のHPを運営している者です．
HPはwordpressで編集していました．

shorts動画を載せようと思ったところ少し詰まったので共有します．
（YouTube: 正しい URL を指定してください．と出ました．）

# YouTube Shortが再生画面にならない問題の解決
結論を先に言うと，再生リストに登録して，再生リストのURLを貼ればshort動画もHP内で再生できるようになりました．
以下のような感じです．
https://youtu.be/k3LvpkiTJ2U?list=PLX6GOaeQ5cf5s3--k75NHod_wLN8YqFdB

----

wordpressの以下の画面からショートコードを選ぶと通常のYouTube動画は挿入できます．
![](https://storage.googleapis.com/zenn-user-upload/c1e1089b5003-20240317.png)
同様にshorts動画もやってみましょう．
![](https://storage.googleapis.com/zenn-user-upload/ee752d510420-20240317.png)

共有可能なリンクを取得します．  
その結果，以下のようなリンクが発行されました．

https://youtube.com/shorts/k3LvpkiTJ2U?feature=share:embed:cite

なので，この動画をwordpressに貼り付けてみましょう．

![](https://storage.googleapis.com/zenn-user-upload/e9953b39f89d-20240317.png)

![](https://storage.googleapis.com/zenn-user-upload/a5f3ceed40d4-20240317.png)

この状態でプレビューをしてみます．

![](https://storage.googleapis.com/zenn-user-upload/3ff71283da76-20240317.png)

するとエラーが出てきました．

つまり，共有リンクではwordpressは対応していないようです．

そこで，埋め込み用のリンクを発行してみました．  
下図のように動画再生ページを右クリックすると選ぶことが出来ます．
![](https://storage.googleapis.com/zenn-user-upload/7b6faf0173ac-20240317.png)

すると


[https://www.youtube.com/embed/k3LvpkiTJ2U]


というリンクを得られます．

なので，このリンクを貼ってみましょう．
![](https://storage.googleapis.com/zenn-user-upload/b318c68d6b5b-20240317.png)
![](https://storage.googleapis.com/zenn-user-upload/7af20f2c3d0b-20240317.png)

このように上手くshorts動画を貼ることが出来ました．

https://youtu.be/k3LvpkiTJ2U?list=PLX6GOaeQ5cf5s3--k75NHod_wLN8YqFdB

参考になれば幸いです．

# 補足
本ブログは，はてなブログから移植したものになっています．

