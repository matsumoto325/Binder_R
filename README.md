# Test Repository for Binder with R

NII: [![Binder](https://binder.cs.rcos.nii.ac.jp/badge_logo.svg)](https://binder.cs.rcos.nii.ac.jp/v2/gh/JaehyunSong/Binder_R/master)

MyBinder: [![Binder](https://binder.cs.rcos.nii.ac.jp/badge_logo.svg)](https://binder.cs.rcos.nii.ac.jp/v2/gh/JaehyunSong/Binder_R/master)

## 備忘録

**初期設定時**

* 最上位branch名に注意。Binderのデフォルトはmasterだが、mainならbranch名にmainと指定

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
