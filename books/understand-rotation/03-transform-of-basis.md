---
title: "基底変換と方向余弦行列"
free: true
---

前章では、基底の復習をしました。
しかし、一つの座標系しか存在していないため、まだメリットは享受できていません。

この章では新しく回転する座標系を定義して、基底を導入したメリットを感じましょう。
そのついでに、**基底変換**について学びます。

----

まずは、Fig.3-1のように2章で考えた慣性座標系Aに対して z軸が共通（$\mathbf{e}_3=\mathbf{e'}_3$）でz軸周りに角度 $\psi \in \mathbb{R}$ だけ回転させた直交基底座標系A'を定義します。

![](/images/understand-rotation/rotate-vector.drawio.png)
*Fig.3-1 2つ目の正規直行座標系の追加*

さて、先ほどと同様に基底を用いて位置ベクトル $\mathbf{r'}$ を表現すると

$$
\begin{equation}
\mathbf{r} = \begin{bmatrix} \mathbf{e'}_1&\mathbf{e'}_2&\mathbf{e'}_3 \end{bmatrix} \begin{bmatrix} x'_a \\ y'_a \\ z'_a  \end{bmatrix} 
\end{equation}
$$

となります（なお、ダッシュは微分と勘違いしやすいので注意してください。関係ないですが、ダッシュは空間に対しての微分、ドットは時間微分を表すことが多いです）。

ここで、$x'_a$、$y'_a$、$z'_a$ は**慣性座標系A'からみた質点aの成分**です。

こうなると座標系Aと座標系A'における成分表示の関係性を知りたくなります。

----

さて、このときにまずは簡単に求められそうな座標系Aと座標系A'の基底の関係を考えてみましょう。
Fig.3-1のx-y平面を垂直に見るとFig.3-2のようになっている。

![](/images/understand-rotation/rotation-vector-2.drawio.png)
*Fig.3-2 z軸方向から見たときの座標系間の関係*

このとき、片方の基底をもう片方の基底のベクトルの和で表現することを考えると

$$
\begin{equation}
\mathbf{e'}_1=(\cos{\psi})\mathbf{e}_1+(\sin{\psi})\mathbf{e}_2
\end{equation}
$$

$$
\begin{equation}
\mathbf{e'}_2=(-\sin{\psi})\mathbf{e}_1+(\cos{\psi})\mathbf{e}_2
\end{equation}
$$

$$
\begin{equation}
\mathbf{e'}_3=mathbf{e}_3
\end{equation}
$$

という関係性を得られます。この関係性を行列を用いて表現してみると

$$
\begin{equation}
\begin{bmatrix} \mathbf{e'}_1 \\ \mathbf{e'}_2 \\ \mathbf{e'}_3  \end{bmatrix} = \begin{bmatrix} \cos{\psi} & \sin{\psi} & 0 \\ -\sin{\psi} & \cos{\psi} & 0 \\ 0 & 0 & 1  \end{bmatrix} \begin{bmatrix} \mathbf{e}_1 \\ \mathbf{e}_2 \\ \mathbf{e}_3  \end{bmatrix}
\end{equation}
$$

となります。この式(5)を行列 $\mathbf{C}\in \mathbb{R}^{3\times 3}$を用いて

$$
\begin{equation}
    \mathbf{C} = \begin{bmatrix} \cos{\psi} & \sin{\psi} & 0 \\ -\sin{\psi} & \cos{\psi} & 0 \\ 0 & 0 & 1  \end{bmatrix}
\end{equation}
$$

としたとき、$\mathbf{C}$ は**基底変換行列**と呼ばれます。

また、Fig.3-1から想像されたと思いますが、単にz軸周りに回転させただけの回転となっています。そのため、$\mathbf{C}$は**回転行列**とも呼ばれます。

さて、式(5)に式(6)を代入したうえで転置行列をかけたいと思います。

$$
\begin{equation}
\begin{bmatrix} \mathbf{e'}_1 \\ \mathbf{e'}_2 \\ \mathbf{e'}_3  \end{bmatrix} \begin{bmatrix} \mathbf{e}_1 & \mathbf{e}_2 & \mathbf{e}_3  \end{bmatrix}= \mathbf{C}\begin{bmatrix} \mathbf{e}_1 \\ \mathbf{e}_2 \\ \mathbf{e}_3  \end{bmatrix}\begin{bmatrix} \mathbf{e}_1 & \mathbf{e}_2 & \mathbf{e}_3  \end{bmatrix}
\end{equation}
$$

さて、ここで次の章で紹介する**行列の性質**の一部を使いたいと思います(※私は最初このあたりの行列の性質に苦しめられました)。

式(7)は**行列の性質**を用いて変換を行うと

$$
\begin{equation}
   \begin{bmatrix} \mathbf{e'}_1\cdot \mathbf{e}_1 & \mathbf{e'}_1\cdot \mathbf{e}_2 & \mathbf{e'}_1\cdot \mathbf{e}_3 \\ \mathbf{e'}_2\cdot \mathbf{e}_1 & \mathbf{e'}_2\cdot \mathbf{e}_2 & \mathbf{e'}_2\cdot \mathbf{e}_3 \\ \mathbf{e'}_3\cdot \mathbf{e}_3 & \mathbf{e'}_3\cdot \mathbf{e}_2 & \mathbf{e'}_3\cdot \mathbf{e}_3  \end{bmatrix} = \mathbf{C} \begin{bmatrix} 1&0&0\\0&1&0\\0&0&1 \end{bmatrix}
\end{equation} 
$$

となり、等号の左右を入れ替えると、

$$
\begin{equation}
   \mathbf{C} = \begin{bmatrix} \mathbf{e'}_1\cdot \mathbf{e}_1 & \mathbf{e'}_1\cdot \mathbf{e}_2 & \mathbf{e'}_1\cdot \mathbf{e}_3 \\ \mathbf{e'}_2\cdot \mathbf{e}_1 & \mathbf{e'}_2\cdot \mathbf{e}_2 & \mathbf{e'}_2\cdot \mathbf{e}_3 \\ \mathbf{e'}_3\cdot \mathbf{e}_3 & \mathbf{e'}_3\cdot \mathbf{e}_2 & \mathbf{e'}_3\cdot \mathbf{e}_3  \end{bmatrix} 
\end{equation} 
$$

となる。さて、先ほど$\mathbf{C}$を基底変換行列や回転行列と呼ぶと説明しましたが、まだ別名があります。
式(9)をみると内積がたくさんあることが分かると思います。

ここで、内積の定義を確認しておくと（あまり意味はないが）角度 $\alpha\in\mathbb{R}$ を用いて

$$
\begin{equation}
     \mathbf{e'}_1\cdot \mathbf{e}_1=| \mathbf{e'}_1| |\mathbf{e}_1| \cos{\alpha}
\end{equation}
$$

となります。つまり、**「内積をとる」と「cos」が出てきます**。cosineは日本語で**余弦**です。

このことから式(9)には内積がたくさんありますので、
**方向余弦行列(DCM : Direct Cosine Matric)** とも呼ばれます。

----

以下キーワードは少しチェックしておくとよいかもしれません。
テンソル積・クロネッカー積、
参考：グラム行列、https://mathlandscape.com/gram-matrix/