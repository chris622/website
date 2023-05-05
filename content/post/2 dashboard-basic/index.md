---
authors:
- admin
categories:
- FlexDashboard
- 教程
date: "2023-04-06"
draft: false
featured: false
lastmod: "2023-04-06"
links:
- icon: database
  icon_pack: fas
  name: FlexDashboard Tutorial
  url: https://pkgs.rstudio.com/flexdashboard/
- icon: chart-line
  icon_pack: fas
  name: Flexdashboard Example
  url: https://rpubs.com/Chris622/dashboard-basic
- icon: book
  icon_pack: fas
  name: Tutorial on Rpubs
  url: https://rpubs.com/Chris622/FlexDashboard-basic
projects: []
subtitle: "📈关于FlexDashboard的基础教程，包括布局，主题设置和实例"
summary: "📈关于FlexDashboard的基础教程，包括布局，主题设置和实例"
tags:
- FlexDashboard
- R
title: FlexDashboard | 基础知识
---

# Flexdashboard {#flex}
## Install package{#install}

```r
#安装 flexdashboard package
install.packages("flexdashboard")
remotes::install_github('rstudio/flexdashboard')
```
## Usage{#use}
+ 使用Rstudio：`File-New File-Rmarkdown-template-flex_dashboard`
+ 在YAML的`output`中加入`flexdashboard::flex_dashboard`
```{flex-YAML}
title: "dashboard"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
```

# Layout{#layout}
## Layout by Column {#col}  
+ Dashboard 默认按列布局，下面是一个简单的例子
```{flex-dash}
---
title: "Column Orientation"
output: flexdashboard::flex_dashboard
---
    
Column
-------------------------------------
### Chart 1
   
Column
-------------------------------------
### Chart 2
 
### Chart 3

```

+ 二级标题`##Row`创建列,一般为了突出显示,写成上述`title + dash line`的形式
+ 三级标题`###Chart`生成一个框(可以包含一个或多个仪表板组件)


## Layout by Row {#row}

+ 可以通过设置`orientation: rows`改成按行分布
```{row-oriented}
output:
  flexdashboard::flex_dashboard:
    orientation: rows
```

## Scrolling Layout{#scroll}   
+ 默认情况下，`vertical_layout=fill`,图表会自动填充浏览器的高度。
+ 如果你有很多图表，可以使用`vertical_layout=scroll`设置成通过滑动切换不同的图表。

```scroll
---
title: "Chart Stack (Scrolling)"
output: 
  flexdashboard::flex_dashboard:
    vertical_layout: scroll
---
```

## Tabsets {#tab}
+ 在许多情况下，选项卡**tabset**是比`vertical_layout:scroll`更好的解决方案，因为它们很容易导航。
+ 添加`{.tabset}`到节标题上，下一层的内容就会以选项卡的形式排列
+ `{.tabset-fade}`切换标签时添加淡入/淡出效果

```tab
Two tabs {.tabset}
------------------
### Tab A

### Tab B
```



## Header attributes {#attribute}
+ 使用`#identifier`为节标题添加唯一标识符
+ 使用`{data-width=300}`和`{data-height=600}`分别设置宽度和高度
+ 注意`key=value`中间不要有空格,否则要用引号连起来，如`data-navmenu="More Info"`


## Multiple pages
+ 使用一级标题 `# title` 或者 `title + equal line` 设置分页

```pages
---
title: "Multiple Pages"
output: flexdashboard::flex_dashboard
---

Visualizations {data-icon="fa-signal"}
===================================== 
    
### Chart 1
    
### Chart 2

Tables {data-icon="fa-table"}
=====================================     
### Table 1
### Table 2

```

