# $TeX$の書式

基本的に`\コマンド{文字列}`という構文になっています。

||コマンド|表示|
|:-:|:-:|:-:|
|分数|`\frac{a}{b}`|$$\frac{a}{b}$$|
|根号|`\sqrt{a}`|$$\sqrt{a}$$|
|累乗|`a^{b}`|$$a^{b}$$|
|下付き文字|`a_{b}`|$$a_{b}$$|
|不等号|`\ne\ge\le\gt\lt`|$$\ne\ge\le\gt\lt$$|
|矢印|`\to`|$$\to$$|
|極限|`\lim_{a}`|$$\lim_{a}$$|
|総和|`\sum_{a}^{b}`|$$\sum_{a}^{b}$$|
|総積|`\prod_{a}^{b}`|$$\prod_{a}^{b}$$|
|二項係数|`\binom{a}{b}`|$$\binom{a}{b}$$|
|無限|`\infty`|$$\infty$$|
|ギリシャ小文字|`\alpha`|$$\alpha$$|
|ギリシャ大文字|`\Beta`|$$\Beta$$|
|バー|`\bar{a}`|$$\bar{a}$$|
|ドット|`\cdot`|$$\cdot$$|
|3点リーダー|`\cdots\ldots\vdots\ddots`|$$\cdots\ldots\vdots\ddots$$|
|スペース|`a~~~~~~b`|$$a~~~~~~b$$|

## 位置を揃える

```tex
\begin{aligned}
  a&=b\\
  &=c
\end{aligned}
```

$$
\begin{aligned}
  a&=b\\
  &=c
\end{aligned}
$$
