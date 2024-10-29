---
title: "基底ベクトルを復習"
free: true
---

まずベクトルの復習から始めたいと思います。基底ベクトルについての説明です。

下図Fig.2-1の質点aの位置ベクトルを考えます。

![](/images/understand-rotation/basis-vector.drawio.png)
*Fig.2-1 慣性座標系における位置ベクトル* 

質点aの位置ベクトル $\mathbf{r}_a$は以下のように書けます。

$$ 
\begin{equation}
   \mathbf{r} = \begin{bmatrix} x_a \\ y_a \\ z_a \end{bmatrix} 
\end{equation}
$$

おそらく見慣れた式だと思います。念のため補足しておくと、ボールドフォント（太字の立体）の小文字で書かれているのがベクトルです。列ベクトルで表現します。そして、今回の場合は列ベクトルは3行1列の**行列**です。

----

さて、この章の本題です。式(1)は、**省略された表記**であることは覚えているでしょうか。
Fig.2-2のように単位ベクトル $\mathbf{e}_1$、$\mathbf{e}_2$、$\mathbf{e}_3$ を用意して各単位ベクトルが直交するように配置したとします（**正規直行基底**）。
このように定義すれば、慣性座標系Aは直交座標系とみなすことが出来ます（ちなみに、慣性座標系は慣性の法則が成り立つ座標系で、座標系自体が静止 or 等速運動しています）。

![](/images/understand-rotation/unit-vector.drawio.png)
*Fig.2-2 単位基底ベクトル*

さて、単位ベクトルを用いて質点aの位置ベクトルは

$$ 
\begin{equation}
   \mathbf{r} = x_a\mathbf{e}_1 + y_a\mathbf{e}_2 + z_a\mathbf{e}_3
\end{equation}
$$

と書くことが出来ます。

式(2)は行列を用いて表現をすると

$$
\begin{equation}
\mathbf{r} = \begin{bmatrix} \mathbf{e}_1&\mathbf{e}_2&\mathbf{e}_3 \end{bmatrix} \begin{bmatrix} x_a \\ y_a \\ z_a  \end{bmatrix} 
\end{equation}
$$

と書くことが出来ます。このとき、3行3列の実行列 $\mathbf{R}$を用いて(行列は大文字のボールドを使うことが多い)

$$
\begin{equation}
\mathbf{R}= \begin{bmatrix} \mathbf{e}_1&\mathbf{e}_2&\mathbf{e}_3 \end{bmatrix} 
\end{equation}
$$

と表現しておく（のちのち便利なので）。このとき、各単位ベクトルを

$$
\begin{equation}
\mathbf{e}_1 = \begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix} , \quad \mathbf{e}_2 =\begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix}, \quad \mathbf{e}_3 =\begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix}
\end{equation}
$$

となるように**上手く定義**すると式(3)は

$$
\begin{equation}
\mathbf{r} = \begin{bmatrix} 1&0&0\\0&1&0\\0&0&1 \end{bmatrix} \begin{bmatrix} x_a \\ y_a \\ z_a  \end{bmatrix} = \begin{bmatrix} x_a \\ y_a \\ z_a  \end{bmatrix}
\end{equation}
$$

と書けるため、結局式(1)と同じになりました。ちなみに式(1)の表記は、**成分表示**ともいわれています。

----

:::message
しかし、回転を考えると式(5)のように上手く定義出来ない場合も多く存在します。そのため、今後は**式(3)の表現を用いてベクトルを表現する**ことにします。

今後複数の座標系を定義することがあります。そうすると同じ質点の位置ベクトルを表現仕様としたとき、座標系ごとに成分が異なることになります。この時に役立ちますが、次の章を見れば理解できると思います。

:::

ちなみに、式(4)の基底をまとめた行列は**正則行列**となります。なぜならば、座標系が正規直行するように定義したためです。
今後は**座標系の定義にも気を付けていきましょう**。