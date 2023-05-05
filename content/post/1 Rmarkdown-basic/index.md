---
authors:
- admin
categories:
- R markdown
- 教程
date: "2023-03-22"
draft: false
featured: false
lastmod: "2023-03-22"
links:
- icon: database
  icon_pack: fas
  name: Rmarkdown Tutorial
  url: https://bookdown.org/yihui/rmarkdown/
- icon: book
  icon_pack: fas
  name: Tutorial on Rpubs
  url: https://rpubs.com/Chris622/Rmarkdown-Basics
projects: []
subtitle: "📕关于Rmarkdown的基础教程，包括创建，基本语法和实例"
summary: "📕关于Rmarkdown的基础教程，包括创建，基本语法和实例"
tags:
- R markdown
- R
title: R markdown | 基础知识
---


<script src="/rmarkdown-libs/htmlwidgets/htmlwidgets.js"></script>
<link href="/rmarkdown-libs/datatables-css/datatables-crosstalk.css" rel="stylesheet" />
<script src="/rmarkdown-libs/datatables-binding/datatables.js"></script>
<script src="/rmarkdown-libs/jquery/jquery-3.6.0.min.js"></script>
<link href="/rmarkdown-libs/dt-core/css/jquery.dataTables.min.css" rel="stylesheet" />
<link href="/rmarkdown-libs/dt-core/css/jquery.dataTables.extra.css" rel="stylesheet" />
<script src="/rmarkdown-libs/dt-core/js/jquery.dataTables.min.js"></script>
<link href="/rmarkdown-libs/crosstalk/css/crosstalk.min.css" rel="stylesheet" />
<script src="/rmarkdown-libs/crosstalk/js/crosstalk.min.js"></script>

# Installation

