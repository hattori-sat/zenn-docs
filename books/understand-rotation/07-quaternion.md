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
(この性質が成り立つことは示していませんが、簡単に示せます)

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

<!-- :::details 式の導出 -->

$$
\begin{aligned}
    f(q)&=qp\bar{q} \\
    &=(a+ib+jc+kd)(ix+jy+kz)(a-ib-jc-kd)\\
    &=(iax+jay+kaz-bx+ijby+ikbz+jicx\\
    &\quad-cy+jkcz+kidx+kjdy-dz)(a-ib-jc-kd)\\
    &=( iax+jay+kaz-bx+kby-jbz-kcx-cy\\
    &\quad+icz+jdx-idy-dz)(a-ib-jc-kd)\\
    &=\{-bx-cy-dz+i(ax-dy+cz)+j(ay-bz+dx)\\
    &\quad+k(az+by-cx) \}(a-ib-jc-kd)\\
    &=a(-bx-cy-dz)-ib(-bx-cy-dz)\\
    &\quad-jc(-bx-cy-dz)-kd(-bx-cy-dz)\\
    &\quad+ai(ax-dy+cz)-i^2b(ax-dy+cz)\\
    &\quad-ijc(ax-dy+cz)-ikd(ax-dy+cz)\\
    &\quad+ja(ay-bz+dx)-jib(ay-bz+dx)\\
    &\quad-j^2c(ay-bz+dx)-jkd(ay-bz+dx)\\
    &\quad+ka(az+by-cx)-kib(az+by-cx)\\
    &\quad-kjc(az+by-cx)-k^2d(az+by-cx)\\
    &=-abx-acy-adz+ib^2x+ibcy+ibdz+jbcx+jc^2y+jcdz \\
    &\quad+kbdx+kcdy+kd^2z+ia^2x-iady+iacz+abx-bdy+cbz \\
    &\quad-kcax+kcdy-kc^2z+jadx-jd^2y+jcdz+ja^2y-jabz+jadx\\
    &\quad+kaby-kb^2z+kbdx+acy-bcz+cdx-iady+ibdz-id^2x \\
    &\quad+ka^2z+kaby-kacx-jabz-jb^2y+jbcx+iacz+ibcy-ic^2x \\
    &\quad+adz+bdy-cdx\\
    &=i(b^2x+bcy+bdz+a^2x-ady+acz-ady+bdz-d^2x+acz+bcy-c^2x)\\
    &\quad+j(bcx+c^2y+cdz+adx-d^2y+cdz+a^2y-abz+adx-abz-b^2y+bcx)\\
    &\quad+k(bdx+cdy+d^2z-cax+cdy-c^2z+aby-b^2z+bdx+a^2z+aby-acx)\\
    &\quad-abx-acy-adz+abx-bdy+cbz+acy-bcz+cdx+adz+bdy-cdx\\
    &=i\{ (b^2+a^2-d^2-c^2)x + (bc-ad-ad+bc)y+(bd+ac+bd+ac)z\}\\
    &\quad+j\{ (bc+ad+ad+bc)x + (c^2-xd^2+a^2-b^2)y +(cd+cd-ab-ab)z \} \\
    &\quad+k\{ (bd-ca+bd-ac)x + (cd+cd+ab+ab)y + (d^2-c^2-b^2+a^2)z\} \\
    &\quad+0

\end{aligned}
$$


さて、ここで四元数とベクトルを同一視してみます。
$q=ix+jy+kz$ は $\mathbf{q}=\begin{bmatrix}x & y & z\end{bmatrix}^T$ と同じと考えられそうなので、こう考えてみると先ほど求めた $f(q)$ は

$$
\begin{equation}
    f(p)=qp\bar{q}=\begin{bmatrix}
        a^2 + b^2 - c^2 - d^2 & 2(bc - ad) & 2(bd + ac) \\
        2(bc + ad) & a^2 - b^2 + c^2 - d^2 & 2(cd - ab) \\
        2(bd - ac) & 2(cd + ab)  &a^2 - b^2 - c^2 + d^2
    \end{bmatrix}\begin{bmatrix}
        x \\ y \\z
    \end{bmatrix}
\end{equation}
$$

と表せます。ここで、式(14)の右辺の行列が回転行列 SO(3) であればクォータニオンと回転行列を同一視出来ることが納得できると思います。
一旦、式(14) の行列を $\mathbf{A}$ として

