---
authors:
- admin
categories:
- FlexDashboard
- 教程
date: "2023-04-07"
draft: false
featured: false
lastmod: "2023-04-07"
links:
- icon: database
  icon_pack: fas
  name: FlexDashboard Tutorial
  url: https://pkgs.rstudio.com/flexdashboard/
- icon: chart-line
  icon_pack: fas
  name: Flexdashboard Example
  url: https://liumj1998.shinyapps.io/dash-exp/
- icon: book
  icon_pack: fas
  name: Tutorial on Rpubs
  url: https://rpubs.com/Chris622/Flexdashboard-Advance
projects: []
subtitle: "📈关于FlexDashboard的进阶教程，包括组件，shiny和html widget实例"
summary: "📈关于FlexDashboard的进阶教程，包括组件，shiny和html widget实例"
tags:
- FlexDashboard
- R
- shiny
- html widget
title: FlexDashboard | 进阶
---

<script src="5 FlexDashboard(advance)_files/htmlwidgets/htmlwidgets.js"></script>
<script src="5 FlexDashboard(advance)_files/jquery/jquery.min.js"></script>
<link href="5 FlexDashboard(advance)_files/leaflet/leaflet.css" rel="stylesheet" />
<script src="5 FlexDashboard(advance)_files/leaflet/leaflet.js"></script>
<link href="5 FlexDashboard(advance)_files/leafletfix/leafletfix.css" rel="stylesheet" />
<script src="5 FlexDashboard(advance)_files/proj4/proj4.min.js"></script>
<script src="5 FlexDashboard(advance)_files/Proj4Leaflet/proj4leaflet.js"></script>
<link href="5 FlexDashboard(advance)_files/rstudio_leaflet/rstudio_leaflet.css" rel="stylesheet" />
<script src="5 FlexDashboard(advance)_files/leaflet-binding/leaflet.js"></script>
<script src="5 FlexDashboard(advance)_files/htmlwidgets/htmlwidgets.js"></script>
<script src="5 FlexDashboard(advance)_files/jquery/jquery.min.js"></script>
<link href="5 FlexDashboard(advance)_files/dygraphs/dygraph.css" rel="stylesheet" />
<script src="5 FlexDashboard(advance)_files/dygraphs/dygraph-combined.js"></script>
<script src="5 FlexDashboard(advance)_files/dygraphs/shapes.js"></script>
<script src="5 FlexDashboard(advance)_files/moment/moment.js"></script>
<script src="5 FlexDashboard(advance)_files/moment-timezone/moment-timezone-with-data.js"></script>
<script src="5 FlexDashboard(advance)_files/moment-fquarter/moment-fquarter.min.js"></script>
<script src="5 FlexDashboard(advance)_files/dygraphs-binding/dygraphs.js"></script>
<script src="5 FlexDashboard(advance)_files/htmlwidgets/htmlwidgets.js"></script>
<script src="5 FlexDashboard(advance)_files/plotly-binding/plotly.js"></script>
<script src="5 FlexDashboard(advance)_files/typedarray/typedarray.min.js"></script>
<script src="5 FlexDashboard(advance)_files/jquery/jquery.min.js"></script>
<link href="5 FlexDashboard(advance)_files/crosstalk/css/crosstalk.min.css" rel="stylesheet" />
<script src="5 FlexDashboard(advance)_files/crosstalk/js/crosstalk.min.js"></script>
<link href="5 FlexDashboard(advance)_files/plotly-htmlwidgets-css/plotly-htmlwidgets.css" rel="stylesheet" />
<script src="5 FlexDashboard(advance)_files/plotly-main/plotly-latest.min.js"></script>
<script src="5 FlexDashboard(advance)_files/htmlwidgets/htmlwidgets.js"></script>
<link href="5 FlexDashboard(advance)_files/bokehjs/bokeh.min.css" rel="stylesheet" />
<link href="5 FlexDashboard(advance)_files/bokehjs/loader.css" rel="stylesheet" />
<script src="5 FlexDashboard(advance)_files/bokehjs/bokeh.min.js"></script>
<script src="5 FlexDashboard(advance)_files/rbokeh-binding/rbokeh.js"></script>
<script src="5 FlexDashboard(advance)_files/htmlwidgets/htmlwidgets.js"></script>
<script src="5 FlexDashboard(advance)_files/jquery/jquery.min.js"></script>
<script src="5 FlexDashboard(advance)_files/proj4js/proj4.js"></script>
<link href="5 FlexDashboard(advance)_files/highcharts/css/motion.css" rel="stylesheet" />
<script src="5 FlexDashboard(advance)_files/highcharts/highcharts.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/highcharts-3d.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/highcharts-more.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/stock.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/map.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/data.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/exporting.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/offline-exporting.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/drilldown.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/item-series.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/overlapping-datalabels.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/annotations.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/export-data.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/funnel.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/heatmap.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/treemap.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/sankey.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/dependency-wheel.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/organization.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/solid-gauge.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/streamgraph.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/sunburst.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/vector.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/wordcloud.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/xrange.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/tilemap.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/venn.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/gantt.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/timeline.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/parallel-coordinates.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/bullet.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/coloraxis.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/dumbbell.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/lollipop.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/series-label.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/plugins/motion.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/custom/reset.js"></script>
<script src="5 FlexDashboard(advance)_files/highcharts/modules/boost.js"></script>
<script src="5 FlexDashboard(advance)_files/highchart-binding/highchart.js"></script>

