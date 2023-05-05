---
title: "Shiny Basic"
author: "lmj"
date: "2023-04-17"
output: 
  bookdown::gitbook:
    split_by: none
---

# Introduction

## 下载加载shiny包


```r
#install.packages("shiny")
library(shiny)
```

## 创建shinyapp

-   **R studio**：`File-New Project-New Directory-Shiny Web Application`
-   **R code**: 已有app.R file,输入`shinyapp`然后`Shift+tab`


```r
ui <- fluidPage(
  "Hello, world!"
)
server <- function(input, output, session) {
}
shinyApp(ui, server)
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/e9e6b44c85c169aedd2c4e630022e7958aa57696/06ba0/images/basic-app/hello-world.png" alt="The very basic shiny app you’ll see when you run the code above"  />
<p class="caption">Figure 1: The very basic shiny app you’ll see when you run the code above</p>
</div>

## 运行app

-   **R studio**: `Run App`
-   **快捷键**：`ctrl+shift+enter`
-   关闭shiny app窗口结束运行

## 添加UI


```r
ui <- fluidPage(
  selectInput("dataset", label = "Dataset", choices = ls("package:datasets")),
  verbatimTextOutput("summary"),
  tableOutput("table")
)
```

-   `fluidPage()`设置页面布局
-   `selectInput()`控制输入
-   `verbatimTextOutput()`和`tableOutput()`控制输出

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/17eadbf2ccdea8d4648b05d486dfb465f562df69/d077d/demos/basic-app/ui.png" alt="The datasets app with UI"  />
<p class="caption">Figure 2: The datasets app with UI</p>
</div>

## 添加server {#server}

```r
server <- function(input, output, session) {
  output$summary <- renderPrint({
    dataset <- get(input$dataset, "package:datasets")
    summary(dataset)
  }) #对data的统计结果
  
  output$table <- renderTable({
    dataset <- get(input$dataset, "package:datasets")
    dataset
  })#data数据
}
```

-   赋值操作符(`<-`)的左侧，`output$ID`，表明正在定义具有该ID的Shiny输出
-   赋值的右侧,使用特定的`render{Type}`函数来包装代码，每个`render{Type}`一般会对应一个`{type}Output`

<div class="figure">
<img src="https://d33wubrfki0l68.cloudfront.net/bb766cc235f190fd2c28d8f011edb7f9cbe526a7/5d540/demos/basic-app/server.png" alt="Now that we’ve provided a server function that connects outputs and inputs, we have a fully functional app"  />
<p class="caption">Figure 3: Now that we’ve provided a server function that connects outputs and inputs, we have a fully functional app</p>
</div>

## 利用reactive expression减少重复代码 {#duplicate}

-   在上节代码中（Section <a href="#server">1.5</a>),`dataset <- get(input$dataset, "package:datasets")`重复出现了两次，每次都需要提取一次数据
-   为了简化代码，可以在`reactive({...})`里包装一段代码，然后将值赋给一个变量
-   代码**只会运行一次**，之后每次调用该变量都是使用之前运行的缓存记录

```r
server <- function(input, output, session) {
  # 创建reactive expression
  dataset <- reactive({
    get(input$dataset, "package:datasets")
  })

  output$summary <- renderPrint({
    # 像调用函数一样调用reactive expression
    summary(dataset())
  })
  
  output$table <- renderTable({
    dataset()
  })
}
```

## shiny cheatsheet 

更多关于shiny的内容可参见 [shiny cheatsheet](https://www.rstudio.com/resources/cheatsheets/)
<div class="figure">
<img src="https://d33wubrfki0l68.cloudfront.net/349cbcca50af85a9214e552296f33f14be37f768/52241/images/basic-app/cheatsheet.png" alt="Shiny cheatsheet, available from https://www.rstudio.com/resources/cheatsheets/"  />
<p class="caption">Figure 4: Shiny cheatsheet, available from https://www.rstudio.com/resources/cheatsheets/</p>
</div>

# Basic UI
## 输入 Inputs

所有的输入类型包括

+ 文本型 (Section <a href="#FreeText">2.1.2</a>)
+ 数值型 (Section <a href="#numeric">2.1.3</a>)
+ 日期型 (Section <a href="#dates">2.1.4</a>)
+ 选择型 (Section <a href="#choices">2.1.5</a>)
+ 文件型 (Section <a href="#file">2.1.6</a>)
+ 操作型 (Section <a href="#action">2.1.7</a>)

### 基本结构  
+ 所有的输入的第一个参数都是要定义一个名称`inputId`（如`name`),之后在server段通过`input$name`调用该输入内容,`inputID`必须：
  + 只包含数字、字母和下划线
  + 独一无二的
+ 大部分输入的第二个参数是`label`，在app内提示用户输入信息
+ 第三个参数是`value`，可以设置默认输入

```r
sliderInput("min", "Limit (minimum)", value = 50, min = 0, max = 100)
```

### 自由文本类型输入{#FreeText}
+ `textInput()` 输入文本
+ `passwordInput()` 输入密码
+ `textAreaInput()` 输入大段文本

```r
ui <- fluidPage(
  textInput("name", "What's your name?"),
  passwordInput("password", "What's your password?"),
  textAreaInput("story", "Tell me about yourself", rows = 3)
)
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/e7b5aeb0678eac89f60033eb84640779ad510c41/f7ade/demos/basic-ui/free-text.png" alt="自由文本类型输入"  />
<p class="caption">Figure 5: 自由文本类型输入</p>
</div>

