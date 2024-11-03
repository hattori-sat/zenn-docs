---
title: "回転に関係する数学の基礎知識"
free: true
---

本章では、三次元の行列において**回転に回転する性質**を纏めておきたいと思います。
また、本章では行列の名前についても定義していきたいと思います。
(三次元でなくても成り立つ性質のものが多いですが、ここでは三次元について考えます。)

----

- [既出の事柄](#既出の事柄)
  - [Define-1: 空間の定義](#define-1-空間の定義)
  - [Define-2: 回転変換の表現行列](#define-2-回転変換の表現行列)
  - [Define-3: 方向余弦行列](#define-3-方向余弦行列)
  - [Nature-1: 内積の集合](#nature-1-内積の集合)
- [行列について](#行列について)
  - [Define-4: 行列の積](#define-4-行列の積)
  - [Define-5: 行列の演算](#define-5-行列の演算)
  - [Define-6: 正方行列](#define-6-正方行列)
  - [Define-7: 対称行列](#define-7-対称行列)
  - [Nature-2 正方行列の分解と正定行列](#nature-2-正方行列の分解と正定行列)
  - [Define-8: 特殊直交群 SO(3)](#define-8-特殊直交群-so3)
  - [Define-9: 交代行列 so(3)](#define-9-交代行列-so3)
  - [Nature-3: so(3)と外積](#nature-3-so3と外積)
  - [Nature-4: 回転行列 SO(3)](#nature-4-回転行列-so3)


----

# 既出の事柄

## Define-1: 空間の定義
$\mathbb{A}, \mathbb{B}$ は、基底を正規直行基底に採ったときの三次元のユークリッド空間であるとする。

----

## Define-2: 回転変換の表現行列
3章で **「大学数学で出てくる回転行列」** と仮称した行列のことです。
ユークリッド空間内において原点を中心とした回転を表現します。

$\begin{Vmatrix} \mathbf{a} \end{Vmatrix}=\begin{Vmatrix} \mathbf{a}' \end{Vmatrix}$ となる $\mathbf{a}, \mathbf{a}' \in \mathbb{A}$ を用いて

$$
\begin{equation}
    \mathbf{a}' = \mathbf{R}\mathbf{a}
\end{equation}
$$

となる $\mathbf{R}\in \mathbb{R}^{3\times 3}$ を回転変換の表現行列と呼ぶ。

----

## Define-3: 方向余弦行列
3章（以下のページ）の式(9)の式を方向余弦行列と定義することにします。
https://zenn.dev/hattori_sat/books/understand-rotation/viewer/03-transform-of-basis

なお、$\mathbb{A}, \mathbb{B}$の2つの基底の関係性を示す行列のことと考える。

----

## Nature-1: 内積の集合
3章で方向余弦行列を示しましたが、このときの式変換の説明は後回しにしました。
ですので、ここで簡単に説明したいと思います。

$\mathbb{A},\mathbb{B}$ の基底（Define-1から正規直行基底）を $\mathbf{a}_1,\mathbf{a}_2,\mathbf{a}_3$ と $\mathbf{b}_1,\mathbf{b}_2,\mathbf{b}_3$ とします。
位置ベクトル $\mathbf{r}_a$ は2章から

$$
\begin{equation}
   \mathbf{r}_a = x_a \mathbf{a}_1 + y_a \mathbf{a}_2 + z_a \mathbf{a}_3 = \begin{bmatrix} \mathbf{a}_1 & \mathbf{a}_2 & \mathbf{a}_3  \end{bmatrix}\begin{bmatrix} x_a \\ y_a \\ z_a \end{bmatrix}  
\end{equation}
$$

と表現しました。このとき、基底を行ベクトルとして表示していました。これは $\mathbf{a}_1$の成分を$\forall i \in \{1,2,3\},\quad a_{1i}$ のように表示することにします。他の基底に対しても同様とします。

そうすると、式(2)の基底ベクトルを行ベクトルで表現している部分は

$$
\begin{equation}
    \begin{bmatrix} \mathbf{a}_1 & \mathbf{a}_2 & \mathbf{a}_3  \end{bmatrix} = \begin{bmatrix}
        a_{11} & a_{21} & a_{31} \\ a_{12} & a_{22} & a_{32} \\ a_{13} & a_{23} & a_{33}
    \end{bmatrix} \equiv　\mathbf{R}_a 
\end{equation}
$$

を意味しています($\equiv$ は定義するという意味)。さて、方向余弦行列 $\mathbf{C} \in \mathbb{R}^{3\times 3}$ は

$$
\begin{equation}
    \mathbf{C} = \mathbf{R}_b^T \mathbf{R}_a 
\end{equation}
$$

と表現できます(式(4)の両辺を転置すれば3章の式となる)。ここで、

$$
\begin{equation}
    \mathbf{R}_b^T =  \begin{bmatrix} \mathbf{b}_1^T \\ \mathbf{b}_2^T \\ \mathbf{b}_3^T  \end{bmatrix}
\end{equation}
$$

と表せるため、方向余弦行列は

$$
\begin{equation}
    \mathbf{C} = \begin{bmatrix} \mathbf{b}_1^T \\ \mathbf{b}_2^T \\ \mathbf{b}_3^T  \end{bmatrix}\begin{bmatrix} \mathbf{a}_1 & \mathbf{a}_2 & \mathbf{a}_3  \end{bmatrix}
\end{equation}
$$

と表せるが、この式には最初違和感を感じるのではないでしょうか。なぜならば、これまでは式(3)のように基底ベクトルの順序対をベクトルとして取り扱ってきました。しかし、式(6)のような演算はユークリッド空間のベクトルでは定義されていないからです。

:::message
物理を学んでいると式(6)と同じ形式だが、行列の成分がスカラーであるものをよく見かける。
これはダイヤディック、二項積（dyadic）と呼ばれるものであり、**テンソル**（tersor）を定義する必要があります。 
私の実力ではテンソルを説明することは難しいので、今回は諦めることとします。
（テンソル、双対空間あたりは参考文書[3] を参考にするとよいと思います。）
:::

式(6) は単に行列の積ですので、テンソルを導入しなくても理解することが出来ます。式(6)の行列を成分表示して書くと

$$
\begin{equation}
    \mathbf{C} = \begin{bmatrix}
        b_{11} & b_{12} & b_{13} \\ b_{21} & b_{22} & b_{23} \\ b_{31} & b_{32} & b_{33}
    \end{bmatrix} \begin{bmatrix}
        a_{11} & a_{21} & a_{31} \\ a_{12} & a_{22} & a_{32} \\ a_{13} & a_{23} & a_{33}
    \end{bmatrix} 
\end{equation}
$$

であり、このとき、方向余弦行列$\mathbf {C}$ の成分を一つ抜き出して書くと

$$
\begin{equation}
    [\mathbf{C}_{11}]= b_{11}a_{11}+b_{12}a_{12}+b_{13}a_{13}=\mathbf{b}_{1}\cdot \mathbf{a}_1=\mathbf{b}_{1}^T\mathbf{a}_1
\end{equation}
$$

となるため、

$$
\begin{equation}
   \mathbf{C} = \begin{bmatrix} \mathbf{b}_1\cdot \mathbf{a}_1 & \mathbf{b}_1\cdot \mathbf{a}_2 & \mathbf{b}_1\cdot \mathbf{a}_3 \\ \mathbf{b}_2\cdot \mathbf{a}_1 & \mathbf{b}_2\cdot \mathbf{a}_2 & \mathbf{b}_2\cdot \mathbf{a}_3 \\ \mathbf{b}_3\cdot \mathbf{a}_3 & \mathbf{b}_3\cdot \mathbf{a}_2 & \mathbf{b}_3\cdot \mathbf{a}_3  \end{bmatrix} = \begin{bmatrix} \mathbf{b}_1^T \mathbf{a}_1 & \mathbf{b}_1^T \mathbf{a}_2 & \mathbf{b}_1^T \mathbf{a}_3 \\ \mathbf{b}_2^T \mathbf{a}_1 & \mathbf{b}_2^T \mathbf{a}_2 & \mathbf{b}_2^T \mathbf{a}_3 \\ \mathbf{b}_3^T \mathbf{a}_3 & \mathbf{b}_3^T \mathbf{a}_2 & \mathbf{b}_3^T \mathbf{a}_3  \end{bmatrix} 
\end{equation} 
$$

と表現することが出来ます。これによって方向余弦行列の定義が理解できるようになると思います。
（なぜこんな複雑？な説明をしているかというと、姿勢について分かり易く説明してくれている教科書や論文であってもこの辺りの説明を書いてくれている文書は少ないからです。かといって、ニッチなのでネットでもあまり書かれていません。）

----

# 行列について
ここでは、行列についてすべて説明することはしません。
使うものと注意してほしいことだけ記述します。

## Define-4: 行列の積
この辺りの説明は不要かと思いますが、表記の確認のために定義します。
行列 $\mathbf{A},\mathbf{B},\mathbf{C} \in \mathbb{R}^{3\times 3}$ は、成分 $a_{ij},b_{ij},c_{ij} \in \mathbb{R}$ を用いてそれぞれ $\mathbf{A}=[a_{ij}]$、$\mathbf{B}=[b_{ij}]$、$\mathbf{C}=[c_{ij}]$ と表す。行列の積は、

$$
\begin{equation}
    \mathbf{C}=\mathbf{A}\mathbf{B}=[{\sum^3_{k=1}a_{ik}b_{kj}}]
\end{equation}
$$

と書けます。

----

## Define-5: 行列の演算
さて、4章では「＋」という演算子をベクトル空間において定義しました。ここで行列における演算を定義しておきましょう。
まず、「＋」についてです。なお、三次元について考えており、回転に関係する三次元の正方行列についてのみ考えることに注意してください。

$$
\begin{equation}
    \mathbf{A}+\mathbf{B}= [a_{ij}+b_{ij}]
\end{equation}
$$

となります。つまり、同じ成分（雑な言い方をすれば行列において同じ位置にあるもの）を（実数空間で定義されている）足し算を行います。

次は「-」についてです。これも「+」と同様に（というよりかは「+」により）定義されます。

$$
\begin{equation}
    \mathbf{A}-\mathbf{B}= [a_{ij}-b_{ij}]
\end{equation}
$$

次は「×」についてですが、行列では定義されていません。その代わりにDefine-4のような積の未定義されています。
一方、ユークリッド空間のベクトルについては「×」を用いて外積が定義されています。

:::message
本書でも1行3列もしくは3行1列の行列を列ベクトル、行ベクトルと呼んでいますが、基本的にベクトルとしてではなく行列として扱っていることに注意してください。
ただし、これらは**ベクトルと同一視**して取り扱うことで内積や外積などを使うことが出来るため、断りなくベクトルとして使うことがあることをここで謝らせてください（方向余弦行列の内積ですでにベクトルと同一視しております）。
:::

これは定義でなく重要な性質になりますが、$\mathbf{A},\mathbf{B}$の一般的な交換法則は成りたちません（掛け算の順序問題で謎に引き合いに出される可哀そうな性質です）。

割り算「÷」も行列では定義されていません。しかし、Define-4の積が定義されていることにより**逆行列**という割り算相当のものも定義されています。
今回は逆行列の定義等は知っているものとして、単位行列（未定義ですが）を $\mathbf{E}$ として

$$
\begin{equation}
    \mathbf{A}\mathbf{A}^{-1}=\mathbf{A}^{-1}\mathbf{A}=\mathbf{E},\quad\mathbf{E}=\mathrm{diag}(1,1,1),\quad \begin{vmatrix}
        \mathbf{A}
    \end{vmatrix}\ne0
\end{equation}
$$

となるような $\mathbf{A}^{-1}$ とします。(スカラーの割り算)

一方、これ以降勘違いしがちな概念として、**転置**があります。
転置の定義は

$$
\begin{equation}
    \mathbf{A}^T=[a_{ji}]
\end{equation}
$$

となります。

----

## Define-6: 正方行列
行列 $\mathbf{A}=[a_{ij}]$ において、i=j の行列のこと。
（とはいえ、今回Define-4 で $\mathbb{R}^{3\times 3}$ としているので出てくる行列はすべて正方行列です。）

----

## Define-7: 対称行列
正方行列のうち、$\mathbf{A}=\mathbf{A}^T$ を満たすもの。
（成分が対角成分を軸として線対称のようになっている）

今回はあまり固有値については出てこないので、定義の中で性質を紹介してしまいますが、**実対称行列の固有値**は全て実数です。
(そして、実対称行列は対角化可能である)

----

## Nature-2 正方行列の分解と正定行列
とりあえず、式展開を

$$
\begin{equation}
    \mathbf{A}=\frac{1}{2}\begin{pmatrix}\mathbf{A}+\mathbf{A}^T\end{pmatrix}+\frac{1}{2}\begin{pmatrix}\mathbf{A}-\mathbf{A}^T\end{pmatrix}
\end{equation}
$$

と行います。すると、式(15)の右辺第一項は**対称行列**になります。

さて、二次形式（説明せずに使います）を考えてみます。$\mathbf{x}\in \mathbb{A}$ を用いて

$$
\mathbf{x}^T \mathbf{A} \mathbf{x}
$$

を考えると、

$$
\mathbf{x}^T \begin{Bmatrix}
    \frac{1}{2}\begin{pmatrix}\mathbf{A}-\mathbf{A}^T\end{pmatrix}
\end{Bmatrix}\mathbf{x} = 0
$$

となります（三次元であれば愚直に計算すれば導けます）。
よって、正方行列であれば二次形式を採ることが出来ます（とはいえ、物理においては対称行列が多く出てきます）。

## Define-8: 特殊直交群 SO(3)
さて、回転行列（Define-2とDefine-3）に深く関わる集合を定義する。本書で使っている $\mathbf{A}$ のうち

$$
\begin{equation}
    \mathrm{SO}(3)=\begin{Bmatrix}
        \mathbf{A} | \det\mathbf{A}=+1, \quad\mathbf{A}^T\mathbf{A}=\mathbf{E}
    \end{Bmatrix}
\end{equation}
$$

である集合。さて、実は2章の最後に「大学数学で出てくる回転行列」を説明する際にこれは出てきています。
（なお、$|\det\mathbf{A}|=1$ は直交行列と呼ばれることに注意する。すべてのSO(3)は直交行列であるが、すべての直交行列がSO(3)であるわけではないです。）

ここはかなり納得しがたい部分であるとは思うのだが、ここからは説明しやすいように $\mathbf{A}\in \mathrm{SO}(3)$ を**回転行列**と呼ぶことにする。

----

## Define-9: 交代行列 so(3)
Define-8 と同じようなものが出てきました。この行列はかなり回転に関係します。
定義は、

$$
\begin{equation}
    \mathrm{so}(3)=\begin{Bmatrix}
        \mathbf{A}\quad|\quad\mathbf{A}^T=-\mathbf{A}
    \end{Bmatrix}
\end{equation}
$$

となります。

----

## Nature-3: so(3)と外積
ここで、$\omega_1,\omega_2,\omega_3\in\mathbb{R}$ を用いて、

$$
\begin{equation}
    \mathbf{A}=\begin{bmatrix}
    0 & -\omega_3 & \omega_2 \\ \omega_3  & 0 &-\omega_1 \\ -\omega_2 & \omega_1 & 0
\end{bmatrix}
\end{equation}
$$

という行列 $\mathbf{A}$ を考えてみる。この行列の転置は

$$
\begin{equation}
    \mathbf{A}^T=-\begin{bmatrix}
    0 & -\omega_3 & \omega_2 \\ \omega_3  & 0 &-\omega_1 \\ -\omega_2 & \omega_1 & 0
\end{bmatrix}
\end{equation}
$$

と表せるため、この行列 $\mathbf{A}$ はso(3)である。ここで、$\mathbf{r}=[x,y,z]^T\in \mathbb{A}$ を用いて、

$$
\begin{equation}
    \mathbf{A}\mathbf{r}=\begin{bmatrix}
    0 & -\omega_3 & \omega_2 \\ \omega_3  & 0 &-\omega_1 \\ -\omega_2 & \omega_1 & 0
\end{bmatrix}\begin{bmatrix}x\\y\\z\end{bmatrix}=\begin{bmatrix}\omega_2 z-\omega_3 y \\ \omega_3 x -\omega_1 z \\ \omega_1 y -\omega_2 x \end{bmatrix}
\end{equation}
$$

と導出できます。一方、$\mathbf{\Omega}=\begin{bmatrix}\omega_1 & \omega_2 & \omega_3\end{bmatrix}^T$ を用いてベクトルと同一視することで外積をとると

$$
\begin{equation}
    \mathbf{\Omega}\times\mathbf{r}=\begin{bmatrix}\omega_1 \\ \omega_2 \\ \omega_3\end{bmatrix}\times \begin{bmatrix}x\\y\\z\end{bmatrix}=\begin{bmatrix}\omega_2 z-\omega_3 y \\ \omega_3 x -\omega_1 z \\  \omega_1 y -\omega_2 x\end{bmatrix}
\end{equation}
$$

となります。さて、式(20)と式(21)を比較すると

$$
\begin{equation}
    \mathbf{A}\mathbf{r}=\mathbf{\Omega}\times\mathbf{r}
\end{equation}
$$

という等号を結ぶことが出来ます（恣意的に変数名にオメガを使いましたが、式(22)は一般的な議論）。さて、式(22)を見てみると以下のことが予測できると思います。
- 行列 $\mathbf{A}$ と 外積 $\mathbf{\Omega\times}$ が同じ作用をしている

つまり、so(3)はベクトルで定義されている外積を行列形式で表現可能な行列です。一般には**交代行列**と呼ばれます。

----

## Nature-4: 回転行列 SO(3)
ここで、$\mathbf{A},\mathbf{B}\in \mathrm{SO}(3)$ があるとき、

$$
\begin{equation}
    \det{\mathbf{A}\mathbf{B}}=\det{\mathbf{A}}\det{\mathbf{B}}=+1
\end{equation}
$$

であり、

$$
    \mathbf{C}=\mathbf{B}\mathbf{A}
$$

$$
    \mathbf{C}^T=\mathbf{A}^T\mathbf{B}^T
$$

とすると、

$$
\begin{equation}
    \mathbf{C}^T\mathbf{C}=\mathbf{A}^T\mathbf{B}^T\mathbf{B}\mathbf{A}=\mathbf{E}
\end{equation}
$$

$$
\begin{equation}
    \mathbf{C}\mathbf{C}^T=\mathbf{B}\mathbf{A}\mathbf{A}^T\mathbf{B}^T=\mathbf{E}
\end{equation}
$$

であるので式(16)を満たしているため、$\mathbf{C}$ はSO(3)となります。

また、回転行列の $n$個 の積も回転行列となり、SO(3)となります。

----

さて、このくらい行列について理解をしておけばオイラー角やクォータニオンを理解するのには十分だと思います。
ここからは本題であるオイラー角についてみていきましょう。

----