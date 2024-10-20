---
title: "Conflueceをmarkdownで書く方法とそのメリットデメリット"
emoji: 🧑‍💻
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["confluence", "github", "wiki", "markdown"]
published: true
---

# 1. 概要
Wikiアプリとして、「*Confluence*」や「*Notion*」が現在多く使われいるように思います。  
メモや日記、議事録、情報管理など様々な用途で使われているでしょう。

Confluenceで文書を書く際に以下のような**小さな苦痛**を感じている人がいるのではないでしょうか。
- *markdown*とは若干違うため、書きにくい（勝手に変換されないで...）
- *VScode*で編集させてほしい
- バージョン管理が何気にしにくい

本文書ではこれらの**小さな苦痛**を解決する方法を記載します。その解決法は以下の通りです。
1. *VSCode*で*markdown*形式のwiki(文書)を書く
2. これらの文書を*GitHub*で管理する
3. pushされたタイミングで*Actions*を用いて*Confluence*に反映する（deployする）

これによって、課題をすべて解決できました。

本文書では、以下のことを記述したいと思います。
- 上述した解決法を行うフロー
- 解決法によって出来ること出来ないこと

# 2. markdownとは
本記事も*markdown*で書いています。
*markdown*とは、文書をより効率的に書くことが出来る記法です。

具体例を示しておきます。下記のFig. 2-1の左側の部分がmarkdownで書いたもので、右側が記事で表示されるものです。
![](/images/sample-markdown.png)
*Fig. 2-1. markdownの具体例*

Fig. 2-1を見て分かる通り、markdown形式では文字のみで書くことが出来るのに、斜体や太字、そして画像の挿入なども出来ます。

記法は覚える必要がありますが、どの環境でも書くことは出来ます。
拡張子として、**.md**を用いてればmarkdown形式のファイルとなります。

VSCodeのpluginを使えばFig. 2-1のようにpreviewも見ることが出来るため、簡単に文書を作成することが出来ます。
VSCodeには他にも様々な便利な拡張機能（例えば、TODO Tree）があるため、VSCodeで文書を書きたくなるわけです。

:::message
Confluenceのようなwikiで文書を集中管理することによるメリットは大きいが、そのwikiを書く方法に困っているため、そのための解決法を次章では説明していきます。
:::


# 3. 解決法のフロー
2章では、*markdown*のメリットを簡単に紹介しました。
ここからは、このmarkdownを*Github*で管理してConfluenceに反映する方法を示していきます。

## 3.1. GitHubでの設定や準備
本文書では、ある程度の*Git*や*GitHub*の知識があるものとして説明します。しかし、調べれば簡単に理解できるものしか使っていないと思います。(*GitHub*アカウントを持っていない場合は参考文書[1]などを参考にしてください。)

## 3.2. 適当な名前のプライベートリポジトリを作成
基本的にはConfluenceはプライベートに使うもの（外部に公開しない）ものだと思いますので、リポジトリも**private**にしておきましょう。そうしなければ、個人的な情報が公開されてしまうことになります。
リポジトリ名は任意の名前で問題ないです。

## 3.3. markdownを格納するフォルダを作成
リポジトリの中でmarkdownファイルを書く右脳するための適当なフォルダを作成しましょう。フォルダ名は任意の名前で問題ないです。私は`docs-markdown`としておきました。

## 3.4. Github Actionsのファイル作成
私のリポジトリ内のフォルダ構成を以下に示します。`.github`フォルダの下に`workflows`フォルダを作成します。
```bash
├─.github
│  └─workflows
├─assets
└─docs-markdown
```

![](/images/github-setting.png)
*Fig. 3-1 Githubの設定*

その後、`workflows`直下に、任意の名前のyamlファイルを作成しましょう。私は今回`deploy-markdown-confluence.yml`としました。