+ 除了`label`，也可以采用`placeholder`提示输入信息

```r
textInput("name",placeholder = 'Your name')
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/0250dd2b285e770d5fd2a66670a6604775bb7f1e/eaa81/demos/basic-ui/placeholder.png" alt="placeholder提示输入信息"  />
<p class="caption">Figure 6: placeholder提示输入信息</p>
</div>

### 数值输入{#numeric}
+ `numericInput()` 数值型文本框
+ `sliderInput()` 数值型滑块条

```r
ui <- fluidPage(
  numericInput("num", "Number one", value = 0, min = 0, max = 100),
  sliderInput("num2", "Number two", value = 50, min = 0, max = 100),
  #如果sliderInput的value是两个数值的向量，则指定一个范围
  sliderInput("rng", "Range", value = c(10, 20), min = 0, max = 100)
)
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/dcd1cdd3477e7f859c8d5388b435ca75836d74ba/cdd56/demos/basic-ui/numeric.png" alt="数值输入"  />
<p class="caption">Figure 7: 数值输入</p>
</div>
+ 查看更多定义sliderInput样式的方法
  + `?sliderInput`
  + [Detials for slider Input]( https://shiny.rstudio.com/articles/sliders.html )

```r
#用sliderInput输入日期
  sliderInput("deliver", "When should we deliver?", 
              value = as.Date('2020-09-17'),timeFormat = '%F',
              min = as.Date('2020-09-16'), max = as.Date('2020-09-23')),
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/95326f3d91fd5e3c1eba0cfb8aa69a65f1cc0953/ae529/demos/basic-ui/date-slider.png" alt="用sliderInput输入日期"  />
<p class="caption">Figure 8: 用sliderInput输入日期</p>
</div>


```r
#animation sliderInput
  sliderInput("animation", "Looping Animation:",
              min = 0, max = 100,
              value = 5, step = 5,
              animate =
                animationOptions(interval = 5, loop = TRUE))
```
  
  

### 日期输入{#dates}
+ `dateInput()` 输入一个日期
+ `dateRangeInput()` 输入一个日期范围

```r
ui <- fluidPage(
  dateInput("dob", "When were you born?"),
  dateRangeInput("holiday", "When do you want to go on vacation next?")
)
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/585b8dcaf38038e5c2768e8a4858171baa04e308/02979/demos/basic-ui/date.png" alt="日期输入"  />
<p class="caption">Figure 9: 日期输入</p>
</div>

### 选择输入{#choices}
+ `selectInput()` 在下拉框里选择
+ `radioButtons()` 在选项按钮里选择

```r
animals <- c("dog", "cat", "mouse", "bird", "other", "I hate animals")

ui <- fluidPage(
  selectInput("state", "What's your favourite state?", state.name),
  radioButtons("animal", "What's your favourite animal?", animals)
)
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/2605fed791b02e16d28d2c389df9130d349bf1b3/4dc8e/demos/basic-ui/limited-choices.png" alt="选择输入"  />
<p class="caption">Figure 10: 选择输入</p>
</div>
+ `radioButtons()`可以使用非文本的选项，用`choiceNames`设置在app里的展示，`choiceValues`设置对应的返回值

```r
ui <- fluidPage(
  radioButtons("rb", "Choose one:",
    choiceNames = list(
      icon("angry"),
      icon("smile"),
      icon("sad-tear")
    ),
    choiceValues = list("angry", "happy", "sad")
  )
)
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/eab54ba86e9dda3fd9cd417553de341695d46d34/0eb0f/demos/basic-ui/radio-icon.png" alt="非文本的按钮选择"  />
<p class="caption">Figure 11: 非文本的按钮选择</p>
</div>

+ `selectInput()` 设置`multiple = TRUE`可以同时选择多个选项

```r
ui <- fluidPage(
  selectInput(
    "state", "What's your favourite state?", state.name,
    multiple = TRUE
  )
)
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/c53795ad09b7866f0ed7adc1b3adabdd6fc120db/437be/images/basic-ui/multi-select.png" alt="多个选择的选择输入"  />
<p class="caption">Figure 12: 多个选择的选择输入</p>
</div>
+ `selectInput()` 在选项中设置子集

```r
selectInput("state", "Choose a state:",
              list(`East Coast` = list("NY", "NJ", "CT"),
                   `West Coast` = list("WA", "OR", "CA"),
                   `Midwest` = list("MN", "WI", "IA"))
  )
```

+ `radioButtons()`没有办法同时选择多项，但是可以用`checkboxGroupInput()`进行多项选择

```r
ui <- fluidPage(
  checkboxGroupInput("animal", "What animals do you like?", animals)
)
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/d3d61d703bac03a0d2823da98e2dbba03d6619d4/51e34/demos/basic-ui/multi-radio.png" alt="checkbox多项选择输入"  />
<p class="caption">Figure 13: checkbox多项选择输入</p>
</div>

+ `checkboxInput()`单项的勾选框

```r
ui <- fluidPage(
  checkboxInput("cleanup", "Clean up?", value = TRUE),
  checkboxInput("shutdown", "Shutdown?")
)
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/ff021265e68d432ea7e3e574849a8f6a8f25b9b0/27e99/demos/basic-ui/yes-no.png" alt="checkbox单项选择输入"  />
<p class="caption">Figure 14: checkbox单项选择输入</p>
</div>

### 文件输入{#file}
+ `fileInput()` 输入文件

```r
ui <- fluidPage(
  fileInput("upload", NULL)
)
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/92e2422e9b00cec1d6a852cbdb484071ce51ae4e/85324/demos/basic-ui/upload.png" alt="文件输入"  />
<p class="caption">Figure 15: 文件输入</p>
</div>
### 操作输入 {#action}
+ `actionButton()` 操作按钮
+ `actionLink()` 操作链接
+ 操作链接和按钮常与`server`函数中的`observeEvent()`或`eventReactive()`配对。

```r
ui <- fluidPage(
  actionButton("click", "Click me!"),
  actionButton("drink", "Drink me!", icon = icon("cocktail"))
)
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/30a4be90f407bf28ef9a780e78c6d48b8e9e3112/b196c/demos/basic-ui/action.png" alt="操作按钮"  />
<p class="caption">Figure 16: 操作按钮</p>
</div>
+ 可以通过class参数设置按钮的外观
  + 样式设置：`"btn-primary"`, `"btn-success"`, `"btn-info"`, `"btn-warning"`, or `"btn-danger"`
  + 大小设置：`"btn-lg"`, `"btn-sm"`, `"btn-xs"`
  + 使用`"btn-block"`使按钮跨越嵌入元素的整个宽度

```r
ui <- fluidPage(
  fluidRow(
    actionButton("click", "Click me!", class = "btn-danger"),
    actionButton("drink", "Drink me!", class = "btn-lg btn-success")
  ),
  fluidRow(
    actionButton("eat", "Eat me!", class = "btn-block")
  )
)
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/4ffe5ca4125016e84a32a67177d74ec870798132/f2be2/demos/basic-ui/action-css.png" alt="操作按钮样式"  />
<p class="caption">Figure 17: 操作按钮样式</p>
</div>
  
  
## 输出 Outputs

所有的输出类型包括

+ 文本型 (Section <a href="#text">2.2.1</a>)
+ 表格型 (Section <a href="#table">2.2.2</a>)
+ 图片型 (Section <a href="#plots">2.2.3</a>)
+ 下载型 (Section <a href="#downloads">2.2.4</a>)


### 文本输出{#text}
+ `textOutput()` 简单文本输出，和`renderText()`联用,用于将结果连成简单的字符串
+ `verbatimTextOutput()` 代码和控制输出，和`renderPrint()`联用，输出R代码的结果

```r
ui <- fluidPage(
  textOutput("text"),
  verbatimTextOutput("code")
)
server <- function(input, output, session) {
  output$text <- renderText({ 
    "Hello friend!" 
  })#简单文本输出
  output$code <- renderPrint({ 
    summary(1:10) 
  })#代码输出
}
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/61f37c119d66f0b25bb834d6773274856b764727/a9ae2/demos/basic-ui/output-text.png" alt="文本输出"  />
<p class="caption">Figure 18: 文本输出</p>
</div>
+ `{}`用于包含多条代码的render语句，上述代码可以简化成

```r
server <- function(input, output, session) {
  output$text <- renderText("Hello friend!")
  output$code <- renderPrint(summary(1:10))
}
```
+ `renderText()`和`verbatimTextOutput()`的区别

```r
ui <- fluidPage(
  textOutput("text"),
  verbatimTextOutput("print")
)
server <- function(input, output, session) {
  output$text <- renderText("hello!")
  output$print <- renderPrint("hello!")
}
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/086a9fbcf20ad02bb679141fbe4f263d6cd7646c/dda5c/demos/basic-ui/text-vs-print.png" alt="两种文本输出的不同"  />
<p class="caption">Figure 19: 两种文本输出的不同</p>
</div>

### 表格输出{#table} 
+ `tableOutput()`和`renderTable()` 输出静态表格
+ `dataTableOutput()`和`renderDataTable()` 输出动态表格
+ [reactable包](https://glin.github.io/reactable/index.html)中的 `reactableOutput()`和 `renderReactable()`提供了更多自定义动态表格的选项

```r
ui <- fluidPage(
  tableOutput("static"),
  dataTableOutput("dynamic")
)
server <- function(input, output, session) {
  output$static <- renderTable(head(mtcars))
  output$dynamic <- renderDataTable(mtcars, options = list(pageLength = 5))
}
```

<div class="figure">
<img src="https://d33wubrfki0l68.cloudfront.net/2fe4c674645896fa18d345ad162f479434746dd2/2e28c/demos/basic-ui/output-table.png" alt="表格输出"  />
<p class="caption">Figure 20: 表格输出</p>
</div>

### 图形输出 {#plots} 
+ 使用`plotOutput()`和`renderPlot()`输出图形

```r
ui <- fluidPage(
  plotOutput("plot", width = "400px")
)
server <- function(input, output, session) {
  output$plot <- renderPlot(plot(1:5), res = 96,
                            width = 700,height = 300,alt="a scatterplot of five random numbers")
)
}#建议始终设置res =96，因为这将使您的Shiny图表尽可能接近您在RStudio中看到的。
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/c87e58475e7a0c2e0d12f06ac28169c0c670dc91/b2f86/demos/basic-ui/output-plot.png" alt="图形输出"  />
<p class="caption">Figure 21: 图形输出</p>
</div>

### 下载输出{#downloads}
+ 可以使用`downloadButton()`或`downloadLink()`让用户下载文件
+ 参见[更多信息](https://mastering-shiny.org/action-transfer.html#action-transfer)

# Basic reactivity
## serve 函数 {#serve}
+ 基本的shiny app组成

```r
library(shiny)

ui <- fluidPage(
  # front end interface
)

server <- function(input, output, session) {
  # back end logic
}

shinyApp(ui, server)
```
+ `ui`对象包含html格式的用户界面,每个用户有相同的ui
+ `server`函数控制app后台操作，每个用户的server不同。每个新的session开始时，shiny都会唤起一次server函数
+ server函数包括三个参数：`input`,`output`,`session`
  + `input$ID` 调用UI输入的内容
    + 不能在server里直接调用`input$ID`，需要在**reactive**环境中调用如：`renderText()` 或 `reactive()`
    + 不能给`input$ID`赋值，必要时可以采用`updateNumericInput()`
  + `outnput$ID` 调用UI定义的输出内容，通常与`render{}()`系列函数联用设置输出内容代码
    + `render{}()`函数创建一个**reactive**环境，可以调用input参数内容
    + `render{}()`函数将输出结果转化成**html**格式，便于在web上显示

## reactive 编程

+ **reactive programming**：输入变化时，输出结果也**自动**跟着变化
+ **imperative programming** 命令式编程，提出命令，执行结果 (R script)
+ **declarative programmin** 声明式编程，表达更高层次的目标或描述重要的限制，并依靠其他人来决定如何和/或何时将其转化为行动 (shiny)
+ 执行顺序不是逐条执行，例如，下述代码不会报错：

```r
server <- function(input, output, session) {
  output$greeting <- renderText(string())
  #注意这里要像调用函数一样调用reactive expression
  string <- reactive(paste0("Hello ", input$name, "!"))
  #string定义在调用后
}
```
+ 上述代码的执行顺序(reactive graph)
<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/6966978d8dc9ac65a0dfc6ec46ff05cfeef541e2/fdc7f/diagrams/basic-reactivity/graph-2b.png" alt="A reactive expression is drawn with angles on both sides because it connects inputs to outputs"  />
<p class="caption">Figure 22: A reactive expression is drawn with angles on both sides because it connects inputs to outputs</p>
</div>

## reactive expression 
**响应表达式**使用`reative()`函数创建。可以简化代码（避免重复代码，参见**Section** <a href="#duplicate">1.6</a>和**Section** <a href="#simplify">3.3.3</a>，同时具有inputs和outputs的特性

+ 与input一样，可以在output中使用reactive expression的结果。
+ 与output一样，reactive expression依赖于input，并自动知道何时需要更新。

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/8dcc3f2cbc55486a76b33a5acd30e379cd05d8ab/27c40/diagrams/basic-reactivity/producers-consumers.png" alt="Inputs and expressions are reactive producers; expressions and outputs are reactive consumers"  />
<p class="caption">Figure 23: Inputs and expressions are reactive producers; expressions and outputs are reactive consumers</p>
</div>

### 一个简单的EDA（Exploratory Data Analysis）例子

下面的例子分析两组数据的分布，并用T检验判断它们的差异是否显著

+ 创建函数绘图和输出结果

```r
library(ggplot2)

freqpoly <- function(x1, x2, binwidth = 0.1, xlim = c(-3, 3)) {
  df <- data.frame(
    x = c(x1, x2),
    g = c(rep("x1", length(x1)), rep("x2", length(x2)))
  )

  ggplot(df, aes(x, colour = g)) +
    geom_freqpoly(binwidth = binwidth, size = 1) +
    coord_cartesian(xlim = xlim)
}

t_test <- function(x1, x2) {
  test <- t.test(x1, x2)
  
  # use sprintf() to format t.test() results compactly
  sprintf(
    "p value: %0.3f\n[%0.2f, %0.2f]",
    test$p.value, test$conf.int[1], test$conf.int[2]
  )
}
```
+ 创建两组随机数使用上述函数

```r
x1 <- rnorm(100, mean = 0, sd = 0.5)
x2 <- rnorm(200, mean = 0.15, sd = 0.9)

freqpoly(x1, x2)
```

<img src="/post/4shiny-basic/shiny_files/figure-html/unnamed-chunk-52-1.png" width="672" />

```r
cat(t_test(x1, x2))
## p value: 0.937
## [-0.17, 0.16]
#> p value: 0.005
#> [-0.39, -0.07]
```

### 将常规代码转化成shiny app
+ 创建UI界面

```r
ui <- fluidPage(
  #输入布局
  fluidRow(
    #第一个变量数据设置
    column(4, 
      "Distribution 1",
      numericInput("n1", label = "n", value = 1000, min = 1),
      numericInput("mean1", label = "µ", value = 0, step = 0.1),
      numericInput("sd1", label = "σ", value = 0.5, min = 0.1, step = 0.1)
    ),
    #第二个变量数据设置
    column(4, 
      "Distribution 2",
      numericInput("n2", label = "n", value = 1000, min = 1),
      numericInput("mean2", label = "µ", value = 0, step = 0.1),
      numericInput("sd2", label = "σ", value = 0.5, min = 0.1, step = 0.1)
    ),
    #输出图形设置
    column(4,
      "Frequency polygon",
      numericInput("binwidth", label = "Bin width", value = 0.1, step = 0.1),
      sliderInput("range", label = "range", value = c(-3, 3), min = -5, max = 5)
    )
  ),
  #输出布局
  fluidRow(
    #输出分布图
    column(9, plotOutput("hist")),
    #输出t-test结果
    column(3, verbatimTextOutput("ttest"))
  )
)
```

+ 创建server函数

```r
server <- function(input, output, session) {
  #绘图
  output$hist <- renderPlot({
    x1 <- rnorm(input$n1, input$mean1, input$sd1)
    x2 <- rnorm(input$n2, input$mean2, input$sd2)
    
    freqpoly(x1, x2, binwidth = input$binwidth, xlim = input$range)
  }, res = 96)
  
  #t-test检验
  output$ttest <- renderText({
    x1 <- rnorm(input$n1, input$mean1, input$sd1)
    x2 <- rnorm(input$n2, input$mean2, input$sd2)
    
    t_test(x1, x2)
  })
}
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/c4b42aa78ca4fa1794c7f29a3b43e42610502212/32dcf/demos/basic-reactivity/case-study-1.png" alt="A Shiny app that lets you compare two simulated distributions with a t-test and a frequency polygon See live at https://hadley.shinyapps.io/ms-case-study-1"  />
<p class="caption">Figure 24: A Shiny app that lets you compare two simulated distributions with a t-test and a frequency polygon See live at https://hadley.shinyapps.io/ms-case-study-1</p>
</div>

+ 上述代码的reactive graph如下图所示，具有如下缺点
  + 这个应用程序很难理解，因为有太多的连接。
  + 这个应用的效率很低，因为它做了太多不必要的工作。例如，如果您更改了绘图的breaks，则会重新计算数据;如果你改变n1的值，x2也会被更新(在`hist`和`ttest`两个地方!)
<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/840574f96b31b47295cc1ec44d359c0493a6e1bb/4d300/diagrams/basic-reactivity/case-study-1.png" alt="The reactive graph shows that every output depends on every input"  />
<p class="caption">Figure 25: The reactive graph shows that every output depends on every input</p>
</div>

### 用reative expression简化app {#simplify}
+ 之前的`server`函数里`x1`和`x2`都重复计算了两次，可以使用**reactive expression**中简化代码

```r
server <- function(input, output, session) {
  x1 <- reactive(rnorm(input$n1, input$mean1, input$sd1))
  x2 <- reactive(rnorm(input$n2, input$mean2, input$sd2))

  output$hist <- renderPlot({
    freqpoly(x1(), x2(), binwidth = input$binwidth, xlim = input$range)
  }, res = 96)

  output$ttest <- renderText({
    t_test(x1(), x2())
  })
}
```
+ 此时的reactive graph如下图所示
<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/9eda867da502862eae05b9b574ebcc5828858bd8/b75dd/diagrams/basic-reactivity/case-study-3.png" alt="Using reactive expressions considerably simplifies the graph, making it much easier to understand"  />
<p class="caption">Figure 26: Using reactive expressions considerably simplifies the graph, making it much easier to understand</p>
</div>

在Shiny中，应该考虑一个规则:

> 每当复制和粘贴一次内容时，应该考虑将重复的代码提取到reactive expression中。

因为reactive expression不仅使人类更容易理解代码，而且还提高了Shiny有效重新运行代码的能力。

## 控制app更新

### 一个app例子 
下面是一个简单的app例子，输入两个lambda分布的参数`lambda1`，`lambda2`和`n`，使用`rpoins()`生成随机的两组数，输出它们的分布图

```r
ui <- fluidPage(
  fluidRow(
    column(3, 
      numericInput("lambda1", label = "lambda1", value = 3),
      numericInput("lambda2", label = "lambda2", value = 5),
      numericInput("n", label = "n", value = 1e4, min = 0)
    ),
    column(9, plotOutput("hist"))
  )
)
server <- function(input, output, session) {
  x1 <- reactive(rpois(input$n, input$lambda1))
  x2 <- reactive(rpois(input$n, input$lambda2))
  output$hist <- renderPlot({
    freqpoly(x1(), x2(), binwidth = 1, xlim = c(0, 40))
  }, res = 96)
}
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/ceed7209822edad11d811e6ad501f2e1cea7fb0d/b35bb/demos/basic-reactivity/simulation-2.png" alt="A simpler app that displays a frequency polygon of random numbers drawn from two Poisson distributions. See live at https://hadley.shinyapps.io/ms-simulation-2."  />
<p class="caption">Figure 27: A simpler app that displays a frequency polygon of random numbers drawn from two Poisson distributions. See live at https://hadley.shinyapps.io/ms-simulation-2.</p>
</div>

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/233a16963b00b9e879f225bc17a918afbad91e15/32c89/diagrams/basic-reactivity/timing.png" alt="The reactive graph"  />
<p class="caption">Figure 28: The reactive graph</p>
</div>

### reactiveTimer()定时更新
+ `reactiveTimer()`是一个响应式表达式，它依赖于一个隐藏的输入:当前时间。
+ 下面的例子通过`reactiveTimer()`设置每0.5s更新一次

```r
server <- function(input, output, session) {
  timer <- reactiveTimer(500)#引入reactiveTimer
  
  x1 <- reactive({
    #调用创建的reactiveTimer，使得x1和x2响应表达式依赖于timer
    timer()
    rpois(input$n, input$lambda1)
  })
  x2 <- reactive({
    timer()
    rpois(input$n, input$lambda2)
  })
  
  output$hist <- renderPlot({
    freqpoly(x1(), x2(), binwidth = 1, xlim = c(0, 40))
  }, res = 96)
}
```
+ 此时的reactive graph为
<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/82776537eeb3726235dd27f53df0f38880965a60/dfd59/diagrams/basic-reactivity/timing-timer.png" alt="reactiveTimer(500) introduces a new reactive input that automatically invalidates every half a second"  />
<p class="caption">Figure 29: reactiveTimer(500) introduces a new reactive input that automatically invalidates every half a second</p>
</div>

### 添加操作按钮手动更新
+ 使用`actionButton`引入操作按钮(**Section** <a href="#action">2.1.7</a>)
+ 将其引入`x1`，`x2`的响应表达式中，每次点击时激活一次更新

```r
ui <- fluidPage(
  fluidRow(
    column(3, 
      numericInput("lambda1", label = "lambda1", value = 3),
      numericInput("lambda2", label = "lambda2", value = 5),
      numericInput("n", label = "n", value = 1e4, min = 0),
      actionButton("simulate", "Simulate!")#引入actionbutton
    ),
    column(9, plotOutput("hist"))
  )
)

server <- function(input, output, session) {
  x1 <- reactive({
    input$simulate #actionbutton加入reactive expression
    rpois(input$n, input$lambda1)
  })
  x2 <- reactive({
    input$simulate
    rpois(input$n, input$lambda2)
  })
  output$hist <- renderPlot({
    freqpoly(x1(), x2(), binwidth = 1, xlim = c(0, 40))
  }, res = 96)
}
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/d65eff11607572a166ddc400a771e5e72fb32b99/362e4/demos/basic-reactivity/action-button.png" alt="App with action button. See live at https://hadley.shinyapps.io/ms-action-button."  />
<p class="caption">Figure 30: App with action button. See live at https://hadley.shinyapps.io/ms-action-button.</p>
</div>

+ `x1`和`x2`随着`simulate`，`lambda1`，`lambda2`和`n`的变化更新，这意味者**上述任意一个**发生变化都会影响输出结果
<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/ffefc8f9300348bb4c29492c4ebfa04235c78eec/94ace/diagrams/basic-reactivity/timing-button.png" alt="This reactive graph doesn’t accomplish our goal; we’ve added a dependency instead of replacing the existing dependencies."  />
<p class="caption">Figure 31: This reactive graph doesn’t accomplish our goal; we’ve added a dependency instead of replacing the existing dependencies.</p>
</div>

### eventReactive()事件触发
+ 如果只想当`simulate`点击时才触发响应表达式，需要采用`eventReactive()`函数
+ `eventReactive()`有两个参数:第一个参数指定要依赖什么（这里是`silumate()`），第二个参数指定要计算什么(这里是`rpois()`。

```r
server <- function(input, output, session) {
  x1 <- eventReactive(input$simulate, {
    rpois(input$n, input$lambda1)
  })
  x2 <- eventReactive(input$simulate, {
    rpois(input$n, input$lambda2)
  })

  output$hist <- renderPlot({
    freqpoly(x1(), x2(), binwidth = 1, xlim = c(0, 40))
  }, res = 96)
}
```
+ `x1`和`x2`只依赖于`simulate`，`这意味者**只有点击simulate按钮**才会改变输出结果
<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/85daccd0739423ca7b9463531a23e66d280c5724/eb459/diagrams/basic-reactivity/timing-button-2.png" alt="eventReactive() makes it possible to separate the dependencies (black arrows) from the values used to compute the result (pale gray arrows)."  />
<p class="caption">Figure 32: eventReactive() makes it possible to separate the dependencies (black arrows) from the values used to compute the result (pale gray arrows).</p>
</div>

### observerEvent()响应输入变化
+ `observeEvent()`与`eventReactive()`非常相似。它有两个重要参数:`eventExpr`和`handlerExpr`。第一个参数是要依赖的输入或表达式;第二个参数是将要运行的代码。
+ 下列代码在input$name发生变化时，输出message到控制台

```r
ui <- fluidPage(
  textInput("name", "What's your name?"),
  textOutput("greeting")
)

server <- function(input, output, session) {
  string <- reactive(paste0("Hello ", input$name, "!"))
  
  output$greeting <- renderText(string())
  observeEvent(input$name, {
    message("Greeting performed")
  })
}
```

<div class="figure" style="text-align: center">
<img src="https://d33wubrfki0l68.cloudfront.net/c34c3dfb6205be6ff2c2d3e8fca18f16d34d9f3a/9d229/diagrams/basic-reactivity/graph-3.png" alt=" In the reactive graph, an observer looks the same as an output"  />
<p class="caption">Figure 33:  In the reactive graph, an observer looks the same as an output</p>
</div>

+ **注意：**`observeEvent()`不能赋给一个变量，也不能在其他的响应表达式中使用它