- [安装 R](https://www.r-project.org/)
- [安装 Rstudio](https://www.rstudio.com)
- 加载 `rmarkdown package`

``` r
# Install from CRAN
install.packages('rmarkdown')

# Or if you want to test the development version,
# install from GitHub
if (!requireNamespace("devtools"))
  install.packages('devtools')
devtools::install_github('rstudio/rmarkdown')
```

- 如果需要输出PDF格式，需要下载[TinyTex](https://yihui.name/tinytex/)

``` r
install.packages('tinytex')
tinytex::install_tinytex()  # install TinyTeX
```

# Introduction to R Markdown

## Video introduction to R Markdown

- [Get start video](https://rmarkdown.rstudio.com)
- [“A reproducible workflow” by Ignasi Bartomeus and Francisco Rodríguez-Sánchez](https://youtu.be/s3JldKoA0zw)
- [“The Importance of Reproducible Research in High-Throughput Biology” by Keith Baggerly](https://youtu.be/7gYIs7uYbMo).

## Examples applications

- [RPubs:a free publishing platform provided by RStudio](https://RPubs.com)
- [Personalized mail 个性化邮件](https://rmarkdown.rstudio.com/articles_mail_merge.html)
- [2017 Employer Health Benefits Survey](https://www.kff.org/health-costs/report/2017-employer-health-benefits-survey/)

# Compile R markdown document

- 使用Rstudio的`knit` 按钮
- 使用快捷键 `ctrl+shift+k`
- 使用函数 `rmarkdown::render`

``` r
rmarkdown::render('basic.Rmd', 'html_document')
```

- 另一个函数`xaringan::inf_mr` 使你可以在保存时，通过Rstudio的Viewer窗口实时预览输出，不需要按下Knit按钮

# Output format

- 通过YAML metadata里的`output`设置，例如，本文档的YAML头文件

``` yaml
---
title: "Rmarkdown Basics"
author: "lmj"
date: "2023-03-22"
output: 
  bookdown::html_document2: 
    base_format: gitbook
bibliography: Liu.bib
---
```

- 许多R package提供了漂亮的格式
  - `rmdformats`
  - `prettydoc`
  - `rticles`
  - `tufte`
  - `cerulean`
- 安装好上述包后，可以通过Rstudio调用：`File-New file-R markdown-From template`使用模板
- 或者添加格式到YAML头文件里使用

``` {output-yaml}
output: tufte::tufte_html
```

- 也可以自定义output参数：查看格式参数 `?rmarkdown::html_document`

``` {output-设置output参数}
output:
   html_document:
    toc: true
     toc_depth: 2
    dev: 'svg'
    #注意每一级都要缩进
```

- YAML里的字符通常不需要引号，如果不确定可以用`yaml`包里的函数测试

``` r
 cat(yaml::as.yaml(list(
  title = 'A Wonderful Day',
   subtitle = 'hygge: a quality of coziness'
 )))
```

    ## title: A Wonderful Day
    ## subtitle: 'hygge: a quality of coziness'

- YAML中需要解析的内容前面加 **!exr**

``` {output-yaml插入需要解析的代码}
 output:
  html_document:
    theme: !expr sample(c("yeti", "united", "lumen"), 1)
#随机选一个theme
```

- `R package:bookdown`拓展了Rmarkdown的功能,提供了诸如`gitbook`等格式
  - [更多关于bookdown的信息](https://bookdown.org/yihui/bookdown/)
  - [我的bookdown output format 笔记](https://rpubs.com/Chris622/1024608)

# R Markdown syntax

## 参考资料

- 采用 [pandoc’s markdown语法](https://pandoc.org/MANUAL.html)
- [R markdown cheatsheet](https://posit.co/wp-content/uploads/2022/10/rmarkdown-1.pdf)
- 在Rstudio里打开快速指南 `R studio-help-markdown quick reference`

## 常用语法

### 字体

- *斜体*: `*text*`或`_text_`
- **粗体**：`**text**`或`__text__`
- <sup>上标</sup>: `^supersript^`
- <sub>下标</sub>：`~subscript~`
- ~~删除线~~：`~~strikethrough~~`

### 链接

- 超链接: `[text](link)` [RStudio](https://www.rstudio.com)
- 脚注：`^[footnote]` [^1]

### 分区元素

- 节标题

``` {block-节标题}
# First-level header
## Second-level header
### Third-level header
```

- 非排序列表 `-` `+` `*` 引导

``` {block-非排序列表}
- one item
- one item
- one item
  - one more item
  - one more item
  - one more item
```

- 排序列表 `1.` `2.` 数字引导

``` {block-排序列表}
1. the first item
2. the second item
3. the third item
  - one unordered item
  - one unordered item
```

- 纯代码块 ```` ```text``` ````

<!-- -->

    This text is displayed verbatim / preformatted

- 引用块 `>text`

> “To be or not to be,
> that is a question.
>
> —ShakeSpear

- 注意不同元素之间需要空行防止模糊识别,
  例如下面两个例子中的`#`和`-`没有被识别为分区元素
  - In R, the character
    \# indicates a comment.

  - The result of 5
    -3 is 2.

### 数学表达式

- 采用latex数学表达式
- in-line 表达式：`$equation$` `\(f(k) = {n \choose k} p^{k} (1-p)^{n-k}\)`
- 独段表达式：`$$equation$$` `$$f(k) = {n \choose k} p^{k} (1-p)^{n-k}$$`

# R chunks

## 插入R code

- 快捷键 `Ctrl + Alt + I`
- Rstudio button `+C`
- markdown 语法
  - 代码条: `` `This is a in-line code` ``
  - 代码块： ```` ```code``` ````

  ``` {rcode-代码块}
    ```{r, chunk-label, results='hide', fig.height=4}```
  ```

## Chunk options

- `eval` 是否运行代码块
- `echo` 是否展示源代码
- `results`
  - `hide` 不展示结果
  - `hold` 所有结果一起展示在该段代码最下端
  - `asis` 安装markdown的语法输出，如下面的Markdown显示为**Markdown**而不是`**Markdown**`

``` r
cat('**Markdown** is cool.\n')
```

**Markdown** is cool.

- `collapse` 是否将代码和结果合并在一起输出，设置为TRUE可以使结构更紧凑

- `warning` `message` `error`是否显示代码的警告、信息和错误。注意，如果设置error = FALSE, rmarkdown::render()将在代码块中的错误时暂停，并且错误将显示在R控制台中。  

- `include` 是否展示code的所有信息，包括代码、输出结果、报错、警告等

  - 如果`include=F`但是`eval=T`，仍会运行代码，但所有相关内容都不展示
  - 如果要设置`echo = FALSE`, `results = 'hide'`, `warning = FALSE`, `message = FALSE`，那么可以直接通过设置`include=F`一次性完成

- `cache` 是否保存记录，如果设置为`T`，那么下次重复knit的时候将直接使用上一次的结果，避免重复运行

- 设置图片输出格式

  - `fig.width` 宽度
  - `fig.height` 高度
  - `fig.dim` `c(6,4)`表示宽6高4
  - `fig.asp` 宽度/高度
  - `out.width` and `out.height` 图片输出占页面的比例
  - `fig.align` 图片排布位置，可选`'left'`, `'center'`, or `'right'`
  - `fig.cap` 图名
  - `dev` 图片格式,可选`'pdf'`,`'png'`,`'svg'`,`'jpeg'`

- `child` 可以在主文档中包含子文档。此选项获取一个外部文件的路径。

- `Chunk label`是一个可选的chunk选项。如果缺少，将生成一个默认形式为`unnamed-chunk-i`的标签，其中i是逐个递增的。在label中最好只使用字母数字字符(a-z, a-z和0-9)和破折号(-)，因为它们不是特殊字符，肯定适用于所有输出格式。

- 如果需要在多个代码块中频繁地将某个选项设置为某个值，可以考虑在文档的第一个代码块中全局地设置它

- [更多详细信息](https://yihui.name/knitr/options)

## Figures

### 使用markdown 插入图片

`![alt text or image title](path/to/image)`,例如：

![中国科学院大学](https://www.ucas.ac.cn/newStyle/images/lougou.png)

### 使用R code插入图片

- chunk option中设置图片格式(Section <a href="#option">6.2</a>)

``` {fig1-设置图片格式}
fig.cap='A figure example with a relative width 50%.',
fig.width =6,fig.asp=0.7,
fig.align='center',out.width='50%'
```

``` r
par(mar = c(4, 4, 0.1, 0.1))
plot(pressure, pch = 19, type = "b")
```

<div class="figure" style="text-align: center">

<img src="/post/Rmarkdown-basic/1basic_files/figure-html/Fig1-1.png" alt="A figure example with a relative width 50%." width="50%" />
<p class="caption">
Figure 1: A figure example with a relative width 50%.
</p>

</div>

- 设置`fig.show='hold',out.width='40%'` 将两张图片并排显示

``` r
par(mar = c(4, 4, 0.1, 0.1))
plot(pressure, pch = 19, type = "b")
plot(cars, pch = 19)
```

<div class="figure" style="text-align: center">

<img src="/post/Rmarkdown-basic/1basic_files/figure-html/Fig2-1.png" alt="Two plots placed side by side." width="40%" /><img src="/post/Rmarkdown-basic/1basic_files/figure-html/Fig2-2.png" alt="Two plots placed side by side." width="40%" />
<p class="caption">
Figure 2: Two plots placed side by side.
</p>

</div>

- 使用`knitr::include_graphics`插入图片，相比于使用markdown语法，具有以下优点
  - 各种格式均适用(Rmarkdown，LaTeX)
  - 可以采用`fig.`设置大小尺寸名称

``` r
knitr::include_graphics(rep("https://www.ucas.ac.cn/newStyle/images/lougou.png", 3))
```

<div class="figure" style="text-align: center">

<img src="https://www.ucas.ac.cn/newStyle/images/lougou.png" alt="UCAS Logo" width="60%" /><img src="https://www.ucas.ac.cn/newStyle/images/lougou.png" alt="UCAS Logo" width="60%" /><img src="https://www.ucas.ac.cn/newStyle/images/lougou.png" alt="UCAS Logo" width="60%" />
<p class="caption">
Figure 3: UCAS Logo
</p>

</div>

## Tables

### 使用R code插入表格

- `knitr::kable`导出R code表格,其中用参数`caption`指定表格名

``` r
knitr::kable(
  head(mtcars[, 1:8], 10), booktabs = TRUE,
  caption = 'A table of the first 10 rows of the mtcars data.'
)
```

|                   |  mpg | cyl |  disp |  hp | drat |    wt |  qsec |  vs |
|:------------------|-----:|----:|------:|----:|-----:|------:|------:|----:|
| Mazda RX4         | 21.0 |   6 | 160.0 | 110 | 3.90 | 2.620 | 16.46 |   0 |
| Mazda RX4 Wag     | 21.0 |   6 | 160.0 | 110 | 3.90 | 2.875 | 17.02 |   0 |
| Datsun 710        | 22.8 |   4 | 108.0 |  93 | 3.85 | 2.320 | 18.61 |   1 |
| Hornet 4 Drive    | 21.4 |   6 | 258.0 | 110 | 3.08 | 3.215 | 19.44 |   1 |
| Hornet Sportabout | 18.7 |   8 | 360.0 | 175 | 3.15 | 3.440 | 17.02 |   0 |
| Valiant           | 18.1 |   6 | 225.0 | 105 | 2.76 | 3.460 | 20.22 |   1 |
| Duster 360        | 14.3 |   8 | 360.0 | 245 | 3.21 | 3.570 | 15.84 |   0 |
| Merc 240D         | 24.4 |   4 | 146.7 |  62 | 3.69 | 3.190 | 20.00 |   1 |
| Merc 230          | 22.8 |   4 | 140.8 |  95 | 3.92 | 3.150 | 22.90 |   1 |
| Merc 280          | 19.2 |   6 | 167.6 | 123 | 3.92 | 3.440 | 18.30 |   1 |

Table 1: A table of the first 10 rows of the mtcars data.

- 将多个表格包装进一个`list`同时导出

``` r
knitr::kable(
  list(
    head(iris[, 1:2], 3),
    head(mtcars[, 1:3], 5)
  ),
  caption = 'A Tale of Two Tables.', booktabs = TRUE
)
```

<table class="kable_wrapper">
<caption>
Table 2: A Tale of Two Tables.
</caption>
<tbody>
<tr>
<td>

| Sepal.Length | Sepal.Width |
|-------------:|------------:|
|          5.1 |         3.5 |
|          4.9 |         3.0 |
|          4.7 |         3.2 |

</td>
<td>

|                   |  mpg | cyl | disp |
|:------------------|-----:|----:|-----:|
| Mazda RX4         | 21.0 |   6 |  160 |
| Mazda RX4 Wag     | 21.0 |   6 |  160 |
| Datsun 710        | 22.8 |   4 |  108 |
| Hornet 4 Drive    | 21.4 |   6 |  258 |
| Hornet Sportabout | 18.7 |   8 |  360 |

</td>
</tr>
</tbody>
</table>

### 使用Pandoc语法

- pandoc 支持多种[表格格式](https://pandoc.org/MANUAL.html#tables)
  - simple tables
  - multiline tables
  - grid tables
  - pipe tables
    - 开始和结束管道字符`|`是可选的，但所有列之间都需要管道。
    - 冒号表示列对齐。
    - 标题不能省略。若要模拟无表头的表，请包含带有空白单元格的表头。

``` {pipe-tab1}
#有表头的pipe table
| Right | Left | Default | Center |
|------:|:-----|---------|:------:|
|   12  |  12  |    12   |    12  |
|  123  |  123 |   123   |   123  |
|    1  |    1 |     1   |     1  |
```

| Right | Left | Default | Center |
|------:|:-----|---------|:------:|
|    12 | 12   | 12      |   12   |
|   123 | 123  | 123     |  123   |
|     1 | 1    | 1       |   1    |

``` {pipe-tab2}
#无表头的pipe table
|       |      |         |        |
|------:|:-----|---------|:------:|
|   12  |  12  |    12   |    12  |
|  123  |  123 |   123   |   123  |
|    1  |    1 |     1   |     1  |
```

|     |     |     |     |
|----:|:----|-----|:---:|
|  12 | 12  | 12  | 12  |
| 123 | 123 | 123 | 123 |
|   1 | 1   | 1   |  1  |

# Cross-reference

## 采用超链接形式

- 直接采用节名 `[Section header text]`
  - [Output format](#output)
  - [Figures](#figure)
- 采用链接文本+节名的形式 `[link text][Section header text]`
  - [Section 4 Output](#output)
- 采用链接文本+节备注名 `[link text][#ID]`，在这种情况下，需要在每条节标题后加`{#ID}`以标识ID
  - [Section 4 with ID output](#output)

## 采用bookdown拓展的交叉引用方式

- 基本的格式为 `\@ref(label)`,图片，表格，label前需要加前缀 `fig:`,`tab:`
- 引用节 在节后面通过`{#label}`设置label Section <a href="#output">4</a>
- 引用图片时label为R code的label Figure <a href="#fig:Fig1">1</a>
- 引用表格时label为R code的label Table <a href="#tab:tab">1</a>

# interactive documents

## HTML widgets

- [htmlwidgets](http://www.htmlwidgets.org/)将javascript库导入R
- 基于其发展了一些R packages
  - **DT**
  - **leaflet**
  - **dygraphs**
  - 更多关于[htmlwidgets for R](https://www.htmlwidgets.org/)

``` r
#install.packages('DT')
DT::datatable(iris)
```

<div class="datatables html-widget html-fill-item-overflow-hidden html-fill-item" id="htmlwidget-1" style="width:100%;height:auto;"></div>
<script type="application/json" data-for="htmlwidget-1">{"x":{"filter":"none","vertical":false,"data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60","61","62","63","64","65","66","67","68","69","70","71","72","73","74","75","76","77","78","79","80","81","82","83","84","85","86","87","88","89","90","91","92","93","94","95","96","97","98","99","100","101","102","103","104","105","106","107","108","109","110","111","112","113","114","115","116","117","118","119","120","121","122","123","124","125","126","127","128","129","130","131","132","133","134","135","136","137","138","139","140","141","142","143","144","145","146","147","148","149","150"],[5.1,4.9,4.7,4.6,5,5.4,4.6,5,4.4,4.9,5.4,4.8,4.8,4.3,5.8,5.7,5.4,5.1,5.7,5.1,5.4,5.1,4.6,5.1,4.8,5,5,5.2,5.2,4.7,4.8,5.4,5.2,5.5,4.9,5,5.5,4.9,4.4,5.1,5,4.5,4.4,5,5.1,4.8,5.1,4.6,5.3,5,7,6.4,6.9,5.5,6.5,5.7,6.3,4.9,6.6,5.2,5,5.9,6,6.1,5.6,6.7,5.6,5.8,6.2,5.6,5.9,6.1,6.3,6.1,6.4,6.6,6.8,6.7,6,5.7,5.5,5.5,5.8,6,5.4,6,6.7,6.3,5.6,5.5,5.5,6.1,5.8,5,5.6,5.7,5.7,6.2,5.1,5.7,6.3,5.8,7.1,6.3,6.5,7.6,4.9,7.3,6.7,7.2,6.5,6.4,6.8,5.7,5.8,6.4,6.5,7.7,7.7,6,6.9,5.6,7.7,6.3,6.7,7.2,6.2,6.1,6.4,7.2,7.4,7.9,6.4,6.3,6.1,7.7,6.3,6.4,6,6.9,6.7,6.9,5.8,6.8,6.7,6.7,6.3,6.5,6.2,5.9],[3.5,3,3.2,3.1,3.6,3.9,3.4,3.4,2.9,3.1,3.7,3.4,3,3,4,4.4,3.9,3.5,3.8,3.8,3.4,3.7,3.6,3.3,3.4,3,3.4,3.5,3.4,3.2,3.1,3.4,4.1,4.2,3.1,3.2,3.5,3.6,3,3.4,3.5,2.3,3.2,3.5,3.8,3,3.8,3.2,3.7,3.3,3.2,3.2,3.1,2.3,2.8,2.8,3.3,2.4,2.9,2.7,2,3,2.2,2.9,2.9,3.1,3,2.7,2.2,2.5,3.2,2.8,2.5,2.8,2.9,3,2.8,3,2.9,2.6,2.4,2.4,2.7,2.7,3,3.4,3.1,2.3,3,2.5,2.6,3,2.6,2.3,2.7,3,2.9,2.9,2.5,2.8,3.3,2.7,3,2.9,3,3,2.5,2.9,2.5,3.6,3.2,2.7,3,2.5,2.8,3.2,3,3.8,2.6,2.2,3.2,2.8,2.8,2.7,3.3,3.2,2.8,3,2.8,3,2.8,3.8,2.8,2.8,2.6,3,3.4,3.1,3,3.1,3.1,3.1,2.7,3.2,3.3,3,2.5,3,3.4,3],[1.4,1.4,1.3,1.5,1.4,1.7,1.4,1.5,1.4,1.5,1.5,1.6,1.4,1.1,1.2,1.5,1.3,1.4,1.7,1.5,1.7,1.5,1,1.7,1.9,1.6,1.6,1.5,1.4,1.6,1.6,1.5,1.5,1.4,1.5,1.2,1.3,1.4,1.3,1.5,1.3,1.3,1.3,1.6,1.9,1.4,1.6,1.4,1.5,1.4,4.7,4.5,4.9,4,4.6,4.5,4.7,3.3,4.6,3.9,3.5,4.2,4,4.7,3.6,4.4,4.5,4.1,4.5,3.9,4.8,4,4.9,4.7,4.3,4.4,4.8,5,4.5,3.5,3.8,3.7,3.9,5.1,4.5,4.5,4.7,4.4,4.1,4,4.4,4.6,4,3.3,4.2,4.2,4.2,4.3,3,4.1,6,5.1,5.9,5.6,5.8,6.6,4.5,6.3,5.8,6.1,5.1,5.3,5.5,5,5.1,5.3,5.5,6.7,6.9,5,5.7,4.9,6.7,4.9,5.7,6,4.8,4.9,5.6,5.8,6.1,6.4,5.6,5.1,5.6,6.1,5.6,5.5,4.8,5.4,5.6,5.1,5.1,5.9,5.7,5.2,5,5.2,5.4,5.1],[0.2,0.2,0.2,0.2,0.2,0.4,0.3,0.2,0.2,0.1,0.2,0.2,0.1,0.1,0.2,0.4,0.4,0.3,0.3,0.3,0.2,0.4,0.2,0.5,0.2,0.2,0.4,0.2,0.2,0.2,0.2,0.4,0.1,0.2,0.2,0.2,0.2,0.1,0.2,0.2,0.3,0.3,0.2,0.6,0.4,0.3,0.2,0.2,0.2,0.2,1.4,1.5,1.5,1.3,1.5,1.3,1.6,1,1.3,1.4,1,1.5,1,1.4,1.3,1.4,1.5,1,1.5,1.1,1.8,1.3,1.5,1.2,1.3,1.4,1.4,1.7,1.5,1,1.1,1,1.2,1.6,1.5,1.6,1.5,1.3,1.3,1.3,1.2,1.4,1.2,1,1.3,1.2,1.3,1.3,1.1,1.3,2.5,1.9,2.1,1.8,2.2,2.1,1.7,1.8,1.8,2.5,2,1.9,2.1,2,2.4,2.3,1.8,2.2,2.3,1.5,2.3,2,2,1.8,2.1,1.8,1.8,1.8,2.1,1.6,1.9,2,2.2,1.5,1.4,2.3,2.4,1.8,1.8,2.1,2.4,2.3,1.9,2.3,2.5,2.3,1.9,2,2.3,1.8],["setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","setosa","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","versicolor","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica","virginica"]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>Sepal.Length<\/th>\n      <th>Sepal.Width<\/th>\n      <th>Petal.Length<\/th>\n      <th>Petal.Width<\/th>\n      <th>Species<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":[1,2,3,4]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script>

- 当输出非html文件时，HTML小部件将自动通过webshot包呈现为截取的屏幕截图

``` r
install.packages("htmlwidgets")
install.packages("webshot")
webshot::install_phantomjs()
```

## Web pages

- `knitr::include_url()`导入网页

``` r
knitr::include_url('https://bookdown.org/yihui/bookdown/web-pages-and-shiny-apps.html',height = '600px')
```

<iframe src="https://bookdown.org/yihui/bookdown/web-pages-and-shiny-apps.html" width="768" height="600px" data-external="1">
</iframe>

## Shiny apps

- 要从R Markdown文档中调用`Shiny`代码，请将`runtime: Shiny`添加到YAML元数据中

``` {shiny-yaml}
---
title: "A Shiny Document"
output: html_document
runtime: shiny
---
```

- 标准的R绘图可以通过将其包装在`Shiny`的`renderPlot()`函数中进行交互。`selectInput()`函数创建了驱动图形的输入小部件。

``` r
#install.packages('shiny')
selectInput(
  'breaks', label = 'Number of bins:',
  choices = c(10, 20, 35, 50), selected = 20
)

renderPlot({
  par(mar = c(4, 4, .1, .5))
  hist(
    faithful$eruptions, as.numeric(input$breaks),
    col = 'gray', border = 'white',
    xlab = 'Duration (minutes)', main = ''
  )
})
```

- `knitr::include_app()`可以导入Shiny apps

``` r
#knitr::include_app("https://psim.shinyapps.io/business_game/",height = "600px")
```

- 更多关于[shiny](https://shiny.rstudio.com/)

# References & footnotes

[^1]: This is a footnote.
