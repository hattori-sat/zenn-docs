---
title: "クォータニオン（四元数）"
free: false
---

# 四元数の導入

**クォータニオン**というのは**四元数（しげんすう）**とも呼ばれます。
私はクォータニオンについて数学的に議論することは難しいため、ある程度の紹介と使い方の説明をするに留めたいと思います。

::: message
クォータニオンを使うことで三次元の回転を表せるというだけで、クォータニオン自体は複素数を拡張した数のことです。
:::

まずは**複素数を拡張**するところから説明します。

複素数は、$i^2=-1$ となる $i$ のことです。オイラーの公式や複素平面など物理において重要な要素です。

この概念を拡張して、数 $i,j,k$ が 

$$
\begin{align}
    i^2&=j^2=k^2=-1 \\
    ij&=jk=ki \\
    ij&=-ji \\
    ij=k
\end{align}
$$

という性質を満たすとき、$a,b,c,d\in \mathbb{R}$ を用いて

$$
\begin{equation}
    q=a+ib+jc+kd
\end{equation}
$$

としたときの $q$ を**四元数**と呼びます。

----

性質として、以下の3つがあります。
- ノルム
- 共役
- 逆元

おそらく以下を見ていけば、納得できる内容だと思います。

■ ノルム

$$
\begin{equation}
    \begin{Vmatrix}
        q 
    \end{Vmatrix}=\sqrt{a^2+b^2+c^2+d^2}
\end{equation}
$$

■ 共役な四元数

$$
\begin{equation}
    \bar{q}=a-ib-jc-kd
\end{equation}
$$

を共役な四元数とする。

:::details 共役

$$
\begin{aligned}
    q\bar{q}
    &=(a+ib+jc+kd)(a-ib-jc-kd)\\
    &=a^2-iab-jac-kad+iab-i^2b^2-ijbc-ikbd \\
    &+jac-jibc-j^2c^2-jkcd-kad-kibd-kjcd-k^2d^2 \\
    &=a^2-iab-jac-kad+iab+b^2-kbc+jbd \\
    &+jac+kbc+c^2-icd-kad-jbd+icd+d^2 \\
    &=a^2+b^2+c^2+d^2 +i(-ab+ab-cd+cd) \\
    &+j(-ac+bd+ac-bd)+k(-ad-bc+bc-ad) \\
    &=a^2+b^2+c^2+d^2 
\end{aligned}
$$
:::

$$
\begin{equation}
    q\bar{q}=\bar{q}q=a^2+b^2+c^2+d^2 
\end{equation}
$$


■逆元

$$
\begin{equation}
    q^{-1}q=qq^{-1}=1
\end{equation}
$$

となる $q^{-1}$ が存在する。ノルムと共役な四元数から

$$
\begin{equation}
    q^{-1}= \frac{\bar{q}}{\begin{Vmatrix}
        q
    \end{Vmatrix}}
\end{equation}
$$

となります。

----

# 3次元空間の表現
ここで説明することは数学的には少しおかしい話をします。
クォータニオンの性質と回転行列について**アナロジ（類推）**を行うことで、**クォータニオンによる回転行列の表現**について考えていきます。

さて、では三次元の空間を四元数で表現してみます。
ある物体が三次元の点 $(x,y,z)$ にあるとします（これまでと同様に正規直行基底を採ることにします）。
（一旦位置ベクトルとは全く関係なく） $x,y,z\in \mathbb{R}$ を用いて、

$$
\begin{equation}
    p=xi+yj+zk
\end{equation}
$$

と表せるとします。

$q$ を $|q|=1$ となる四元数とすれば、$p=xi+yj+zk$ に対して写像

$$
\begin{equation}
    f(p)=qp\bar{q}
\end{equation}
$$f(p)

は 

$$
\overline{f(q)}=\overline{qp\bar{q}}=\bar{\bar{q}}\bar{q}\bar{q}=-(qp\bar{q})=-f(q)
$$

となり、$\overline{f(q)}$ と $-f(q)$ は同じです。この関係をもつとき、$f(q)$ は実部（$i,j,k$がない項を持たないということです。
また、このノルムをとってみると

$$
\begin{equation}
    \begin{Vmatrix}f(q)\end{Vmatrix}=\begin{Vmatrix}q\end{Vmatrix}\begin{Vmatrix}p\end{Vmatrix}\begin{Vmatrix}\bar{q}\end{Vmatrix}=\begin{Vmatrix}p\end{Vmatrix}
\end{equation}
$$

という関係が成り立ちます。いま、写像 $f$ のよって、成分 $(x,y,z)$ は新しい成分 $(x',y',z')$ に移動しましたが、原点からの距離は変化せずに一定であり、つまり任意の回転を示していると考えることが出来ます（SO(3)はオイラーの定理で説明した通り、ノルムを変化させません。つまり、距離が変わりません。ここまで見てきた皆さんからすると、ノルムが変化しないのは直交行列の性質であって、SO(3)であるかは分からないだろうと思うかもしれません）。



:::details SO(3)の再掲
■ Define-8: 特殊直交群 SO(3)
さて、回転行列（Define-2とDefine-3）に深く関わる集合を定義します。本書で使っている $\mathbf{A}$ のうち

$$
\begin{equation}
    \mathrm{SO}(3)=\begin{Bmatrix}
        \mathbf{A} | \det\mathbf{A}=+1, \quad\mathbf{A}^T\mathbf{A}=\mathbf{E}
    \end{Bmatrix}
\end{equation}
$$

である集合のことをいいます。
（なお、$|\det\mathbf{A}|=1$ は直交行列と呼ばれることに注意する。すべてのSO(3)は直交行列であるが、すべての直交行列がSO(3)であるわけではないです。）
:::

このとき、前章で示した**オイラーの定理**から任意の回転はある軸周りの回転で表現できます。
よって、ある軸