---
title: "回転に関係する数学の基礎知識"
free: true
---

本章では、三次元の行列において**回転に回転する性質**を纏めておきたいと思います。
また、本章では行列の名前についても定義していきたいと思います。
(三次元でなくても成り立つ性質のものが多いですが、ここでは三次元について考えます。)

----

- [Define-1: 空間の定義](#define-1-空間の定義)
- [Define-2: 回転変換の表現行列](#define-2-回転変換の表現行列)
- [Define-3: 方向余弦行列](#define-3-方向余弦行列)
- [Nature-1: 内積の集合](#nature-1-内積の集合)


----

# Define-1: 空間の定義
$\mathbb{A}, \mathbb{B}$ は、基底を正規直行基底に採ったときの三次元のユークリッド空間であるとする。

----

# Define-2: 回転変換の表現行列
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

# Define-3: 方向余弦行列
3章（以下のページ）の式(9)の式を方向余弦行列と定義することにします。
https://zenn.dev/hattori_sat/books/understand-rotation/viewer/03-transform-of-basis

なお、$\mathbb{A}, \mathbb{B}$の2つの基底の関係性を示す行列のことと考える。

----

# Nature-1: 内積の集合
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

と表現できます。ここで、

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
   \mathbf{C} = \begin{bmatrix} \mathbf{b}_1\cdot \mathbf{e}_1 & \mathbf{e'}_1\cdot \mathbf{e}_2 & \mathbf{e'}_1\cdot \mathbf{e}_3 \\ \mathbf{e'}_2\cdot \mathbf{e}_1 & \mathbf{e'}_2\cdot \mathbf{e}_2 & \mathbf{e'}_2\cdot \mathbf{e}_3 \\ \mathbf{e'}_3\cdot \mathbf{e}_3 & \mathbf{e'}_3\cdot \mathbf{e}_2 & \mathbf{e'}_3\cdot \mathbf{e}_3  \end{bmatrix} 
\end{equation} 
$$

----

