---
title: "【コード全公開】物理を直感的に学ぶための物理シミュレータ"
emoji: "👨‍🏫"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["physics", "高校物理","大学物理", "processing", "python"]
published: true
---
# 1. 概要
本文書では、自作の**物理シミュレータ**を用いて**運動方程式のシミュレーション**を行う方法を解説します（以下のYouTubeの動画のようなシミュレーションが出来るようになります）。
コードは全公開しているため、自由に改変していただきご自身の活動に役立ててほしいと思っております。
https://www.youtube.com/watch?v=o2QZctsSqnk

私自身、高校で物理を学び、大学・大学院では人工衛星のシミュレーションに取り組んできました。振り返ると、もう8年以上物理に触れてきており、見慣れた運動方程式であれば挙動を頭の中で予測できるようになってきました。

そんな経験の中、以下のような**問題**があるように感じてきました。
- 運動方程式を見ても物体がどんな動きをしているのか頭の中で分からない
- 教科書には運動方程式を解いた結果として**グラフ**しか示されていない
- グラフィカルに物理のシミュレーションを行った動画は少ないし、個人利用してよいか迷う
- 物理を教えるための**グラフィカルに物理シミュレーション**をする方法がない
- 物理エンジンにお金を出せない（または物理シミュレータを委託するほど学校にお金がない）

:::message
本文書を読むことで以下のことが出来るようになります。
- 物理シミュレーションをコードを見ながら学べます
- 物理の運動方程式を2Dや3D表示で**グラフィカルに表示**する手段を得ます
:::

----

# 2. はじめに
## 2.1. 対象読者
本文書の対象読者としては、以下を想定ししております。
- 物理を教える際に物体の動きをグラフィカルに見せたい**高校教師**
- 大学での物理や工学で、運動方程式の理解を深めたい**大学生・大学院生**
- （物理をわかりやすく教えたい**大学教員**）
- 視覚化に興味がある**プログラミング初学者**や**エンジニア**

## 2.2. 目的
本文書の目的は、「**対象読者が自由に物理シミュレーションを行える環境**を与える」ことです。
そのために、可能な限りコードもシンプルに書いています。

## 2.4. 仮定 / 環境
特に固有の環境は必要ないです。私は以下の環境を用いております。
- windows11 / macbook air
- Python 3.10.6
- processing 4.3
- VSCode(コードは用意しているので編集時はメモ帳で構いません)
- GitHub（コード格納場所）

今回はこれらの環境のインストール方法は参考文書を示すことに留めたいと思います。
学校でも使える環境を想定してこのようにしています。
（unityを使えばよい気もしますが、学習コストと必要なスペックがそれなりに高いので）

----

## 2.5. 略語 / 用語
1. 物理シミュレータ：物理現象をコンピュータ上でシミュレーションするためのソフトウェア
2. グラフィカル：2Dや3Dで視覚的に分かり易いということ（数値のみではない）
3. python：簡単に書けるプログラミング言語、今回は演算を担当
4. processing：アニメーションを簡単に作れるプログラミング言語、今回はグラフィカルに表示する担当
5. GitHub：コードを格納しているwebサイト、ソフトウェア開発にはほぼ必須

