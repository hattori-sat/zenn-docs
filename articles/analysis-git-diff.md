---
title: "「git diff」のソースを読んで理解を深める"
emoji: 🧑‍💻
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["git", "diff", "c-lang","github"]
published: true
---

# 1. 概要
**git diff** は開発をしている使用する機会が多いgitのコマンドです。

出来ることは、色んなものの差分を取ることです。例えば、

- 2つのコミット間の変更点比較
- 特定のブランチやタグとの比較
- 変更された行数やファイル数を統計的に表示（--statオプション）。

などがあります。開発中に使える便利ツールです。

しかし、**diffはどうやって差分をとっている**のでしょうか。また、**なぜ中身が一致しないファイルを探し出して比較できる**のでしょうか。

この辺りの詳細について知るためには、**diffの中身**(ソースコード)をみて学ぶのがよいと思ったため、本文書では簡単に解析した結果を示します。

----

# 2. git diff の解析をするために勘所を探る
以下がgitのソースコード(と思われる)。
https://github.com/git/git/tree/master

C言語とshell scriptなどで構成されています。  
gitのリポジトリには大量のファイルがあります。しかし、コマンド名などで区切られていそうな雰囲気を感じます。

gitコマンドに大きくかかわりそうなものとして、*builtin* ディレクトリというものがあります。
**buildin**というののは、**組み込みコマンド** などを意味することがあるため、この辺りを追っていけば良さそうに思います。

この *buitin* ディレクトリの中に `./builtin/diff.c` があります。  
その中に、`cmd_diff` という関数があり、おそらくこれが git diff コマンド本体ではないかと思われます。

この `cmd_diff` 関数の前方宣言は、`./builtin.h` にあります。
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/builtin.h#L154

また、`./Makefile` にもコンパイルされた `.o` ファイルがあり、リンクされています。
どのようにgitのバイナリがビルドされるのかを知るためにMakefileを2.1.節で眺めてみます。

## 2.1. Makefileを眺めてみる
**gitのバイナリ**をビルドしているのはたぶんここ。
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/Makefile#L2489-L2491

gccで`./builtin/`以下の`*.c` をコンパイルして `*.o` を生成している。
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/Makefile#L2793-L2794

`QUIET_CC`は以下の通りです。`CC=gcc` であり、別途記載されています。
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/shared.mak#L58-L59

コンパイルされたオブジェクトファイルは,`BUILTIN_OBJS`に格納されます。
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/Makefile#L1235-L1239

`BUILIN_OBJS`は`GIT_OBJS`に格納されます。
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/Makefile#L2744

最終的に、`GIT_OBJS`は`OBJECTS`に格納されます。そしてこれは**上記のgccビルド時に指定**されています。
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/Makefile#L2744

:::message
よって、`./builtin`以下のcソースを見ていけば、**git diff**が何をしているのか理解できそう
:::

## 2.2. `./builtin/diff.c` を眺める
おそらく、`git diff` を実行すると`cmd_diff`関数がコールされると思われます。
https://github.com/git/git/blob/master/builtin/diff.c#L396-L633

この辺りもincludeしていることを確認できます。
https://github.com/git/git/blob/master/builtin/diff.c#L15-L17

例えば、`./diff.h` の実装は、`./diff.c` に書かれているので一例を見てみると、
https://github.com/git/git/blob/master/diff.c#L6410-L6424

コンポーネントとなる関数が実装されている感じに思われます。

----

# 3. `git diff` を解析
`./builtin/diff.c`の`cmd_diff` の流れは以下のような感じ。
1. 引数の解析（例：--cachedやファイルパス）
2. リポジトリ設定の初期化
3. 適切な diff 処理（ファイル、ツリー、インデックス間の比較）
4. 最終的な diff 結果の計算と後処理

この辺りで差分をとる対象について設定している。
https://github.com/git/git/blob/master/builtin/diff.c#L553-L584

## 3.1. tree vs tree
ゆくゆく他も解析することとして、一旦私が知りたい**tree vs tree**のdiffについて調べます。
(N (tree-ish): ツリー、M (blobs): Gitにおけるファイルの内容、P (pathspecs):パス指定)
https://github.com/git/git/blob/master/builtin/diff.c#L434-L435

