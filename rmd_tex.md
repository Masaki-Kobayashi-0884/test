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