# 3. 環境設定
## 3.1. インストール
本文書で紹介するコードを動かすためには、Python環境の準備と、Processingとの通信を行うための設定が必要です。PythonとProcessingの基本的なインストール方法については、それぞれの[Pythn japanのサイト](https://www.python.jp/install/windows/install.html)および[Processing公式サイトのドキュメント](https://processing.org/download)をご参照ください。

サイトの通りに進める（基本的にはwindowsならxxx.exeをダブルクリックして出てくる画面に従う）ことでインストール可能ですが、大体上手くいかないときの原因は以下なので困ったら以下のキーワードでトラブルシューティングしてみてください。
- 環境変数の設定（PCのどこからでもpythonやprocessingを使うためには、インストールされたフォルダの場所を環境変数に登録してオープンにする必要があります）
- （Windowsの場合）Microsoft Store版のpythonを使うように誘導されStore経由でインストールさせられている→アンインストールしてサイトの通り行う

環境設定で詰まり、調べても分からず、この記事の先に進めない場合は本記事にコメントください。

## 3.2. 必要なPythonライブラリのインストール
Pythonでシミュレーションを行うために、いくつかのライブラリが必要です。コマンドプロンプトやpowershell、terminal等を開き、以下のコマンドを使用して必要なライブラリをインストールしてください。

```bash
pip install numpy matplotlib
```
- **numpy**：数値計算を行うためのライブラリです。物体の位置・速度ベクトルや力の計算に使用します。
- **matplotlib**：データを可視化するためのライブラリで、シミュレーション結果のプロットに使用します。

デフォルトのPythonインストールにはソケット通信用のライブラリ**socket**が含まれていますので、追加インストールは不要

## 3.3. ソケット通信に関する設定
本節の設定は、別途特殊な設定をされている方向けです。

ソケット通信を行うには、ホストとポートの設定が必要です。この記事でのサンプルコードでは以下の設定を使用しますが、特に変更が必要な場合はご自身の環境に合わせて適宜調整してください。

- **ホスト**： 127.0.0.1（ローカルホストを指定します。PythonとProcessingが同じPC上で動作する前提です。）
- **ポート番号**： 10001（未使用のポートであることを確認し、他のソフトウェアと競合しないように設定してください。）

## 3.4. 今回使うプログラムのダウンロード
本文書のために作成したコードは以下の**GitHub**に格納しています。
https://github.com/hattori-sat/simulator-processing?tab=readme-ov-file

ダウンロードする方法は二つあります。
1. `git clone https://github.com/hattori-sat/simulator-processing.git`をターミナルで実行
2. リンク先で`Code`ボタンを押下し、`Download ZIP`を押下

![](/images/simulator-processing-python/github.png)

----

# 4. シンプルなプログラムの実践
ここから本題に入ります。

まずは、以下の動画のように立方体を回転させてみましょう。
https://www.youtube.com/watch?v=bUHnfvJCXHA

## 4.1. プログラムのデモ
3.4節の方法でダウンロードした結果、以下のようなフォルダ構成になっていると思います。
```
どこでもzipを解凍した場所（自分はDownloadsに解凍しました）
├─simulator-processing
│  ├─simple-rotation
│  │  └─rotation3DServer
│  └─two-body-simulator
│      └─twoBodySimulator
```

今回は、とりあえず`Downloadsフォルダ`にzipを回答した上記のフォルダ群があるものとして話を進めていきます。(どこにフォルダをおいても動作はしますが、`cd`などのコマンドの操作に慣れていない場合は同じ状況で動作させることをお勧めします。)

ここでは、`simple-rotation`のフォルダの中身を使っていきたいと思います。
以下の4つのフローで実行していきましょう。
1. `rotation3DServer`の中に入っているファイルをダブルクリック
2. Fig. 4.1-1の画面が開くので、左上の`▶ボタン`を押下
3. この状態でPowershellを立ち上げる
4. `rotation_client.py`を実行するために、以下のようにpowershellでコマンドを打つ(任意の方法で問題ないです)。
```bash
PS C:\Users\hatto> cd .\Downloads\
PS C:\Users\hatto\Downloads> cd .\simulator-processing-main\
PS C:\Users\hatto\Downloads\simulator-processing-main> cd .\simulator-processing-main\
PS C:\Users\hatto\Downloads\simulator-processing-main\simulator-processing-main> cd .\simple-rotation\
PS C:\Users\hatto\Downloads\simulator-processing-main\simulator-processing-main\simple-rotation> python .\rotation_client.py
```

![](/images/simulator-processing-python/processing.png)
*Fig.4.1-1 rotation3DServer.pdeを開いた結果*

## 4.2. プログラムの説明
ここでは簡単に説明をしていきたいと思います。
全体の説明は今回は省略したいと思います。

今回作成したプログラムは以下のような構成になっています。
![](/images/simulator-processing-python/consist.drawio.png)
*Fig. 4.2-1 processingとpythonの役割*

### 4.2.1. processing
コードの全体は以下のようになっています。（//のある行はコメントアウトと呼ばれ、プログラムには影響を与えないメモです。）
https://github.com/hattori-sat/simulator-processing/blob/main/simple-rotation/rotation3DServer/rotation3DServer.pde

**processing**では起動時に *setup()* 関数を実行します。
ここでは以下のことを行っています。
- processingによって開く画面のウィンドウサイズの設定
- pythonとの通信を行う*server*の設定

https://github.com/hattori-sat/simulator-processing/blob/main/simple-rotation/rotation3DServer/rotation3DServer.pde#L12-L20

その後に、*draw()* 関数をループします（何回も実行します）。
*draw()*関数の中でも以下の部分では、クライアント（今回はpythonのプログラム）からの接続があるかをチェックして、接続があればpythonから送られてきた文字列を読み取り、`rotationSpeedX`と`rotationSpeedX`に保存しています。
https://github.com/hattori-sat/simulator-processing/blob/main/simple-rotation/rotation3DServer/rotation3DServer.pde#L27-L37

4章のプログラムで理解していただきたいのは以下の点です。
- socket通信と呼ばれる通信でpythonと通信している
- socket通信で送られてくる**文字列を上手く変換してprocessingの変数に代入する**

### 4.2.2. python
コード全体は以下のようになっています。（#のある行はコメントアウトと呼ばれ、プログラムには影響を与えないメモです。）
https://github.com/hattori-sat/simulator-processing/blob/main/simple-rotation/rotation_client.py

以下のコードで、socket通信を行うための設定をしています。
https://github.com/hattori-sat/simulator-processing/blob/main/simple-rotation/rotation_client.py#L12-L14

4.2.1.項にてprocessingではpythonからの文字列を受け取っているということを説明しました。以下のコードがpythonからprocessingに文字列を送っている部分です。ここでは、1という文字が格納されている`message`を送っています。
https://github.com/hattori-sat/simulator-processing/blob/main/simple-rotation/rotation_client.py#L16-L20

socket通信なるものをしているとなんとなく理解していただければ、socket通信周りのコードを本文書では追う必要はありません。

### 4.2.3. プログラムがなぜ分けられているのか
機能ごとに分けることで修正しやすいよーーって感じのことを書く。

:::details メリット(プログラミングをよく学びたい人向け)
1. 責務の分離によるコードの明確化
   - Pythonは、物理シミュレーションの計算やサーバーとしての役割に特化しています。数値計算やデータの生成など、演算が中心の処理に向いているPythonの特徴を活かして、物体の動きや状態のシミュレーションを担当させています。
   - Processingは、ビジュアル表現に特化しています。Pythonから送信されたデータを受け取り、物体の軌跡や動きをグラフィカルに表示します。Processingの特徴である直感的なグラフィック処理を活かし、データの視覚化を担う役割として位置づけられています。

2. モジュール性の向上
PythonとProcessingを分けることで、それぞれのモジュール性が高まります。以下のメリットがあります。
   - Python側のシミュレーションコードを変更しても、Processing側の描画コードへの影響が最小限に抑えられるため、修正が容易になります。
   - Processing側の表示の詳細（色、形状、表示方法）を自由にカスタマイズでき、Python側のシミュレーションロジックを維持したまま、可視化の変更が可能です。

3. 性能と開発効率の最適化
PythonとProcessingを分けた構成により、各環境で得意な部分を活かした処理性能の最適化が可能になります。
   - Pythonがシミュレーション計算の負荷を持つことで、効率的な演算が行える一方、Python単体では得にくいグラフィック表示をProcessingが補います。
  - Processingが描画処理を担うため、計算負荷を軽減しながらリアルタイム性のあるビジュアライゼーションを実現しています。

1. 学習と拡張の容易さ
PythonとProcessingに分けることで、今後それぞれの環境に特化したライブラリや技術を学び、適用しやすくなります。例えば：
   - Pythonで新しい物理シミュレーションモデルやデータ構造、さらには他の計算ライブラリを組み込むなど、計算能力を向上させる方法を自由に追求できます。
   - Processingで新たなビジュアライゼーション手法やアニメーションの技術、あるいはユーザーインターフェースの改良に挑戦する余地が生まれます。
1. マルチプラットフォーム対応
今回の分離により、異なるプラットフォームでの動作や、ネットワーク越しのデータ送信なども容易になります。将来的に、異なるデバイス上で計算と表示を分担させるような、ネットワーク処理を伴う拡張もスムーズに行えます。
:::

# 5. 2つの物体の物理シミュレーション

https://www.youtube.com/watch?v=o2QZctsSqnk

$$
m_1\ddot{x}_1=-F \\
m_2\ddot{x}_2=F \\
F = \mu' m_1g
$$

----

# おわりに
本文書及び提供するコード自体が学習コンテンツとして利用できるようなものとなっていれば幸いです。
また、今回のコードではMITライセンスを付与しています。よく分からないと思いますので簡単に説明をすると、
- 私が書いたコードには著作権が自動的に発生します
- 著作権がある場合は誰かがそのコードを使用、改変、再配布するには、著作権者の許可が必要
- しかし、MITライセンスを付与すれば「著作権表示をすれば自由に使っていい」と示せます
- それにより、著作権表示があれば利用者は安心して使用や改変、再配布ができます

また、このコードを修正してより良いものになったときに私のGitHubにPullRequestをするなどして、これらのコードをより良いものにしていっていただけると非常に嬉しいところです。

# 参考文献
1. 高校物理ICT教材、https://physics.cloudfree.jp/
2. 

テンプレート：
https://zenn.dev/hattori_sat/articles/github-zenn-template-20240227