`./builtin/diff.c`の`cmd_diff`の該当部は以下であると思われます。
https://github.com/git/git/blob/master/builtin/diff.c#L615-L620

ここでコールされている`builtin_diff_tree` はこんな感じ。
https://github.com/git/git/blob/master/builtin/diff.c#L170-L206

コールされている`diff_tree_oid`はこれ。
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/tree-diff.c#L726-L740

ここでコールされている`ll_diff_tree_oid`はこれ。
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/tree-diff.c#L706-L724

ここの`diff_tree_paths` からが目的の部分っぽい。
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/tree-diff.c#L581-L595

これが該当部分と思われます。
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/tree-diff.c#L430-L579

1. 準備段階
   - まず、ツリーオブジェクトと親ツリー（`parents_oid`）をロードし、それぞれを`tree_desc`構造体に格納します。
   - `fill_tree_descriptor` 関数でツリー情報を取得し、ツリーの構造体を初期化します。
2. 再帰の有効化
    - `opt->pathspec.recursive` を設定して、ツリーの深さに関わらず再帰的に探索を続けるようにします。
    - どんどん深くに進んでいく
1. 差分の取得と処理
   - ツリー内の差分を比較し、`tree_entry_pathcmp` 関数を使って、ツリー内のエントリをパス名とモードで比較します。
   - 最も小さなエントリ（`imin`）を特定し、そのエントリと現在のツリー（`t`）を比較します。
   - 同一であれば、変更がない部分（`t = p[imin]`）をスキップしますが、異なる場合（`t < p[imin]` または `t > p[imin]`）は、それぞれ追加または削除を `emit_path` 関数で処理します。
1. ツリーの更新
   - 比較後は、`update_tree_entry` や `update_tp_entries` を使って、ツリーエントリの状態を更新します。
2. 終了条件
   - すべてのツリーが処理された場合、ループを終了します。差分が満たされた場合や最大変更数を超えた場合にも終了します。
3. メモリの解放
   - 最後に、使用したメモリを解放します。

探索の順番は、明示的に設定した値まで深く探索する深さ優先探索のように思える。

ちなみにhashで比較している。
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/tree-diff.c#L526
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/hash.h#L366-L369

### 3.3.1. 子ノードを選ぶ順番
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/tree-diff.c#L486-L505

ここで、`imin` は、`tp[i]` と `tp[imin]` の間でパス名を比較し、最も小さい（辞書順で最初に来る）エントリを選んでいます。この `imin` が、次に進むべき子ノードを決める際の基準となります。

- `cmp < 0` であれば、`tp[i]` のエントリが `tp[imin]` よりも辞書順で前に来るため、`imin` を更新します。
- `cmp == 0` の場合、同じパス名であれば、`tp[i]` と `tp[imin]` は等しいとみなします。
- `cmp > 0` の場合は、`tp[i]` のエントリが `tp[imin]` よりも後に来るため、`tp[i]` は「`imin` とは異なる」とマークされます（`tp[i].entry.mode |= S_IFXMIN_NEQ`）。

このように、`imin` は `tp` 配列の中で最も辞書順で前に来るパス を選び、その後の処理で `imin` に基づいて探索が進みます。


### 3.1.1. 子ノードへの移動
https://github.com/git/git/blob/6ea2d9d271a56afa0e77cd45796ea0592aa9c2d4/tree-diff.c#L513-L569
ここでは、現在のツリーのエントリ t と、選ばれた最小の親ツリーのエントリ tp[imin] とを比較しています。比較の結果に応じて、次にどのように処理を進めるかが決まります。

- `cmp == 0`: `t` と `tp[imin]` が等しい場合、次に進むべき処理（差分を出力するなど）が行われます。
- `cmp < 0`: t が `tp[imin]` よりも小さい場合、`t` を探索した後、次に進む処理が行われます。
- `cmp > 0`: `t` が `tp[imin]` よりも大きい場合、`tp[imin]` に関連する処理が行われます。

もう少し探索方法についてまとめたい(UML書いておきたい)

----

次はこれかな？