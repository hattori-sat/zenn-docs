---
title: "オイラー角"
free: false
---

- [オイラーの定理](#オイラーの定理)
  - [固有値](#固有値)
  - [オイラーの定理の理解](#オイラーの定理の理解)
- [オイラー角](#オイラー角)


# オイラーの定理
まず、オイラー角について理解するために**オイラーの定理**について説明します。

:::message
**オイラーの定理**
任意の回転は、必ずある一つの軸周りの回転で表現することが可能である。
:::

さて、私にとってこの定理は割と直感的に理解できる文章でした。
しかし、この文章を**定式化する**することはとても難しかったです。

5章では、Nature-2で**二次形式**について説明し、Define-8で **SO(3)** について説明しました。
SO(3)の特徴として、以下の二つの性質がありました。

$$
\begin{equation}
    \mathbf{A}^{-1}=\mathbf{A}^T
\end{equation}
$$

$$
\begin{equation}
    \det \mathbf{A}=1
\end{equation}
$$

また、この行列は正方行列でもあるので対角化が出来ます＝**固有値**が存在します。

## 固有値
さて、大学数学で出てくる固有値と回転についてどのような関係があるのか疑問に思った人もいると思います。
固有値というのは、$\lambda\in \mathbb{C}$ と $\mathbf{r}=[x,y,z]^T\in \mathbb{A}$ を用いて、

$$
\begin{equation}
    \mathbf{A}\mathbf{r}=\lambda \mathbf{r}
\end{equation}
$$

という関係性を示す $\lambda$ のことである(実数のみで構成される行列に対する固有値であっても複素数である可能性がある)。

式(13)の両辺で絶対値をとると

$$
\begin{equation}
    |\mathbf{A}\mathbf{r}|=|\lambda|| \mathbf{r}|
\end{equation}
$$

となり、式(2)を代入すると

$$
\begin{equation}
    |\lambda|= 1
\end{equation}
$$

という関係性を得ます。また、固有値と聞いたら**対角化**を行うというのがお決まりなので、対角化を行った結果を示すと、固有ベクトルからなる行列 $\mathbf{P}$ (この行列 $\mathbf{P}$ はユニタリ行列という複素数を含む行列)を用いて、

$$
\begin{equation}
    \mathbf{A}=\mathbf{P}\begin{bmatrix}
        \lambda_1 & 0 & 0 \\ 0 & \lambda_2 & 0 \\ 0 & 0 & \lambda _3
    \end{bmatrix}\mathbf{P}^*
\end{equation}
$$

と表すことが出来る。また、固有値の定義から固有ベクトルは0であってはいけないので

$$
\begin{equation}
    \det \begin{pmatrix}
        \mathbf{A}-\lambda \mathbf{E}
    \end{pmatrix} = 0
\end{equation}
$$

を満たす必要があります。行列式の定義から（ユニタリ行列の行列式の性質を用いて）

$$
\begin{equation}
    \det \mathbf{A}=\begin{pmatrix}
        \det\mathbf{P}
    \end{pmatrix} \begin{pmatrix}
        \det \begin{bmatrix}
        \lambda_1 & 0 & 0 \\ 0 & \lambda_2 & 0 \\ 0 & 0 & \lambda _3
    \end{bmatrix}
    \end{pmatrix}
    \begin{pmatrix}
        \det \mathbf{P}^*
    \end{pmatrix}=\det \begin{bmatrix}
        \lambda_1 & 0 & 0 \\ 0 & \lambda_2 & 0 \\ 0 & 0 & \lambda _3
    \end{bmatrix}=1
\end{equation}
$$

よって、固有値の組み合わせとしては、順不同で $(1,-1,-1)$ もしくは $(1,e^{j\theta},e^{-j\theta})$ となることが分かります（$j$ は虚数です。工学では $i$ ではなく、$j$ を使うことが多いです）

:::message
さて、つまり 回転行列には $\lambda=1$ という固有値が存在することが分かりました。
:::

## オイラーの定理の理解

いま、固有値 $\lambda=1$ のときの（単位ベクトルであり）固有ベクトルを $\mathbf{v}$ とします。現時点では、固有値1が存在することが分かっているだけであるが、この情報から

$$
\begin{equation}
    \mathbf{A}\mathbf{v}=\mathbf{v}
\end{equation}
$$

を満たすことは分かっている。

さて、Nature-4 から回転行列の積は回転行列です。任意の回転は回転行列の積で表すことが出来る（たとえば、直交座標系Aのx,y,z軸それぞれに回転させると3つの回転行列の積となる）ため、ひとつの回転行列 $\mathbf{A}$ で表すことが出来ます。

つまり、式(9)はベクトル $\mathbf{v}$ に対して任意の回転を施しても ベクトル $\mathbf{v}$ のままにできるような $\mathbf{v}$ が存在することを示しています。

ここで、単位固有ベクトル $\mathbf{v}$ の線形倍によって出来る直線 $l$ に対して垂線を落としてみます。

![](/images/understand-rotation/euler.drawio.png)

任意の位置ベクトル $\mathbf{r}\in \mathbb{A}$ から直線へと落とした垂線は、ベクトル $\mathbf{v}$ を用いて

$$
\begin{equation}
    \mathbf{h}=\mathbf{r}-(\mathbf{r}^T\mathbf{v})\mathbf{v} 
\end{equation}
$$

と表すことが出来ます。一方、位置ベクトル $\mathbf{r}$ を回転行列 $\mathbf{A}$ で回転させたあとの垂線ベクトル $\mathbf{h}'$は

$$
\begin{equation}
    \mathbf{h}'= \mathbf{A}\mathbf{r}-(\mathbf{r}^T\mathbf{A}^T\mathbf{v})\mathbf{v}
\end{equation}
$$

と表すことが出来る。任意の回転を一つの軸周りの回転で表せることを示すためには、二つの垂線ベクトルの絶対値が等しい必要があります。

式(9)を上手く使いながら絶対値を取ると

$$
\begin{aligned}
    \begin{Vmatrix}\mathbf{h}'\end{Vmatrix}&=\begin{Vmatrix}
        \mathbf{A}\mathbf{r}-(\mathbf{r}^T\mathbf{A}^T\mathbf{v})\mathbf{v}
    \end{Vmatrix} \\
    &=\begin{Vmatrix}
        \mathbf{A}\mathbf{r}-(\mathbf{v}^T\mathbf{A}\mathbf{r})\mathbf{v}
    \end{Vmatrix} \\
    &=\begin{Vmatrix}
        \mathbf{A}\mathbf{r}-\{(\mathbf{v}^T\mathbf{A}^T)\mathbf{A}\mathbf{r}\}\mathbf{v}
    \end{Vmatrix} \\
    &=\begin{Vmatrix}
        \mathbf{A}\mathbf{r}-(\mathbf{v}^T\mathbf{A}^T\mathbf{A}\mathbf{r})\mathbf{v}
    \end{Vmatrix} \\
    &=\begin{Vmatrix}
        \mathbf{A}\mathbf{r}-(\mathbf{v}^T\mathbf{r})\mathbf{v}
    \end{Vmatrix} \\
    &=\begin{Vmatrix}
        \mathbf{A}\mathbf{r}-(\mathbf{v}^T\mathbf{r})\mathbf{A}\mathbf{v}
    \end{Vmatrix} \\
    &=\begin{Vmatrix}
        \mathbf{A}\{\mathbf{r}-(\mathbf{v}^T\mathbf{r})\mathbf{v}\}
    \end{Vmatrix} 
\end{aligned}
$$

となり、

$$
\begin{equation}
    \begin{Vmatrix}\mathbf{A}\mathbf{r}\end{Vmatrix}^2=\mathbf{r}^T\mathbf{A}^T \mathbf{A}\mathbf{r}=\mathbf{r}^T\mathbf{r}=\begin{Vmatrix}\mathbf{r}\end{Vmatrix}^2
\end{equation}
$$

が成り立つため、

$$
\begin{equation}
    \begin{Vmatrix}
        \mathbf{h}'
    \end{Vmatrix}=\begin{Vmatrix}
        \mathbf{A}\{\mathbf{r}-(\mathbf{v}^T\mathbf{r})\mathbf{v}\}
    \end{Vmatrix} =\begin{Vmatrix}
        \{\mathbf{r}-(\mathbf{v}^T\mathbf{r})\mathbf{v}\}
    \end{Vmatrix} =\begin{Vmatrix}
        \mathbf{h}
    \end{Vmatrix}
\end{equation}
$$

が導けます。

:::message
よって、任意の位置ベクトル $\mathbf{r}$ に対して任意の回転 $\mathbf{A}$ を作用させたときに、ある軸 $\mathbf{v}$周りの回転であると示すことが出来ました（どれだけ回転させても直線 $l$ に対して降ろす垂線の大きさは変化しない）。
:::

----

# オイラー角
ここからは、オイラー角の説明をします。
正直オイラーの定理について理解する方が難しい気もします。

さて、まずオイラー角の気持ちを説明すると
- 直交基底座標系の軸周りに3回回転させれば回転を表せそう

ということである。一番簡単な例で言うと、静止座標系において、x軸周り、y軸周り、z軸周りに順次回転させればどんな回転でも表現できそうということである。
（3章で説明したようなz軸周りの回転をx軸とy軸も計算すれば回転は表現できる）

その前に、なぜ3回回転させればいいのか気になると思います。
どういうことかというと、3章の方向余弦行列では**9つの内積**を計算する必要があったのに「3回の回転で済む＝自由度が3」になっていることが不思議に感じるということです。

これは回転行列の定義より、$\mathbf{A}=\begin{bmatrix}\mathbf{a}_1&\mathbf{a}_2&\mathbf{a}_3\end{bmatrix}$ において、

$$
\begin{equation}
    \mathbf{a}_1\cdot \mathbf{a}_1 = \mathbf{a}_2\cdot \mathbf{a}_2 = \mathbf{a}_3\cdot \mathbf{a}_3 =1 
\end{equation}
$$

$$
\begin{equation}
    \mathbf{a}_1\cdot \mathbf{a}_2 = \mathbf{a}_2\cdot \mathbf{a}_3 = \mathbf{a}_3\cdot \mathbf{a}_1 =0 
\end{equation}
$$

という条件があり、6つの拘束条件があります。そして、方向余弦行列では9つの自由度があったため、$9-6=3$ で3自由度で良くなるという寸法です。

さて、ではオイラー角を示そうと思いますが、実は考え方が2つあります。

:::message
オイラー角を定義させるときに以下の2つの考え方があります。
1. 静止された座標系のx,y,z軸周りに回転させる
2. まず、x軸周りに回して、その後回転した座標系からy軸周りに回転させて、その後回転した座標系からz軸周りに回転させる（つまり、動く物体に固定された座標系周りに回転させる）
:::

本書では、2つ目の考え方でのオイラー角の説明を行います。
なぜならば、2詰めの考え方がドローンや人工衛星に固定の座標系を考えたときにマッチした考え方であり、（ジャイロセンサなどの）センサ情報で得られるのは物体に固定された座標系の情報なので、実際にオイラー角を使うときには物体固定の座標系を使う機会がほとんどだからです。

3章の式(2)から式(6)で $z$ 軸周りの回転を示すオイラー角を求めています。
$x$軸周りの回転は $\phi$、$y$軸周りの回転は $\theta$、$z$軸周りの回転は $\psi$ と表すことが多いです。
それぞれオイラー角を示すと

■ $x$軸周り

$$
\begin{equation}
    \mathbf{C}_1(\phi) = \begin{bmatrix} 1 & 0 & 0 \\ 0 & \cos{\phi} & sin{\phi} \\ 0 & -\sin{\phi} & \cos{\phi}  \end{bmatrix}
\end{equation}
$$

■ $y$軸周り

$$
\begin{equation}
    \mathbf{C}_2(\theta) = \begin{bmatrix} \cos{\theta} & 0 & -\sin{\theta}  \\ 0 & 1 & 0 \\ \sin{\theta} & 0 & \cos{\theta}  \end{bmatrix}
\end{equation}
$$

■ $z$軸周り

$$
\begin{equation}
    \mathbf{C}_3(\psi) = \begin{bmatrix} \cos{\psi} & \sin{\psi} & 0 \\ -\sin{\psi} & \cos{\psi} & 0 \\ 0 & 0 & 1  \end{bmatrix}
\end{equation}
$$

----