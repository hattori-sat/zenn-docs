---
title: "【備忘録】zennとGithubを連携してversion管理を楽にする"
emoji: "🐙"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["zenn", "Github", "cli", "所感", "やってみた"]
published: true
---
# 1. 概要
Zenn CLIを使用してGitHubリポジトリを編集し、Zennに技術記事をデプロイする方法があるようです。
実際に使ってみたところ、以下の良いポイントが存在
- ver.管理が楽に
- web上で記事を書くのが当然だったのが、VSCodeで完結
- 複数端末でも編集ができるように
- zennが消えてもgithubにデータは残る

# 2. 序論
## 2.1. 対象読者 / 適用範囲
- GitHubを使う機会がないが、使ってみたい方
- この記事は、ソフトウェア開発者やエンジニアを対象としています。
- 特に、GitHubでコードを管理し、Zennで技術記事を公開したいと考えている人
- 使用してみた書簡を記述します

## 2.2. 目的
- Zenn CLIを使用してGitHubとZennを連携させ、記事を効果的に管理する方法を簡単に挙げる
- この手法を用いるメリットを提示する

## 2.3. 仮定 / 環境
この記事を理解するためには、ZennアカウントとGitHubアカウントが必要
また、Node.jsとnpmがインストールされていることが前提条件です。

## 2.4. 略語 / 用語
Zenn CLI: Zennのコマンドラインインターフェース。Zennの記事をローカルで管理し、公開するためのツール。
GitHub: ソフトウェア開発プロジェクトのホスティングとバージョン管理プラットフォーム。
デプロイ：SWやアプリケーションなどを実際の運用環境に配置して、利用可能にすること

## 2.5. キーワード
Zenn CLI, GitHub, 技術記事, デプロイ

# 3. 課題
## 3.1. 現状
zennの記事は、zennのサイトで書く必要がある。

## 3.2. 理由付け
編集履歴などを残せず、markdownである旨みを活かし切れていない。
個人アカウントによる

## 3.3. 解決法
Zenn CLIを使用して、GitHubリポジトリを編集し、変更をZennにデプロイする。

# 4. 先行事例 / 具体例
GitHubは、チーム開発などで広く使われている。
例えば、Twitterなどでも使われている。
https://github.com/twitter/the-algorithm

# 5. GitHubとzennを連携

GitHubというコード管理のプラットフォームがあります。
今回の記事もこちらに書かれています。
https://github.com/hattori-sat/zenn-docs

このGitHubにMarkdown形式のファイルをアップロードすれば、自動的にzennにも表示されるようになります。
VSCodeの画面はこんな感じ。
![](https://storage.googleapis.com/zenn-user-upload/8a4b5e447311-20240227.png)

GitHubのメリットの説明になってしまうので、なるべく簡単に説明しますが、以下のメリットがあります。
- リポジトリ管理: プロジェクトごとにリポジトリを作成し、ソースコードやドキュメントを保存・管理します。
- バージョン管理: Gitを使用して、プロジェクトの変更履歴を記録し、複数の開発者が同時に作業できます。
- コラボレーション: チームメンバーとのコードの共有やコードレビュー、プロジェクトの管理を行います。
- イシュートラッキング: バグや機能のリクエストなどの問題を追跡し、解決を進めます。
- プルリクエスト: コードの変更を提案し、他の開発者にレビューしてもらう機能です。

これまnoteでさまざまな文書を作成してきましたが、この機能は目から鱗でした。

# 6. 結論
## 6.1. まとめ
Zenn CLIを使用することで、エンジニアはGitHubでコードを管理しながら、Zennに技術記事を簡単かつ効率的にデプロイすることができます。

## 6.2. 展望
本内容とは少し逸れるが、Markdownによる引き継ぎ資料や文書作成なども検討したくなる。

## 6.3. その他
Zenn CLIの詳細な使い方や設定に関する情報は、Zennの公式ドキュメントを参照してください。
https://zenn.dev/zenn/articles/install-zenn-cli
https://zenn.dev/zenn/articles/install-zenn-cli
https://zenn.dev/zenn/articles/zenn-cli-guide

# 7. 参考文献
https://zenn.dev/eguchi244_dev/articles/github-zenn-linkage-20230501
https://qiita.com/blue_fish/items/7440df68734f3d5ce772
https://nodejs.org/en/download
https://www.tohoho-web.com/ex/github.html
https://zenn.dev/hattori_sat/articles/github-zenn-template-20240227
https://qiita.com/G-awa/items/147dfc88513710d5da3c