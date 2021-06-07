# R Markdownによる$\TeX$文章作成

TeXを使ったレポートなどを簡単に作成できるように、環境設定を行います。

基本的にWindows向けですが、適宜読み替えれば他OSでも使えるはずです（未検証）。

## 1. 必要なツールのインストール

今回必要になるのは、

- [R](https://www.r-project.org)
- [RStudio](https://www.rstudio.com/products/rstudio/download/)
- [Ghostscript](https://www.ghostscript.com)

の3つです。手動でインストールしてもいいですが[chocolatey](https://chocolatey.org)や[winget](https://docs.microsoft.com/ja-jp/windows/package-manager/winget/)のようなパッケージマネージャーを使うと楽です。パッケージマネージャーのインストール方法は公式ページを参照してください。

### chocolateyの場合

```
choco install -y r r.studio ghostscript
```

### wingetの場合

```
winget install RProject.R
winget install RStudio.RStudio
winget install GhostScript
```

### 環境変数PATHの設定

GhostScriptにはパスが通ってないので手動で設定する必要があります。[この記事](https://www.atmarkit.co.jp/ait/articles/1805/11/news035.html)などを参考にして、インストールしたGhostScriptのbinフォルダをPathに追加します。私の場合は`C:\Program Files\gs\gs9.54.0\bin`を追加しました。

## 2. TeXのインストール

公式版のTeX Liveは非常に重いので**Tinytex**を利用します。TinytexはTeX Liveから必要最小限の機能を抜き出したものです。

RStudioを起動し、以下のスクリプトを実行してください。

```r
install.packages("tintex" dependencies = TRUE)
tinytex::install_tinytex()
tinytex::tlmgr_install("pdfcrop")
tinytex::tlmgr_install("texlive-msgmtranslations")
tinytex::tlmgr_install("haranoaji")
```

以上で環境設定は完了です。

```r
tinytex::lualatex("path/to/file.tex")
```

などとすると$\TeX$ファイルをPDFに変換することができます。

通常の$\TeX$環境としても利用することができ、`tlmgr --version`などのコマンドも動作します。

## 3. Rmdファイルの作成

$\TeX$ファイルを簡単に記述するために**R Markdown**を使います。

R MarkdownとはRの出力をMarkdownにテキストをとして貼り付ける`knitr`パッケージと、Markdownを様々な形式 $(\ni\TeX)$に変換する事ができる`Pandoc`を組み合わせたもので、直感的な書き方で文章の構造を定義することができます。

Rstudioの`File`から、`New File -> R Markdown -> PDF -> OK`とすると新しいRmdファイルが開かれます。

このままだと日本語が使えないので、YAMLヘッダ（先頭の`---`で囲まれた部分）を次のように書き換えます。

```yaml
title: タイトル
author: 名前
date: "`r Sys.Date()`"
output:
  pdf_document:
    latex_engine: lualatex
documentclass: bxjsarticle
classoption: pandoc
```

エディタ上部の**Knit**ボタンを押すと変換が始まり、しばらくするとpdfファイルが作成されます。

## R Markdownの書き方

R Markdownでは[Pandoc Markdown](https://pandoc-doc-ja.readthedocs.io/ja/latest/users-guide.html)という記法により、見出し・表・リスト・数式・コードブロックなどを書くことができます。

### 見出し

```markdown
## 見出しレベル２
```

## 見出しレベル２

### 表

```markdown
|    |Sepal.Length|Sepal.Width|Petal.Length|Petal.Width|Species|
|:---|-----------:|----------:|-----------:|----------:|------:|
|1   |         5.1|        3.5|         1.4|        0.2| setosa|
|2   |         4.9|        3.0|         1.4|        0.2| setosa|
|3   |         4.7|        3.2|         1.3|        0.2| setosa|
```

|    |Sepal.Length|Sepal.Width|Petal.Length|Petal.Width|Species|
|:---|-----------:|----------:|-----------:|----------:|------:|
|1   |         5.1|        3.5|         1.4|        0.2| setosa|
|2   |         4.9|        3.0|         1.4|        0.2| setosa|
|3   |         4.7|        3.2|         1.3|        0.2| setosa|

### リスト

```markdown
- setosa
- verslcolor
- virginica
```

- setosa
- verslcolor
- virginica

### 数式

```markdwon
$$\int_{-\infty}^{\infty} e^{-ax^2}dx = \sqrt{\frac{\pi}{a}}$$
```

$$\int_{-\infty}^{\infty} e^{-ax^2}dx = \sqrt{\frac{\pi}{a}}$$

### コードブロック

    ```r
    print("Hello World")
    ```

```r
print("Hello World")
```

## knitrパッケージ

さらに、R Markdownでは、Rスクリプトを実行し、その出力をMarkdwonに貼り付ける事もできます。

### 出力の貼り付け

    ```{.r .numberLines}
    summary(iris)
    ```

```
#   Sepal.Length    Sepal.Width     Petal.Length    Petal.Width
#  Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100
#  1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300
#  Median :5.800   Median :3.000   Median :4.350   Median :1.300
#  Mean   :5.843   Mean   :3.057   Mean   :3.758   Mean   :1.199
#  3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800
#  Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500
#        Species
#  setosa    :50
#  versicolor:50
#  virginica :50
#
#
#
```

### プロットの貼り付け

    ```{.r .numberLines}
    library(ggplot2)
    ggplot(iris, aes(x = Sepal.Length, y = Sepal.Width, color = Species)) +
      geom_point()
    ```

![](./img/iris-1.png)

## Enjoy!

参考文献

* [R Markdown入門](https://kazutan.github.io/kazutanR/Rmd_intro.html)
* [R Markdownによるレポート生成](https://qiita.com/tomotagwork/items/c92fb40a76f56ea16aa4)
* [R Markdownで楽々レポートづくり](https://gihyo.jp/admin/serial/01/r-markdown/0002)
* [R Markdown: The Definitive Guide](https://bookdown.org/yihui/rmarkdown/)
