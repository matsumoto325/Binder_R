# Test Repository for Binder with R

[![Binder](https://binder.cs.rcos.nii.ac.jp/badge_logo.svg)](https://binder.cs.rcos.nii.ac.jp/v2/gh/JaehyunSong/Binder_R/master) (MyBinder.org)

---

## 動作環境

国立情報学研究所 (NII) のオンライン分析システム

```bash
$ $BASH_VERSION
bash: 4.4.20(1)-release: command not found

$ cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=18.04
DISTRIB_CODENAME=bionic
DISTRIB_DESCRIPTION="Ubuntu 18.04.5 LTS"

$ gcc --version
gcc (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

---

## 備忘録

**初期設定時**

* 最上位branch名に注意。Binderのデフォルトはmasterだが、mainならbranch名にmainと指定
* 分析環境から初期ファイル（`apt.txt`や`environment.yml`）などを削除したい場合は`postBuild`ファイル内で指定

**パス指定 (解決済み)**

* NIIオンライン分析システムでは解決
* `start`ファイルを用いて`LD_LIBRARY_PATH`を指定しないと、condaの`libstdc++.so.6`でなく、ubuntuの`libstdc++.so.6`を参照することとなり、{Rcpp}パッケージに依存するパッケージの読み込みができない。[[参考](https://discourse.jupyter.org/t/glibcxx-3-4-26-not-found-from-rstudio/7778)]
  * ただし、Jupyter Notebookやターミナル上でRを使用するなら`start`ファイルはなしでOK
  * システム上のgccが9.0以上なら不要かも?
* {Rcpp}に依存するパッケージ読み込みの際、`/usr/lib/x86_64-linux-gnu/libstdc++.so.6: version 'GLIBCXX_3.4.26' not found`のエラーが表示される場合、レポジトリに`start`ファイルを生成し、以下のように記述

```bash
#!/bin/bash
set -e
export LD_LIBRARY_PATH=${NB_PYTHON_PREFIX}/lib:${LD_LIBRARY_PATH}
exec "$@"
```

**日本語作図**

RStudio上で表示は問題ない。ただし、PDFで書き出す際、フォントの埋め込みが必要なのでdeviceをCairoに指定。

* Plot Paneからの操作だとCairo利用にチェック
* `ggsave()`を利用するなら`device = cairo_pdf`を指定

```r
pacman::p_load(tidyverse)

tibble(x = rnorm(10), y = rnorm(10)) %>%
  ggplot(aes(x = x, y = y)) +
  geom_point() +
  labs(x = "横軸", y = "縦軸")

ggsave(filename = "Test.pdf", plot = last_plot(), device = cairo_pdf, height = 5, width = 5)
```

**プロジェクトを開く**

* Jupyter hubから`.Rproj`を選択しても開かれないため、RStudioを起動し、File > Open Project...で開く必要がある。

---

## 参考

* [Specifying an R environment with a runtime.txt file](https://github.com/binder-examples/r)
* [From Zero to Binder in R!](https://github.com/alan-turing-institute/the-turing-way/blob/master/workshops/boost-research-reproducibility-binder/workshop-presentations/zero-to-binder-r.md)
* [r-conda](https://github.com/binder-examples/r-conda)
  * こっちの方がDockerイメージ生成が速いらしい
* [オンライン分析システム（実証実験）](https://meatwiki.nii.ac.jp/confluence/display/niircosap)
  * [初期設定](https://binder.cs.rcos.nii.ac.jp/) (学認)
  * [Jupyter hub](https://jupyter.cs.rcos.nii.ac.jp/) (学認)
