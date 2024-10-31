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
\mathbf{r'} = \begin{bmatrix} \mathbf{e'}_1&\mathbf{e'}_2&\mathbf{e'}_3 \end{bmatrix} \begin{bmatrix} x'_a \\ y'_a \\ z'_a  \end{bmatrix} 
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

$$\begin{equation}
\mathbf{e'}_1=(\cos{\psi})\mathbf{e}_1+(\sin{\psi})\mathbf{e}_2
\end{equation}$$

$$
\begin{equation}
\mathbf{e'}_2=(-\sin{\psi})\mathbf{e}_1+(\cos{\psi})\mathbf{e}_2
\end{equation}
$$

$$\begin{equation}
\mathbf{e'}_3=\mathbf{e}_3
\end{equation}$$

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

さて、本題に戻ります。本章では座標系Aと座標系A'の成分の関係を知ることが目的でした。

あらためて、式(5)に式(6)を用いて簡潔に表現すると

$$
\begin{equation}
\begin{bmatrix} \mathbf{e'}_1 \\ \mathbf{e'}_2 \\ \mathbf{e'}_3  \end{bmatrix} = \mathbf{C}\begin{bmatrix} \mathbf{e}_1 \\ \mathbf{e}_2 \\ \mathbf{e}_3  \end{bmatrix}
\end{equation}
$$

となります。この式(12)を両辺転置すると（この関係性も次の章でまとめて説明します）

$$
\begin{equation}
\begin{bmatrix} \mathbf{e'}_1 & \mathbf{e'}_2 & \mathbf{e'}_3  \end{bmatrix} = \begin{bmatrix} \mathbf{e}_1 & \mathbf{e}_2 & \mathbf{e}_3  \end{bmatrix}\mathbf{C}^T
\end{equation}
$$

となります。この式(12)の形はどこかで見たことある形となっています。再度式(1)を示すと

$$
\mathbf{r'} = \begin{bmatrix} \mathbf{e'}_1&\mathbf{e'}_2&\mathbf{e'}_3 \end{bmatrix} \begin{bmatrix} x'_a \\ y'_a \\ z'_a  \end{bmatrix} 
$$

という形でした。この式に式(12)を代入すると

$$
\begin{equation}
   \mathbf{r'} = \begin{bmatrix} \mathbf{e'}_1&\mathbf{e'}_2&\mathbf{e'}_3 \end{bmatrix} \begin{bmatrix} x'_a \\ y'_a \\ z'_a  \end{bmatrix}=\begin{bmatrix} \mathbf{e}_1 & \mathbf{e}_2 & \mathbf{e}_3  \end{bmatrix}\mathbf{C}^T\begin{bmatrix} x'_a \\ y'_a \\ z'_a  \end{bmatrix}
\end{equation}
$$

となる。前章の式(3)を以下に再度示すと

$$
\begin{equation}
\mathbf{r} = \begin{bmatrix} \mathbf{e}_1&\mathbf{e}_2&\mathbf{e}_3 \end{bmatrix} \begin{bmatrix} x_a \\ y_a \\ z_a  \end{bmatrix} 
\end{equation}
$$

であった。このとき、式(13)と式(14)をみると同じ基底ベクトルを用いて表現されていることに気づくと思います。

さて、質点aに対して異なる座標系から見たときの位置ベクトルを式(13)と式(14)に示したわけですが、式(13)では座標系Aの基底ベクトルを用いて式を表現できています。同じ基底ベクトルを用いて表現したとき、質点aの位置ベクトルの成分は同じになるはずなので、

$$
\begin{equation}
 \mathbf{C}^T\begin{bmatrix} x'_a \\ y'_a \\ z'_a  \end{bmatrix}=\begin{bmatrix} x_a \\ y_a \\ z_a  \end{bmatrix} 
\end{equation}
$$


これはつまり、「座標系A'における成分の値」を使って「座標系Aでみたときの成分表示の値」を表現できることになります。

さて、またもや次章で説明する行列の知識を用いる（回転行列は**直交行列**であり、転置行列と逆行列が一致する）と

$$
\begin{equation}
 \begin{bmatrix} x'_a \\ y'_a \\ z'_a  \end{bmatrix}=\mathbf{C}\begin{bmatrix} x_a \\ y_a \\ z_a  \end{bmatrix} 
\end{equation}
$$

と書くことが出来ます。

また、人工衛星などの教科書や論文に出てくる表現として、式(11)の意味の行列 $\mathbf{C}$ を $\mathbf{C}^{A'/A}$ と表現することがあるので注意しましょう。割とこの辺りの表現に躓くこともありますので表現に慣れていくとよいと思います。

$$

$$

----

さて、ここで注意したいのは、**大学数学で出てくる回転行列**とは少し考えている対象が異なります。
何が違うかというと、
- **大学数学で出てくる回転行列**は同一の座標系の（原点以外の任意の）2点の関係性を示している
- **今回導出した回転行列**は異なる座標系において、同じ点の見え方の違いを記述している

数学的には同じような操作をしていますが、考え方が異なるので混ぜると頭がこんがらがってしまいます。

自分もかなり詰まったポイントなので補足すると**大学数学で出てくる回転行列**というのは、

https://w3e.kanazawa-it.ac.jp/math/category/gyouretu/senkeidaisu/henkan-tex.cgi?target=/math/category/gyouretu/senkeidaisu/rotation_matrix_2d.html

で紹介されているような回転行列のことです。

:::message
この意味の回転行列を**表現行列**といいます。
:::

つまり、同じ座標系(直交座標系でなくてもよい)において、同一の基底ベクトルで表現できる成分（原点以外）から成分への変換を表現しています。

----

数学的に表現をしておくと（といっても局所的な表現をすると）

- 3次元のベクトル空間 $\mathbb{V}$ の基底を $\{v_1,v_2,v_3\}$としたとき、
- ベクトル空間の線形写像 $f:\mathbb{V} \rightarrow \mathbb{V}$ と
- $\mathbf{v} \in \mathbb{V}$ を用いて、
- $f(\mathbf{v})=\mathbf{A}\mathbf{v}$ を満たす $\mathbf{A}$ のことを**線型写像の表現行列**と呼び、
- **表現行列**の中でも $\|\mathbf{v}\|=\|f(\mathbf{v})\|$ かつ $\det \mathbf{A}=+1$ となるものを
- **「大学数学で出てくる回転行列」** とここでは呼んでいます。

以下の文書にこれらの違いを説明している部分があり、とても有用なので余裕があれば読んでみることをお勧めしたいと思います。（工学の世界で怠けた"数学"をしていた自分にはかなり難しかったですが）。

https://mathlandscape.com/matrix-basis-transform/

----

補足の補足ですが、私の理解が正しければ、
- $\|\mathbf{v}\|=\|f(\mathbf{v})\|$ であるものは、**直交行列**
- $\|\mathbf{v}\|=\|f(\mathbf{v})\|$かつ $\det \mathbf{A}=-1$ のものは**反射行列**

と呼ばれていると思います。

----

かなり最後ボリュームがあったように思いますが、**基底変換の文脈での回転行列**と**表現行列の文脈での回転行列**を混ぜると頭がバグりますのでここでゆっくり説明させていただきました。

この章では、異なる座標系での成分同士の関係を**回転行列（基底変換行列、DCM）**によって得られることを確認しました。
次章以降では、この回転行列がオイラー角やクォータニオンによって表現する方法について説明していきます。
しかし、その前に次章では**三次元の行列の性質**について説明します。

----