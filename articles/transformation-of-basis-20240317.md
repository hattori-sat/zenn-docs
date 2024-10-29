---
title: "【回転行列の理解のために】基底変換の説明"
emoji: "🐥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["基底変換","姿勢","座標変換","線形代数"]
published: true
---
# はじめに
本文書では、基底の変換について説明します。


物理を学んでいると至る所で**ベクトル**が出てきます。
例えば、位置ベクトル、速度ベクトルなどです。

そのほかにも行列を多用するようになります。
行列を用いて表現することで簡潔に表現することが出来、さらに行列の性質によって多くのメリットを享受することが出来ます。

さて、そんなこんなで物理を学んでいるとB3、B4あたりで「ロボット工学」「衛星工学」などの講義を受けることになりました。

----

# ベクトルの表示
普段、数学や物理を勉強する際、ベクトルが出てきたら、以下のような書き方をするのではないでしょうか。

$$ \mathbf{r} = \begin{bmatrix} x_a \\ y_a \\ z_a \end{bmatrix}　\tag{1} $$


式(1)のような表示を**成分表示**と呼びます。

一方で、基底ベクトル用いて

$$ \mathbf{r}=x_a \mathbf{a}_1+y_a\mathbf{a}_2+z_a\mathbf{a}_\mathbf{3} \tag{2} $$

と書けます。そして、式(2)を行列表示すると

$$ \mathbf{r} = \begin{bmatrix} \mathbf{a}_1&\mathbf{a}_2&\mathbf{a}_3 \end{bmatrix} \begin{bmatrix} x_a \\ y_a \\ z_a  \end{bmatrix} \tag{3} $$

というようになります。

----

# 基底変換（座標変換）

式(2)とは異なる座標系の基底を用いて 位置ベクトル $\mathbf{r}$ は

$$ \mathbf{r}=x_b \mathbf{b}_\mathbf{1}+y_b \mathbf{b}_\mathbf{2}+z_b \mathbf{b}_\mathbf{3}= \begin{bmatrix}\mathbf{b}_\mathbf{1}&\mathbf{b}_\mathbf{2}&\mathbf{b}_\mathbf{3}\ \end{bmatrix} \begin{bmatrix} x_b \\ y_b \\ z_b \end{bmatrix} \tag{4} $$

と書くことが出来る。

ここで、座標系Bの基底ベクトルが座標系Aの基底ベクトルの線形結合で表せるとすると

$$ \mathbf{b}_1=c_{11} \mathbf{a}_1+c_{12} \mathbf{a}_2+c_{13} \mathbf{a}_3  \tag{5} $$

$$ \mathbf{b}_2=c_{21} \mathbf{a}_1+c_{22} \mathbf{a}_2+c_{23} \mathbf{a}_3 \tag{6} $$

$$ \mathbf{b}_3=c_{31} \mathbf{a}_1+c_{32} \mathbf{a}_2+c_{33} \mathbf{a}_3  \tag{7} $$

と表すことが出来る。これを行列表示すると

$$ \begin{bmatrix} \mathbf{b}_{1} \\ \mathbf{b}_{2} \\ \mathbf{b}_{3} \\ \end{bmatrix} = \begin{bmatrix} c_{11} & c_{12} & c_{13} \\ c_{21} & c_{22} & c_{23} \\ c_{31} & c_{32} & c_{33}\\ \end{bmatrix}  \begin{bmatrix} \mathbf{a}_{1}\\\mathbf{a}_{2}\\\mathbf{a}_{3} \\ \end{bmatrix} = \mathbf{C} \begin{bmatrix} \mathbf{a}_{1} \\ \mathbf{a}_{2}\\\mathbf{a}_{3} \\ \end{bmatrix}  \tag{8} $$


と表すことが出来る。ここで、行列 $\mathbf{C}$ は方向余弦行列と呼ばれる。

あたらめて方向余弦行列 $\mathbf{C}$ を書くと

$$ \mathbf{C} =  \begin{bmatrix} c_{11} & c_{12} & c_{13} \\ c_{21} & c_{22} & c_{23} \\ c_{31} & c_{32} & c_{33}\\ \end{bmatrix} \tag{9} $$

となる。式(8)を転置すると

$$ \begin{bmatrix}\mathbf{b}_1&\mathbf{b}_2&\mathbf{b}_3\\\end{bmatrix}=\begin{bmatrix}\mathbf{a}_\mathbf{1}&\mathbf{a}_\mathbf{2}&\mathbf{a}_\mathbf{3}\\ \end{bmatrix} \mathbf{C}^{T} \tag{10} $$

となる。この関係式を用いて、座標系Bからみた成分を座標系Aの基底を用いて表現してみる。

$$ \mathbf{r} = \begin{bmatrix} \mathbf{b}_{1} & \mathbf{b}_{2} & \mathbf{b}_{3} \\ \end{bmatrix} \begin{bmatrix} x_b \\ y_b\\ z_b\\ \end{bmatrix} = \begin{bmatrix} \mathbf{a}_{1} & \mathbf{a}_{2} & \mathbf{a}_{3}\\ \end{bmatrix} \mathbf{C}^{T} 
\begin{bmatrix} {x}_{b} \\ {y}_{b} \\ {z}_{b} \\ \end{bmatrix} 
= \begin{bmatrix} \mathbf{a}_{1} & \mathbf{a}_{2} & \mathbf{a}_{3}\\ \end{bmatrix} 
\begin{bmatrix} x_{a}\\ {y}_{a}\\ {z}_{a}\\ \end{bmatrix} \tag{11} $$

このように座標系Bの成分を用いて表現できることが分かった。  
座標系Aの基底で揃っているので、座標系同士の成分の関係性をみてみると

$$ \mathbf{C}^{T} \begin{bmatrix} x_b \\ y_b \\ z_b \\ \end{bmatrix} = \begin{bmatrix} x_a\\y_a\\z_a\\\end{bmatrix} \tag{12} $$

となる。

このように異なる座標系の成分であっても、方向余弦行列を求めることが出来れば関係を知ることが出来る。

今回は方向余弦行列を取り扱ったが、オイラー角やクォータニオンなども使えるので、勉強してみてほしい。

----
# おわりに
ご覧いただきありがとうございます．
より詳細の内容は動画をご覧ください．

https://youtu.be/eqH0I3-Plaw?si=BZOBkIctm5PuMBxO


* note  

https://note.com/hattoriofpigeon/

----

# 参考文献
[![](https://m.media-amazon.com/images/I/41ZSRYR783L._SY466_.jpg)](https://amzn.to/3TOUSBb)

---