<div class="figure" style="text-align: center">
<img src="https://bookdown.org/yihui/rmarkdown/images/dashboard-pages.png" alt="Multiple pages on a dashboard"  />
<p class="caption">Figure 1: Multiple pages on a dashboard</p>
</div>
+ 页面标题显示为仪表板顶部的导航菜单(Figure <a href="#fig:page-image">1</a>)。可以通过`data-icon`属性将[图标](https://fontawesome.com)应用于页面标题
+ `{data-orientation=rows}`设置页面布局为横向
+ 默认情况下，每个页面在导航栏上都有自己的顶级选项卡。但是，如果有大量的页面(超过5个)，可以使用`{data-navmenu="Menu A" data-navmenu-icon="fa-bookmark"}`把页面组织成导航栏上的菜单。
+ 下面的例子将**Page 1**和**Page 2**组织到**Menu A**， **Page 3**和**Page 4** 组织到**Memu B**。
```{page-navmenu}
---
title: "Page Navigation Menus"
output: flexdashboard::flex_dashboard
---

Page 1 {data-navmenu="Menu A" data-navmenu-icon="fa-bookmark"}
=====================================

Page 2 {data-navmenu="Menu A"}
=====================================  

Page 3 {data-navmenu="Menu B" data-navmenu="fa-cog"}
=====================================

Page 4 {data-navmenu="Menu B"}
=====================================  
```
+ 和rmarkdown一样，可以采用`[section title]`或者`[link label](#section identifier)`链接不同的页面
+ `{.hidden}`可以设置隐藏页面，使它不出现在顶部导航栏，然后通过上一条的页面链接到达
+ 下面的例子隐藏了**Page 3**，然后在**Page 1**里设置了链接`[Page 3](#page-3)`指向**Page 3**
```{hide-page}
---
title: "Hiding Pages"
output: flexdashboard::flex_dashboard
---

Sidebar {.sidebar}
=====================================
Link to [Page 3](#page-3)

Page 2 
=====================================  

Page 3 {.hidden}
=====================================
```

## Story board   

`storyboard`布局允许给图片添加评述,顶部的左/右导航按钮，点击后逐一浏览所有可视化和相关的注释 (Figure <a href="#fig:story-image">2</a>)。

+ 创建storyboard：`storyboard: true`
```{story-board}
---
title: "Storyboard Commentary"
output: 
  flexdashboard::flex_dashboard:
    storyboard: true
---

### A nice scatterplot here

---

Some commentary about Frame 1.

### A beautiful histogram on this board

---

Some commentary about Frame 2.
```

<div class="figure" style="text-align: center">
<img src="https://bookdown.org/yihui/rmarkdown/images/dashboard-story.png" alt="An example story board"  />
<p class="caption">Figure 2: An example story board</p>
</div>

+ `{.storyboard}`将某一个页面设置为storyboard
```{story-page}
---
title: "Storyboard Page"
output: flexdashboard::flex_dashboard
---

Analysis {.storyboard}
=========================================

### Frame 1

### Frame 2

Details
=========================================

Column
-----------------------------------------
```
+ `***`向storyboard添加评注
```{storyboard-comment}
---
title: "Storyboard Commentary"
output: 
  flexdashboard::flex_dashboard:
    storyboard: true
---

### Frame 1

*** 

Some commentary about Frame 1.

### Frame 2 {data-commentary-width=400}

*** 
Some commentary about Frame 2.

```

# Themes {#theme}
+ 有多种主题可用于修改flexdashboard的基本外观。可用的主题包括:
  + default
  + cosmo
  + bootstrap
  + cerulean
  + journal
  + flatly
  + readable
  + spacelab
  + united
  + lumen
  + paper
  + sandstone
  + simplex
  + yeti
+ 通过`theme`设置主题
```{theme-set}
---
title: "Themes"
output: 
  flexdashboard::flex_dashboard:
    theme: bootstrap
---
```
+ [bootswatch](https://bootswatch.com/)提供多种主题，可以在theme里调用

```bootswatch
---
title: "Themes"
output: 
  flexdashboard::flex_dashboard:
    theme: 
      bootswatch: minty
---
```

+ 使用`logo` 和`favicon`添加图标

```logo
---
title: "Logo and Favicon"
output: 
  flexdashboard::flex_dashboard:
    logo: logo.png
    favicon: favicon.png
---
```
+ 也可以使用`css`设置外观
```{css-set}
---
title: "Custom CSS"
output: 
  flexdashboard::flex_dashboard:
    css: styles.css
---
```

# Sizing {#size}
+ 配置为适合页面(`vertical_layout: fill`)：图表高度由浏览器高度决定
+ 配置为滚动(`vertical_layout: scroll`)：图表高度由`fig.height`决定，默认为480 pixels
+ 可以通过将`data-width`和`data-height`属性应用于行row、列column甚至单个图表chart来修改默认的大小。这些属性设定了在相同维度(水平或垂直)中布局的图表之间的**相对大小**。
+ 默认情况下，flexdashboard在图表边缘放置8个像素的填充。可以添加.no-padding属性来指定完全不填充，也可以添加`data-padding`属性来指定特定的像素数量。

```pad
### Chart 1 {.no-padding}

### Chart 2 {data-padding=10}
```

# An example

下面是基于前文的一个例子，效果参见 [Dashboard Basic Example](https://rpubs.com/Chris622/dashboard-basic)


```exp
---
title: "Dashboard (Basic)"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
    theme: readable
    navbar:
      - { title: "Author", href: "https://rpubs.com/Chris622" }
      - { icon: "fa-solid fa-book", href: "https://rpubs.com/Chris622/FlexDashboard-basic" }
    social: [ "twitter", "facebook", "menu" ]
---

Col-based Layout {#col-layout data-icon="fa-solid fa-file" data-navmenu="Layout" data-navmenu-icon="fa-regular fa-folder"}
===============================

这是一个按列布局的例子，按行布局参见：[row-layout](#row-layout)

This is an example for row-based Layout,you can refer to [row-layout](#row-layout)

col1 {data-width=550}
----------------------

### chart 1 

> 这是一个宽度为550的图表框 This is a chart box with a width of 550 

col2{.tabset .tabset-fade data-width=300}
----------------------

### chart 2 

> 这是一个宽度为300的图表框的一个选项卡 This is a TAB for a chart box of width 300

### chart 3


> 这是一个宽度为300的图表框的另一个选项卡 This is another TAB of a chart box of width 300

Row-based Layout {#row-layout data-icon="fa-regular fa-file" data-navmenu="Layout" data-orientation=rows}
===============================

这是一个按行布局的例子，按列布局参见：[col-layout](#col-layout)

This is an example for row-based Layout,you can refer to [col-layout](#col-layout)

row1 {data-heigh=550}
----------------------

### chart 1 

> 这是一个高度为550的图表框 This is a chart box with a height of 550 

col2{.tabset .tabset-fade data-heigh=300}
----------------------

### chart 2 

> 这是一个高度为300的图表框的一个选项卡 This is a TAB for a chart box of height 300

### chart 3

>这是一个高度为300的图表框的另一个选项卡 This is another TAB for a chart box of height 300

Story board {#story data-icon="fa-solid fa-book"  data-orientation=cols .storyboard}
===============================

### Frame 1


*** 
这是关于Frame1的注释

Some commentary about Frame 1.

### Frame 2 {data-commentary-width=400}



*** 
这是关于Frame2的注释

Some commentary about Frame 2.
```