**本文中提到的Dashboard组分实例可参见** [A Dashboard example by lmj](http://liumj1998.shinyapps.io/dash-exp)

# Components

Dashboard里可以包含的组分有

- 基于HTML小部件的交互式JavaScript数据可视化 **HTML widgets** (Section <a href="#widget">3</a>)
- R图形输出：包括base, lattice, and grid graphics **R graphical output**
- 表格数据(带有可选的排序、过滤和分页) **Tabular data**
- 用于突出显示重要摘要数据的值框 **Value boxes**
- 仪表上在指定范围内显示数值的仪表 **Gauges**
- 各种文本注释 **Text annotations**
- 一个导航栏，提供与仪表板相关的更多链接 **navigation bar**

其中后四个是`flexdashboard`特有的。

## R Graphics

当创建包含标准静态R图形仪表板时，设置`fig.width`和`fig.height`值控制图片大小。

## Tabular Data

### Simple table

- 简单表格采用`knitr::kable`调用

``` r
knitr::kable(mtcars)
```

### Data Table

- `DT package`可以将R表格显示为支持过滤、分页和排序的交互式HTML表。

``` r
install.packages("DT")
```

- `DT::datatable` 函数输出表格
- `bPaginate = FALSE`设置不分页，通过滑动展示全部数据

``` r
DT::datatable(mtcars, options = list(
  bPaginate = FALSE
))
```

- `pagelength`设置一页展示多少条数据

``` r
DT::datatable(mtcars, options = list(
  pagelength =25
))
```

## Value box

使用`valueBox`发出一个值并指定一个图标，参数包括

- `value` 值
- `icon` [图标](https://fontawesome.com)
- `color` 颜色，这可以是一个内置的背景颜色:`"primary"`， `"info"`， `"success"`， `"warning"`， `"danger"`或任何有效的CSS颜色值,e.g., `"#ffffff"`, `"rgb(100, 100, 100)"`
- `heref` 一个可选的URL链接。也可以是另一个仪表板页面的锚(例如。“\#details”)

``` vb
spam=12
valueBox(
  value=spam, #值
  icon = "fa-trash",#图标
  color = ifelse(spam > 10, "warning", "primary")
)
```

<div class="figure" style="text-align: center">

<img src="https://bookdown.org/yihui/rmarkdown/images/dashboard-valueboxes.png" alt="Three value boxes side by side on a dashboard"  />
<p class="caption">
Figure 1: Three value boxes side by side on a dashboard
</p>

</div>

## Gauges

`gauge`在指定范围内显示仪表上的数值,参数包括：

- `value` 值
- `min` 最大值
- `max` 最小值
- `sectors = gaugeSectors()` `gaugeSectors()`指定`"success"`, `"warning"`, `"danger"`的分隔
- `symbol = NULL` 单位
- `label = NULL` 展示在数值下的标签
- `abbreviate = TRUE` 将大数字缩写(例如1234567 -1.23M)。默认为TRUE。
- `href = NULL` 一个可选的URL链接。也可以是另一个仪表板页面的锚(例如。“\#details”)
- `colors` 用于表示`"success"`, `"warning"`, `"danger"`范围的颜色向量。可以是一个内置的背景颜色:`"primary"`， `"info"`， `"success"`， `"warning"`， `"danger"`或任何有效的CSS颜色值,e.g., `"#ffffff"`, `"rgb(100, 100, 100)"`

``` gauge
gauge(37.4, min = 0, max = 50, 
      gaugeSectors(success = c(41, 50),
             warning = c(21, 40), 
             danger = c(0, 20)),
      symbol = 'unit',label='click to see more details')
```

<div class="figure" style="text-align: center">

<img src="https://bookdown.org/yihui/rmarkdown/images/dashboard-gauges.png" alt="Three gauges side by side on a dashboard"  />
<p class="caption">
Figure 2: Three gauges side by side on a dashboard
</p>

</div>

## Text annotations

<div class="figure" style="text-align: center">

<img src="https://bookdown.org/yihui/rmarkdown/images/dashboard-text.png" alt="Text annotations on a dashboard"  />
<p class="caption">
Figure 3: Text annotations on a dashboard
</p>

</div>

- 在引入dashboard section之前，在页面顶部包含文本 (Figure <a href="#fig:text-image">3</a> **顶部文字**)

``` {before-txt}
---
title: "Text Annotations"
output:
  flexdashboard::flex_dashboard:
    orientation: rows
---

Monthly deaths from bronchitis, emphysema and asthma in the
UK, 1974–1979 (Source: P. J. Diggle, 1990, Time Series: A
Biostatistical Introduction. Oxford, table A.3)

Col1
----------------------
### section1
```

- 定义不包含图表但包含任意内容(文本、图像和方程等)的仪表板部分 (Figure <a href="#fig:text-image">3</a> **右下角文字框**)

``` {pure-txt}
### Pure txt

This example makes use of the dygraphs R package. The dygraphs
package provides rich facilities for charting time-series data 
in R. You can use dygraphs at the R console, within R Markdown
documents, and within Shiny applications.
```

- 用`>`对组件进行文字描述 (Figure <a href="#fig:text-image">3</a> **左下角图注**)

``` {description-txt}
### Male Deaths

[Here is a dashbord component produced by rcode]

> Monthly deaths from lung disease in the UK, 1974–1979
```

- 设置`{.no-title}`不显示节标题

``` notitle
### All Lung Deaths {.no-title}
```

## Navigation bar

- 添加链接 `title/icon+href`,`title`和`icon`至少有一样，设置`align`控制位置

``` about
---
title: "Navigation Bar"
output:
  flexdashboard::flex_dashboard:
    navbar:
      - { title: "About", href: "https://example.com/about" }
      - { icon: "fa-solid fa-book", href: "https://bookdown.org/yihui/rmarkdown/dashboards.html" ,align: left}
---
```

- 添加分享链接 `social`,`social`选项可以包括以下任何数量的服务:
  - `“facebook”`
  - `“twitter”`
  - `“google-plus”`
  - `“linkedin”`
  - `“pinterest”`
  - 还可以指定`“menu”`以提供包含所有服务的通用共享下拉菜单。

``` social
---
title: "Social Links"
output:
  flexdashboard::flex_dashboard:
    social: [ "twitter", "facebook", "menu" ]
---
```

- 添加源代码
  - 内嵌源代码 `source_code: embed`
  - 设置连接 `source_code: "https://github.com/user/repo"`

  ``` {source-code}
  ---
  title: "Source Code"
  output: 
  flexdashboard::flex_dashboard:
    source_code: embed
  ---
  ```

# Shiny Basics

## A shiny dashboard

<div class="figure" style="text-align: center">

<img src="https://bookdown.org/yihui/rmarkdown/images/dashboard-shiny.png" alt="An interactive dashboard based on Shiny"  />
<p class="caption">
Figure 4: An interactive dashboard based on Shiny
</p>

</div>

## Using Shiny

### Prepare

- 添加 `runtime:shiny` 到YAML在dashboard中引入`shiny`

``` {shiny-yanml}
---
title: "Old Faithful Eruptions"
output: flexdashboard::flex_dashboard
runtime: shiny
---
```

- 添加`global rcode`，加入需要的全局数据，并设置`include=FALSE`

``` global
# ```{r global,include=FALSE}
library(datasets)
data(faithful)
# ```
```

### Input & output

当你在一个flexdashboard中使用Shiny时，你会同时使用输入元素(例如滑块、复选框等)和输出元素(图表、表格等)。输入元素通常显示在侧边栏中，而输出显示在flexdashboard内容窗格中(也可以在单个窗格中组合输入和输出)。

下面是一个简单的例子，`sliderInput`设置滑块输入`bins`，`renderplot`调用输入值进行绘图

``` r
sliderInput("bins", "Number of bins:", 
            min = 1, max = 50, value = 30)

renderPlot({
  hist(faithful[, 2], breaks = input$bins)
})
```

- **Shiny Input**包括
  - [selectInput](https://shiny.rstudio.com/reference/shiny/latest/selectinput) 提供备选项输入框
  - [sliderInput](https://shiny.rstudio.com/reference/shiny/latest/sliderinput) 滑块输入调
  - [radioButtons](https://shiny.rstudio.com/reference/shiny/latest/radiobuttons) 一组单选按钮
  - [textInput](https://shiny.rstudio.com/reference/shiny/latest/textinput) 文本输入框
  - [numericInput](https://shiny.rstudio.com/reference/shiny/latest/numericinput) 数值输入框
  - [checkboxInput](https://shiny.rstudio.com/reference/shiny/latest/checkboxinput) 复选框/Ture False输入框
  - [dateInput](https://shiny.rstudio.com/reference/shiny/latest/dateinput) 日期输入框
  - [dateRangeInput](https://shiny.rstudio.com/reference/shiny/latest/daterangeinput) 日期段输入框
  - [fileInput](https://shiny.rstudio.com/reference/shiny/latest/fileinput) 文件输入框
- **Shiny Output**包括
  - [renderPlot](https://shiny.rstudio.com/reference/shiny/latest/renderplot) 输出R图形
  - [renderPrint](https://shiny.rstudio.com/reference/shiny/latest/renderprint) 输出打印
  - [renderTable](https://shiny.rstudio.com/reference/shiny/latest/rendertable) 输出表格
  - [renderText](https://shiny.rstudio.com/reference/shiny/latest/renderText) 输出字符向量
- 使用`shiny::renderPlot`绘图

``` renderplot
Column
--------------------------------------------------

### Geyser Eruption Duration

#```{r}
renderPlot({
  erpt = faithful$eruptions
  hist(
    erpt, probability = TRUE, breaks = as.integer(input$n_breaks),
    xlab = "Duration (minutes)", main = "Geyser Eruption Duration",
    col = 'gray', border = 'white'
  )
  
  dens = density(erpt, adjust = input$bw_adjust)
  lines(dens, col = "blue", lwd = 2)
})
#```
```

## Input sidebar

- 在dashboard节标题中指定`{.sidebar}` 添加shiny的input参数 (Figure <a href="#fig:shiny-image">4</a> **左侧 sidebar**)

``` sidebar
Column {.sidebar}#设置该列为sidebar
--------------------------------------------------

Waiting time between eruptions and the duration of the eruption
for the Old Faithful geyser in Yellowstone National Park,
Wyoming, USA.

 #```{r}
selectInput(
  "n_breaks", label = "Number of bins:",
  choices = c(10, 20, 35, 50), selected = 20
) 

sliderInput(
  "bw_adjust", label = "Bandwidth adjustment:",
  min = 0.2, max = 2, value = 1, step = 0.2
)
 #```
```

- 也可以将一级标题设置为sidebar，即整个页面为sidebar

``` global
---
title: "Sidebar for Multiple Pages"
output: flexdashboard::flex_dashboard
runtime: shiny
---
  
Sidebar {.sidebar}
=====================================
# shiny inputs defined here

Page 1
=====================================  
### Chart 1
    
Page 2
=====================================     
### Chart 2
```

- 也可以使用[shinydashboard](https://rstudio.github.io/shinydashboard/)包创建dashboard
- 更多关于shiny使用
  - [The official Shiny website](http://shiny.rstudio.com)
  - [shiny tutorial](https://shiny.rstudio.com/articles/interactive-docs.html)
  - [shiny cheatsheet](https://posit.co/wp-content/uploads/2022/10/shiny-1.pdf)

# HTML Widgets

CRAN上有超过30个提供`htmlwidgets`的包。可以在[htmlwidgets showcase](https://www.htmlwidgets.org/showcase_leaflet.html)中找到常用的htmlwidgets的示例使用，并在[gallary](https://gallery.htmlwidgets.org/)中浏览所有可用的wodgets。

## Leaflet

[leflet](https://rstudio.github.io/leaflet/)用于创建支持平移和缩放的**动态地图**，具有各种注释，如标记、多边形和弹出窗口。

``` r
library(leaflet)
library(maps)
mapStates = map("state", fill = TRUE, plot = FALSE)
leaflet(data = mapStates) %>% addTiles() %>%
  addPolygons(fillColor = topo.colors(10, alpha = NULL), stroke = FALSE)
```


## dygraphs

[dygraphs](https://rstudio.github.io/dygraphs/)为绘制**时间序列**数据提供丰富的工具，并支持许多交互功能，包括序列/点突出显示、缩放和平移。

``` r
library(dygraphs)
dygraph(nhtemp, main = "New Haven Temperatures") %>% 
  dyRangeSelector(dateWindow = c("1920-01-01", "1960-01-01"))
```


## Plotly

[Plotly](https://plotly.com/r/)通过它的ggplotly界面允许您轻松地将**ggplot2图形**转换为交互式的基于web的版本。

``` r
library(ggplot2)
library(plotly)
p <- ggplot(data = diamonds, aes(x = cut, fill = clarity)) +
            geom_bar(position = "dodge")
ggplotly(p)
```



## rbokeh

[rbokeh](https://hafen.github.io/rbokeh/)是Bokeh的接口，一个强大的声明式Bokeh框架，用于创建**基于web的图**。

``` r
library(rbokeh)
figure() %>%
  ly_points(Sepal.Length, Sepal.Width, data = iris,
    color = Species, glyph = Species,
    hover = list(Sepal.Length, Sepal.Width))
```



## Highcharter

[Highcharter](https://jkunst.com/highcharter/)一个**JavaScript图形库**的R接口。

``` r
library(magrittr)
library(highcharter)
hchart(mtcars, "scatter", hcaes(wt, mpg, z = drat, color = hp)) %>%
  hc_title(text = "Scatter chart with size and color")
```