$$
\begin{equation}
    \mathbf{A}=\begin{bmatrix}
        a^2 + b^2 - c^2 - d^2 & 2(bc - ad) & 2(bd + ac) \\
        2(bc + ad) & a^2 - b^2 + c^2 - d^2 & 2(cd - ab) \\
        2(bd - ac) & 2(cd + ab)  &a^2 - b^2 - c^2 + d^2
    \end{bmatrix}
\end{equation}
$$

とします。

$|\mathbf{q}|=|f(\mathbf{q})|$ は示せているので、$\det \mathbf{A}=+1$ を示しせば、SO(3) であることが言えます。

$$
\begin{aligned}
    \det \mathbf{A} &= (a^2 + b^2 - c^2 - d^2)(a^2 - b^2 + c^2 - d^2)(a^2 - b^2 - c^2 + d^2) \\
    &\quad+8(bc - ad)(cd - ab)(bd - ac)+8(bc + ad)(cd + ab)(bd + ac) \\
    &\quad-4(a^2 + b^2 - c^2 - d^2)(cd - ab)(cd + ab) \\
    &\quad-4(a^2 - b^2 + c^2 - d^2)(bd + ac)(bd - ac) \\
    &\quad-4(a^2 - b^2 - c^2 + d^2)(bc - ad)(bc + ad)
\end{aligned}
$$

さて、$a^2+b^2+c^2+d^2=1$ であることを利用して、 それぞれを計算する。

右辺の積の最左の項は

$$
\begin{aligned}
    &(a^2 + b^2 - c^2 - d^2)(a^2 - b^2 + c^2 - d^2)(a^2 - b^2 - c^2 + d^2)\\
    &=(1-2c^2-2d^2)(1-2b^2-2d^2)(1-2b^2-2c^2)\\
    &=1-2b^2-2c^2-2b^2+4b^4+4b^2c^2-2d^2+4b^2d^2+4c^2d^2-2c^2+4b^2c^2 \\
    &\quad +4c^4+4b^2c^2-8b^4c^2-8b^2c^4+4c^2d^2-8b^2c^2d^2-8c^4d^2-2d^2\\
    &\quad +4b^2d^2+4c^2d^2+4b^2d^2-8b^4d^2-8b^2c^2d^2+4d^4-8b^2d^4-8c^2d^4
\end{aligned}
$$

となり、2項目は、

$$
\begin{aligned}
    &8(bc - ad)(cd - ab)(bd - ac)+8(bc + ad)(cd + ab)(bd + ac) \\
    &=8(2b^2c^2d^2+2a^2b^2d^2+2a^2b^2c^2+2a^2c^2d^2)
\end{aligned}
$$

となる。-4の係数の項ものを纏めて計算すると

$$
\begin{aligned}
    &-4(c^2d^2-a^2b^2-2c^4d^2+2a^2b^2c^2-2c^2d^4+2a^2b^2d^2+b^2d^2-a^2c^2-2b^4d^2\\
    &+2a^2b^2c^2-2b^2d^4+2a^2c^2d^2+b^2c^2-a^2d^2-2b^4c^2+2a^2b^2d^2-2b^2c^4+2a^2b^2)
\end{aligned}

$$

となる、よってまとめて計算すると

