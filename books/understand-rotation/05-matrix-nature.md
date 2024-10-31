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