## 3.5. workflowの作成
3.4.節で作成したファイルの中に以下のコードを記述します。基本的なコードは、参考文書[2]のコードを参考にしました。
```yaml
name: "Confluecne Publisher"
on: 
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  workflow_dispatch:  
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: Deploy Markdown to Confluence
        uses: markdown-confluence/publish-action@main
        with:
          confluenceBaseUrl: ${{secrets.CONFLUENCE_BASE_URL}}
          confluenceParentId: ${{secrets.CONFLUENCE_PARENT_ID}}
          atlassianUserName: ${{secrets.CONFLUENCE_MAIL_ADDRESS}}
          atlassianApiToken: ${{secrets.CONFLUENCE_TOKEN}}
          contentRoot: docs-markdown
          folderToPublish: .
```
さて、このワークフローを実行すれば、markdownをpush（など）した際にconfluenceに項開催されるようになります。

現時点ではこのファイル内部のいくつかの点の内容が分からないと思いますので、それらについて説明していきたいと思います。

## 3.6. workflow内の設定
3.4. 節のコードは簡単に言えば以下のことをしているだけです。
1. `on`で指定されている`main`への`push`もしくは`pr`、手動をトリガーとして実行
2. `main`にcheckout
3. 公開されている[`publish-actin`](https://github.com/markdown-confluence/publish-action)を実行。その際に`with`以下の設定を用いる

よって、ここから設定するのは、`with`以下の変数です。
プライベートな内容なので、secretsに書いています。
GitHubのwebサイトから以下の場所で設定できます。
![secretの設定の写真](/images/secret-repository.png)

さて、各設定の内容は以下の通りとなります。
|Items|Contents|Notes|
|:--:|:--:|:--:|
|confluenceBaseUrl|`https://`から最初の次の`/`の前まで|自分のConfluenceのページ。baseURLの最後のスラッシュは不要|
|confluenceParentId|confluenceでmarkdownで書いた文書を反映したい親ページのID（整数値）|好きな親ページの右上の・・・ボタンから詳細情報→ページ情報と移動した時に、URLの最後尾に記載のid|
|atlassianUserName|Confluenceを作成した時のメアド|他でも行けるかも...?|
|atlassianApiToken|API token（絶対に非公開）|ページから右上アカウントのアイコンをタップ→アカウントを管理→セキュリティ→APIトークン|
|contentRoot|markdownを格納するために作ったフォルダ|`/`はいらない|
|folderToPublish|contentRootの中で公開したいフォルダがあれば指定|今回はそもそもcontentRoot内にフォルダを作成していないため、フォルダ全部|

## 3.7. 適当なフォルダを作成してpushする
3.3.節で作成した`docs-markdown`フォルダ（好きな名前で作成したフォルダ）の下に任意の名前のmarkdwonファイルを作成します。
私は今回、`trial.md`というファイルを作成し、そのファイルにhello worldと書きました。そして、`push`します。
:::details pushする方法の一例
.git があるフォルダ内で以下を実行
```bash
git add .
git commit -m "first trial"
git push origin main
```
:::

これでworkflowが実行されるはずです。
GitHubのwebサイトに行き、Actionsというタブをクリックすると以下のようにworkflowが実行されています。この状態になれば成功です。
![](/images/actions.png)

成功していることを確認したら、Confluenceの指定した親ページを開くとその子ページとして、`***.md`としたときの`***`という名前のページが作成され、その内部にhello worldと書かれていると思います。

これで*VSCode*で作成した*markdown*の文書が*Confluence*に反映されました。

# 4. 出来ること出来ないこと
さて、ここまでで**小さな苦痛**は解決されたかのように見えました。
しかし、出来ることが増えたことで新しい課題も出てきます。
- Confluenceのマクロを使えるない（確認できていない）
- Confluence側の変更はGitHbに反映されない
この辺りはpushのたびにmarkdownの内容がConfluenceに追加されてしまうのである注意が必要です。（言い換えると、Confluenceで修正を加えても、次のGitHubの変更によって修正がなかったことにされてしまう）

出来ないことはまだありますが、出来ることになったことを見ていきましょう。
以下のようなmarkdownをpushしてみます。（適当にファイルを用意しているので、もし同じコードを実行したい場合はassetsフォルダを作成して、sample.drawio.pngを作成してください。）

````
- [概要](#概要)
  - [見出し2](#見出し2)
    - [見出し3](#見出し3)
----
# 概要
こちらの見出しは見えていますか？

## 見出し2
これも見えます？

### 見出し3
この見出しはみえる？
----
リストは？
- abc
- def

数字リストは？
1. hey
2. hello

引用は？
> 吾輩は猫である

コードは？

```python
print("hello world")
```
リンクは？
https://zenn.dev/hattori_sat/articles/github-zenn-template-20240227

ショートカットリンクは？
[この記事は上と同じ](https://zenn.dev/hattori_sat/articles/github-zenn-template-20240227)

~~打消し線~~

**太字**

テーブル
header1|header2|header3|
|:--|--:|:--:|
|align left|align right|align center|
|a|b|c|
----
画像  
![draw.ioのやつ](../assets/sample.drawio.png)

> [!NOTE]
> note
>

> [!TIP]
> TIPS

>[!IMPORTANT]
> important

mermaid
```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```

/page tree
一旦これで満足かも
````

この結果、Confluenceでは以下のように見える。
![](/images/confluence1.png)
![](/images/confluence2.png)

出来ることを纏めておく。
- markdownの見出しはちゃんと反映される
- 見出し1,2,3
- 箇条書きのリスト
- 数値のリスト
- 引用
- コードブロック
- リンク
- 埋め込みリンク
- 打消し線・太字
- テーブル
- 画像（vscodeで保存した画像）
- NOTEなどのgithubで使われる文字装飾
- mermaid形式の埋め込み（ただし画像として保存）

他にも出来ることはあると思われるが個人的な利用の範囲ならこの辺りで充分だと思われます。
Confluence独自のマクロを使う手段を見つける必要はありますが、好きなConfluenceの親ページを指定してその直下のConfluenceのみを操作できるため、活用方法は多いと思われます。

Confluece以外にもAtlassianのツールでJiraというものがあります。
これによって、チケット管理をしながら開発を行えます。

今回、Conflueceの文書をGitHubで管理できるようになったことで、Jiraによってブランチを切り文書をmarkdownで作成し、それによってConflueceの文書を作成するというなフローも出来るようになりました。

# 5. おわりに
markdownを使えるようになってからは、markdown形式で文章を書きたくなってしまうようになりました。zennはmarkdownで記述できるうえに、自動deployしてくれるのでとても有難いです。
最近（でもないが）、VSCode上で`draw.io`が使えるようになった。毎回expoertしなくても、そもそも`.drawio.png`などの形式で保存できるため、これを画像として使えるようになりました。（これによってUMLもVSCode作れるようになりました）
このようにVSCodeで文書を作成することでより効率的に開発などを行えるようになってきているように思います。

本文書によって少しでも楽な開発・執筆環境になれば幸いです。
読んでいただきありがとうございます。

ご質問・ご指摘・アドバイス等ございましたら、本記事へのコメントや任意のオウンドメディアへお問い合わせください。

# 6. 関連文書
## 6.1. 参考文書
1. GitHub Docs、[GitHubでのアカウント作成](https://docs.github.com/ja/get-started/start-your-journey/creating-an-account-on-github)→GitHubのアカウントを持っていない場合は作成方法を参考にしてください
2. bakachou、[GitHub においた md (マークダウン) のドキュメントを GitHub Actions で Conluence のページに連携](https://qiita.com/bakachou/items/72536f4a397306371dbf)→コードはこの方の記事を参考にしました
3. [Markdown Confluence Tools](https://github.com/markdown-confluence)→今回使用しているカスタムアクション
