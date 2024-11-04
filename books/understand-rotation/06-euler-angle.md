---
title: "オイラー角"
free: false
---

- [オイラーの定理](#オイラーの定理)
  - [固有値](#固有値)
  - [オイラーの定理の理解](#オイラーの定理の理解)
- [オイラー角](#オイラー角)
  - [オイラー角について説明](#オイラー角について説明)
  - [座標変換してみる](#座標変換してみる)
- [運動方程式で違いを実感する](#運動方程式で違いを実感する)
- [オイラー角を実感してみよう](#オイラー角を実感してみよう)
  - [基底変換行列（回転行列）のオイラー角](#基底変換行列回転行列のオイラー角)
  - [回転変換の表現行列のオイラー角](#回転変換の表現行列のオイラー角)


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
## オイラー角について説明
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
ちなみに、3自由度という制限しかないので、
- x軸→y軸→x軸周りの回転でもよい（3変数定義することになるので方程式の解は定まる）
- x軸→x軸→x軸周りの回転は単にx軸周りの回転に集約されてしまうので1自由度の固定としかならない

さて、ではオイラー角を示そうと思いますが、実は考え方が2つあります。

:::message
オイラー角を定義させるときに以下の2つの考え方があります。
1. 静止された座標系のx,y,z軸周りに回転させる
2. まず、x軸周りに回して、その後回転した座標系からy軸周りに回転させて、その後回転した座標系からz軸周りに回転させる（つまり、動く物体に固定された座標系周りに回転させる）
:::

本書では、2つ目の考え方でのオイラー角の説明を行います。
なぜならば、2つ目の考え方がドローンや人工衛星に固定の座標系を考えたときにマッチした考え方であり、（ジャイロセンサなどの）センサ情報で得られるのは物体に固定された座標系の情報なので、実際にオイラー角を使うときには物体固定の座標系を使う機会がほとんどだからです。

3章の式(2)から式(6)で $z$ 軸周りの回転を示すオイラー角を求めています。
$x$軸周りの回転は $\phi$、$y$軸周りの回転は $\theta$、$z$軸周りの回転は $\psi$ と表すことが多いです。
それぞれの軸周りの回転行列を示すと

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

となります。さて、このときの回転行列というのは参照で紹介した以下の式

$$
\begin{equation}
\begin{bmatrix} \mathbf{e'}_1 & \mathbf{e'}_2 & \mathbf{e'}_3  \end{bmatrix} = \begin{bmatrix} \mathbf{e}_1 & \mathbf{e}_2 & \mathbf{e}_3  \end{bmatrix}\mathbf{C}^T
\end{equation}
$$

の行列 $\mathbf{C}$ のことです（式(16)、式(17)、式(18) をそのまま式(19)に代入すれば問題ないです）。

さて、このときの角度 $\phi, \theta,\psi$ のことを**オイラー角**と呼びます。

:::details どうでもいい補足
オイラー角自体は位置ベクトルのように、$\mathbf{r}=\begin{bmatrix}x&y&z\end{bmatrix}^T$ のように角度ベクトルのようなものではないことに注意しましょう。

他方で、角速度ベクトルは大きな意味があります。
これは角速度ベクトルを定義することから始める必要がありますが、角速度ベクトルの概念が変わりますのでぜひ調べてみていただきたいです。 
:::

----

## 座標変換してみる
Fig.6-1 のように固定された座標系（**物体座標系**と呼びます）周りでの回転を考えてみましょう。
最初は静止座標系 A （ユークリッド空間で正規直行基底を採る）と同じ基底を採っていたものとして、物体座標系のx軸周りに回転させたときの静止座標系から見た座標系を A' として、その座標系A'のy軸周りに回転させた座標系をA"とし、その座標系A" からz軸周りに回転させた座標系をBとします。

（ややこしくなりますが、A'やA"、Bの座標系は非慣性座標系となり遠心力やらコリオリ力などの見かけの力を体感可能です。実際の外力ではなく座標系自体が動いていることによって力が掛かっているように見えるだけですが。）


![](/images/understand-rotation/euler-angle.drawio.png)
*Fig.6-1 物体固定座標系*

さて、数式で表現してみましょう。座標系が回転しているということですので式(19)をそのまま使えるわけです（3章で同じ手続きで導出したためです）。

それぞれの基底は座標系の小文字を使うことにします。(何度もしつこいですが、ここでのダッシュやダッシュ2個は微分を示しているわけではありません。)

■ 固定座標系のx軸周りでの回転（最初は静止座標系 A）：A→A'

$$
\begin{equation}
\begin{bmatrix} \mathbf{a}'_1 & \mathbf{a}'_2 & \mathbf{a}'_3  \end{bmatrix} = \begin{bmatrix} \mathbf{a}_1 & \mathbf{a}_2 & \mathbf{a}_3  \end{bmatrix}\mathbf{C}_1^T(\phi)
\end{equation}
$$

■ 固定座標系のy軸周りでの回転：A'→A"

$$
\begin{equation}
\begin{bmatrix} \mathbf{a}''_1 & \mathbf{a}''_2 & \mathbf{a}''_3  \end{bmatrix} = \begin{bmatrix} \mathbf{a'}_1 & \mathbf{a'}_2 & \mathbf{a'}_3  \end{bmatrix}\mathbf{C}_2^T(\theta)
\end{equation}
$$

■ 固定座標系のz軸周りでの回転：A"→B

$$
\begin{equation}
 \begin{bmatrix} \mathbf{b}_1 & \mathbf{b}_2 & \mathbf{b}_3  \end{bmatrix}=\begin{bmatrix} \mathbf{a}''_1 & \mathbf{a}''_2 & \mathbf{a}''_3  \end{bmatrix}\mathbf{C}_3^T(\theta) 
\end{equation}
$$

ここで私たちが知りたいのは、回転する前と回転した後の関係だけです。それは式(20)から式(22)までを使えば求めることが出来ます。代入をしていくと

$$
\begin{align}
        \begin{bmatrix} \mathbf{b}_1 & \mathbf{b}_2 & \mathbf{b}_3  \end{bmatrix}
        &=\begin{bmatrix} \mathbf{a}''_1 & \mathbf{a}''_2 & \mathbf{a}''_3  \end{bmatrix}\mathbf{C}_3^T(\psi) \\
        &=\begin{bmatrix} \mathbf{a}'_1 & \mathbf{a}'_2 & \mathbf{a}'_3  \end{bmatrix}\mathbf{C}_2^T(\theta)\mathbf{C}_3^T(\psi) \\
        &=\begin{bmatrix} \mathbf{a}_1 & \mathbf{a}_2 & \mathbf{a}_3  \end{bmatrix}\mathbf{C}_1^T(\phi)\mathbf{C}_2^T(\theta)\mathbf{C}_3^T(\psi) 
\end{align}
$$

という関係を得ることが出来ました。慣習的に

$$
\begin{equation}
    {\mathbf{C}^{B/A}}^T=\mathbf{C}_1^T(\phi)\mathbf{C}_2^T(\theta)\mathbf{C}_3^T(\psi) 
\end{equation}
$$
$$
\begin{equation}
    {\mathbf{C}^{B/A}}\equiv\mathbf{C}_3(\psi)\mathbf{C}_2(\theta) \mathbf{C}_1(\phi)
\end{equation}
$$

と書かれ、座標系Aから座標系Bへの回転行列と呼ばれます。つまり、各座標系における成分同士の関係は、

$$
\begin{equation}
    \begin{bmatrix}
        x_b \\ y_b \\ z_b
    \end{bmatrix} = \mathbf{C}^{B/A} \begin{bmatrix}
        x_a \\ y_a \\ z_a
    \end{bmatrix}
\end{equation}
$$

と求めることが出来ます。

こちらも何度も言いますが、これは固定座標系周りの回転を表現したものです。
ロボットや人工衛星などで取得できる情報から、静止座標系からの回転を知りたいために使うような用途のものです。
（ちなみに式(25) は行列の演算からすると逆順のような感じがしてかなり気持ち悪いです。）

----

この辺り、ちゃんと解説されていることが少ないので説明をすると、オイラー角の考え方1というのは、「**同じ座標系において回転を考える場合**」です。
つまり、オイラー角を**回転変換の表現行列**として使うということです。

![](/images/understand-rotation/seishi-rotation.drawio.png)
*Fig.6-2 考え方1の回転*

Fig.6-2 が原点からの距離が1で回転した時のことを考えると

$$
\begin{equation}
    x_1= \cos{\alpha},\quad y_1=\sin{\alpha},\quad z_1=0
\end{equation}
$$

$$
\begin{align}
    x'_1&= \cos{(\alpha+\psi)}=\cos{\alpha}\cos{\psi} - \sin{\alpha}\sin{\psi}=(\cos{\psi})x_1-(\sin{\psi})y_1\\
    y'_1&=\sin{(\alpha+\psi)}= \cos{\alpha}\sin{\psi} + \sin{\alpha}\cos{\psi}=(\sin{\psi})x_1+(\cos{\psi})y_1\\
    z'_1&=0
\end{align}
$$

よって、

$$
\begin{equation}
    \begin{bmatrix}
        x'_1 \\ x'_2 \\ x'_3
    \end{bmatrix} =  \begin{bmatrix} \cos{\psi} & -\sin{\psi} & 0 \\ \sin{\psi} & \cos{\psi} & 0 \\ 0 & 0 & 1  \end{bmatrix}\begin{bmatrix}
        x_1 \\ x_2 \\ x_3
    \end{bmatrix}
\end{equation}
$$

となるわけです。（z軸周りの回転を表す回転行列であるはずなのに、行列の1行目2列の値と2行目1列の値が逆になっています。この辺りの正負は毎回チェックして考えるようにしましょう。）

:::message
以上のことから「基底を回転させ、その基底どうしの回転の関係を示す方向余弦行列（回転行列）」と「同じ座標系で何かを回転させるという回転変換の表現行列」は同じ性質を持つ回転行列（SO(3)）ではありますが、考え方が全く違うものとなります。
:::

----

# 運動方程式で違いを実感する
さて、ここまで説明をした結果、「違いをすっきり理解できたがだからなんだ？」という人や「よくまだすっきりしない」という人もいるでしょう。
これは**運動方程式を立ててみることで違いが理解がしやすい**です。

ニュートンの運動方程式

$$
\begin{equation}
m \ddot{\mathbf{r}}=\mathbf{F}    
\end{equation}

$$

は**ガリレイ変換**および**回転変換**において**ベクトル表現であれば形式は変化しません**。
(これを**一般共変性原理**といいます。相対性理論などで出てきます。)

まずは、物体固定座標系における運動方程式を考えましょう。
共変性により、運動方程式は回転座標系であっても式(34)で書くことが出来ます。しかし、静止座標系で表現したいときは

$$
\begin{equation}
    m\begin{bmatrix}
        \ddot{x_b} \\ \ddot{y_b} \\ \ddot{z_b}
    \end{bmatrix}=\mathbf{F}_b    
\end{equation}
$$

とはなりません。なぜならば、ベクトルの基底を考える必要があるからです。静止座標系の基底の微分は0となることに注意して以下の変換を見てみましょう。
($\mathbf{R}_a=\begin{bmatrix} \mathbf{a}_1 & \mathbf{a}_2 & \mathbf{a}_3  \end{bmatrix}$ としたら $\dot{\mathbf{R}}_a=\mathbf{0}$ )

$$
\begin{align}
    \mathbf{r}&=\begin{bmatrix} \mathbf{b}_1 & \mathbf{b}_2 & \mathbf{b}_3  \end{bmatrix}\begin{bmatrix}x_b \\ y_b \\ z_b\end{bmatrix}= \begin{bmatrix} \mathbf{a}_1 & \mathbf{a}_2 & \mathbf{a}_3  \end{bmatrix} {\mathbf{C}^{B/A}}^T \begin{bmatrix}x_b \\ y_b \\ z_b\end{bmatrix}=\mathbf{R}_a{\mathbf{C}^{B/A}}^T \begin{bmatrix}x_b \\ y_b \\ z_b\end{bmatrix}\\

    \dot{\mathbf{r}}&=\dot{\mathbf{R}}_a{\mathbf{C}^{B/A}}^T \begin{bmatrix}x_b \\ y_b \\ z_b\end{bmatrix}+\mathbf{R}_a{\dot{\mathbf{C}}^{B/A}}^T \begin{bmatrix}x_b \\ y_b \\ z_b\end{bmatrix}+\mathbf{R}_a{\mathbf{C}^{B/A}}^T \begin{bmatrix}\dot{x_b} \\ \dot{y_b} \\ \dot{z_b}\end{bmatrix}\\

    \ddot{\mathbf{r}}&=\mathbf{R}_a{\ddot{\mathbf{C}}^{B/A}}^T \begin{bmatrix}x_b \\ y_b \\ z_b\end{bmatrix}+2\mathbf{R}_a{\dot{\mathbf{C}}^{B/A}}^T \begin{bmatrix}\dot{x_b} \\ \dot{y_b} \\ \dot{z_b}\end{bmatrix}+\mathbf{R}_a{\mathbf{C}^{B/A}}^T \begin{bmatrix}\ddot{x_b} \\ \ddot{y_b} \\ \ddot{z_b}\end{bmatrix}
\end{align}
$$

よって、運動方程式は静止座標系において、

$$
\begin{equation}
    m\begin{Bmatrix}
        {\ddot{\mathbf{C}}^{B/A}}^T \begin{bmatrix}x_b \\ y_b \\ z_b\end{bmatrix}+2{\dot{\mathbf{C}}^{B/A}}^T \begin{bmatrix}\dot{x_b} \\ \dot{y_b} \\ \dot{z_b}\end{bmatrix}+{\mathbf{C}^{B/A}}^T \begin{bmatrix}\ddot{x_b} \\ \ddot{y_b} \\ \ddot{z_b}\end{bmatrix}
    \end{Bmatrix}=\mathbf{F}_b
\end{equation}
$$

となるので、

$$
\begin{equation}
    m\begin{bmatrix}\ddot{x_b} \\ \ddot{y_b} \\ \ddot{z_b}\end{bmatrix}=\mathbf{F}_b-m{\mathbf{C}^{B/A}}{\ddot{\mathbf{C}}^{B/A}}^T \begin{bmatrix}x_b \\ y_b \\ z_b\end{bmatrix}-2{\mathbf{C}^{B/A}}{\dot{\mathbf{C}}^{B/A}}^T \begin{bmatrix}\dot{x_b} \\ \dot{y_b} \\ \dot{z_b}\end{bmatrix} 
\end{equation}
$$

と表現できます。式(40) の右辺第二項は遠心力、右辺第三項はコリオリ力と呼ばれています。

一方で、Fig.6-2に示すような**回転変換の表現行列**の場合は、静止座標系において、物体が運動しているだけなので、

$$
\begin{equation}
    m\begin{bmatrix}
        \ddot{x_a} \\ \ddot{y_a} \\ \ddot{z_a}
    \end{bmatrix}=\mathbf{F}_a    
\end{equation}
$$

で表現できます。

このように同じ回転行列（オイラー角）であっても、運動方程式を立てたときに違いを感じると思います。

:::message
式(40)は物体に固定された座標系で得られる値（加速度や速度、角速度など）であるので、この式を用いることで表現できることはとても重要である。
:::

----

# オイラー角を実感してみよう
## 基底変換行列（回転行列）のオイラー角
pythonスクリプトを記載。


## 回転変換の表現行列のオイラー角
pythonスクリプトを記載。

----