$$
\begin{aligned}
    \det \mathbf{A} 
    &=1-2b^2-2c^2-2b^2+4b^4+4b^2c^2-2d^2+4b^2d^2+4c^2d^2-2c^2+4b^2c^2 \\
    &\quad +4c^4+4b^2c^2-8b^4c^2-8b^2c^4+4c^2d^2-8b^2c^2d^2-8c^4d^2-2d^2\\
    &\quad +4b^2d^2+4c^2d^2+4b^2d^2-8b^4d^2-8b^2c^2d^2+4d^4-8b^2d^4-8c^2d^4\\
    &\quad +8(2b^2c^2d^2+2a^2b^2d^2+2a^2b^2c^2+2a^2c^2d^2)\\
    &\quad -4(c^2d^2-a^2b^2-2c^4d^2+2a^2b^2c^2-2c^2d^4+2a^2b^2d^2+b^2d^2-a^2c^2\\
    &\quad -2b^4d^2+2a^2b^2c^2-2b^2d^4+2a^2c^2d^2+b^2c^2-a^2d^2-2b^4c^2\\
    &\quad +2a^2b^2d^2-2b^2c^4+2a^2b^2)\\
    &=1-2c^2+4b^4+4b^2c^2-2d^2+4b^2d^2+4c^2d^2-2c^2+4b^2c^2 \\
    &\quad +4c^4+4b^2c^2-8b^4c^2-8b^2c^4+4c^2d^2-8c^4d^2-2d^2\\
    &\quad +4b^2d^2+4c^2d^2+4b^2d^2-8b^4d^2+4d^4-8b^2d^4-8c^2d^4\\
    &\quad +8(a^2c^2d^2)\\
    &\quad -4(c^2d^2-a^2b^2-2c^4d^2-2c^2d^4+b^2d^2-a^2c^2\\
    &\quad -2b^4d^2-2b^2d^4+b^2c^2-a^2d^2-2b^4c^2\\
    &\quad -2b^2c^4+2a^2b^2)\\
    &=1-2c^2+4b^4+4b^2c^2-2d^2+4b^2d^2+4c^2d^2-2c^2+4b^2c^2 \\
    &\quad +4c^4+4b^2c^2-8b^4c^2-8b^2c^4+4c^2d^2-8c^4d^2-2d^2\\
    &\quad +4b^2d^2+4c^2d^2+4b^2d^2-8b^4d^2+4d^4-8b^2d^4-8c^2d^4\\
    &\quad +8(c^2d^2-b^2c^2d^2-c^4d^2-c^2d^4)\\
    &\quad -4(c^2d^2-a^2b^2-2c^4d^2-2c^2d^4+b^2d^2-a^2c^2\\
    &\quad -2b^4d^2-2b^2d^4+b^2c^2-a^2d^2-2b^4c^2\\
    &\quad -2b^2c^4+2a^2b^2)\\
    &=1-2c^2+4b^4+4b^2c^2-2d^2+4b^2d^2+4c^2d^2-2c^2+4b^2c^2 \\
    &\quad +4c^4+4b^2c^2-8b^4c^2-8b^2c^4+4c^2d^2-8c^4d^2-2d^2\\
    &\quad +4b^2d^2+4c^2d^2+4b^2d^2-8b^4d^2+4d^4-8b^2d^4-8c^2d^4\\
    &\quad +8(c^2d^2-b^2c^2d^2)\\
    &\quad -4(c^2d^2-a^2b^2+b^2d^2-a^2c^2\\
    &\quad -2b^4d^2-2b^2d^4+b^2c^2-a^2d^2-2b^4c^2\\
    &\quad -2b^2c^4+2a^2b^2)\\
    &=1-2c^2+4b^4+4b^2c^2-2d^2+8c^2d^2-2c^2+8b^2c^2 \\
    &\quad +4c^4-8b^4c^2-8b^2c^4-8c^4d^2-2d^2\\
    &\quad +8b^2d^2+4d^4-8b^2d^4-8c^2d^4\\
    &\quad +8(c^2d^2-b^2c^2d^2)\\
    &\quad -4(c^2d^2+a^2b^2-a^2c^2\\
    &\quad -2b^4d^2-2b^2d^4+b^2c^2-a^2d^2)\\
    &=1-4c^2+4b^4+4b^2c^2-4d^2+8c^2d^2-\\
    &\quad +4c^4-8b^4c^2-8b^2c^4-8c^4d^2\\
    &\quad +8b^2d^2+4d^4-8c^2d^4\\
    &\quad +4c^2d^2-8b^2c^2d^2\\
    &\quad -4(a^2b^2-a^2c^2-2b^4d^2-a^2d^2)\\
    &=1-4c^2+4b^4+4b^2c^2-4d^2+8c^2d^2-\\
    &\quad +4c^4-8b^4c^2-8b^2c^4-8c^4d^2\\
    &\quad +8b^2d^2+4d^4-8c^2d^4\\
    &\quad +4c^2d^2-8b^2c^2d^2+2b^4d^2\\
    &\quad -4(b^2-b^4-b^2c^2-b^2d^2-c^2+b^2c^2+c^4\\
    &\quad +c^2d^2-d^2+b^2d^2+c^2d^2+d^4)\\
    &=1+8b^4+4b^2c^2-8b^4c^2-8b^2c^4-8c^4d^2\\
    &\quad +8b^2d^2-8c^2d^4+4c^2d^2-8b^2c^2d^2+2b^4d^2-4b^2\\
    &=1+2b^4(1-c^2+d^2)
\end{aligned}
$$