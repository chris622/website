---
title: "R data skills"
author: "lmj"
date: "2023-05-05"
output: 
  bookdown::gitbook:
    split_by: none
---



+ 📢本内容为**R语言数据分析**的一些小技巧，**不定期更新**
+ 🎮[gitbook版本](https://rpubs.com/Chris622/R-skills)
+ 💻[website版本]()

# 目录

+ [Data transformation|数据转换](#transformation)
  + [查看缺失值数目](#count-na)
  + [查看数据中可以变成因子型的变量](#find-fct)
  + [按条件给观测分组，并定义为新变量](#case-when)
  + [将不常见因子合并为“其他”](#fct-lump)
  + [用特定数据代替原始数据中的NA](#replace-na)
+ [Data tidying|数据整理](#tidy)
+ [Data visualization|数据可视化](#viz)
+ [Others|其他](#others)
  + [将数据随机分为测试集和训练集](#sample-data)


# Data transformation|数据转换 {#transformation}
## 查看缺失值数目{#count-na}
+ `sum(is.na())`
+ `colSums(is.na())`
+ `rowSums(is.na())`

```r
X <- c(1,2,NA,4,5)
Y <- c('a','','c','','e')
Z <- c(NA,'B',NA,'D','E')
df <- data.frame(X,Y,Z,row.names = c(1,2,3,4,5))
df
##    X Y    Z
## 1  1 a <NA>
## 2  2      B
## 3 NA c <NA>
## 4  4      D
## 5  5 e    E
sum(is.na(X))
## [1] 1
sum(is.na(Z))
## [1] 2
colSums(is.na(df))
## X Y Z 
## 1 0 2
rowSums(is.na(df))
## 1 2 3 4 5 
## 1 0 2 0 0

#is.na()还可以替换成其他条件判断，如
sum(Y=='')
## [1] 2
```


## 查看数据中可以变成因子型的变量{#find-fct}
+ 查看数据唯一值的数目，数目少的说明为factor因子变量
+ `unique()` 提取唯一值
+ `length()` 计算向量长度
+ `map_dbl()` 对每个列进行计算，并返回double向量

```r
#以mtcars为例
glimpse(mtcars)
## Rows: 32
## Columns: 11
## $ mpg  <dbl> 21.0, 21.0, 22.8, 21.4, 18.7, 18.1, 14.3, 24.4, 22.8, 19.2, 17.8,…
## $ cyl  <dbl> 6, 6, 4, 6, 8, 6, 8, 4, 4, 6, 6, 8, 8, 8, 8, 8, 8, 4, 4, 4, 4, 8,…
## $ disp <dbl> 160.0, 160.0, 108.0, 258.0, 360.0, 225.0, 360.0, 146.7, 140.8, 16…
## $ hp   <dbl> 110, 110, 93, 110, 175, 105, 245, 62, 95, 123, 123, 180, 180, 180…
## $ drat <dbl> 3.90, 3.90, 3.85, 3.08, 3.15, 2.76, 3.21, 3.69, 3.92, 3.92, 3.92,…
## $ wt   <dbl> 2.620, 2.875, 2.320, 3.215, 3.440, 3.460, 3.570, 3.190, 3.150, 3.…
## $ qsec <dbl> 16.46, 17.02, 18.61, 19.44, 17.02, 20.22, 15.84, 20.00, 22.90, 18…
## $ vs   <dbl> 0, 0, 1, 1, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0,…
## $ am   <dbl> 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0,…
## $ gear <dbl> 4, 4, 4, 3, 3, 3, 3, 4, 4, 4, 4, 3, 3, 3, 3, 3, 3, 4, 4, 4, 3, 3,…
## $ carb <dbl> 4, 4, 1, 1, 2, 1, 4, 2, 2, 4, 4, 3, 3, 3, 4, 4, 4, 1, 2, 1, 1, 2,…
map_dbl(mtcars,~length(unique(.)))
##  mpg  cyl disp   hp drat   wt qsec   vs   am gear carb 
##   25    3   27   22   22   29   30    2    2    3    6
#cyl, vs, am, gear, carb 唯一值数目少，说明可以转换为因子

#采用for循环进行转换
cols <- c('cyl', 'vs', 'am', 'gear', 'carb' )
for (i in cols){
  mtcars[,i] <- as.factor(mtcars[,i])
}
#查看转换后的数据
glimpse(mtcars)
## Rows: 32
## Columns: 11
## $ mpg  <dbl> 21.0, 21.0, 22.8, 21.4, 18.7, 18.1, 14.3, 24.4, 22.8, 19.2, 17.8,…
## $ cyl  <fct> 6, 6, 4, 6, 8, 6, 8, 4, 4, 6, 6, 8, 8, 8, 8, 8, 8, 4, 4, 4, 4, 8,…
## $ disp <dbl> 160.0, 160.0, 108.0, 258.0, 360.0, 225.0, 360.0, 146.7, 140.8, 16…
## $ hp   <dbl> 110, 110, 93, 110, 175, 105, 245, 62, 95, 123, 123, 180, 180, 180…
## $ drat <dbl> 3.90, 3.90, 3.85, 3.08, 3.15, 2.76, 3.21, 3.69, 3.92, 3.92, 3.92,…
## $ wt   <dbl> 2.620, 2.875, 2.320, 3.215, 3.440, 3.460, 3.570, 3.190, 3.150, 3.…
## $ qsec <dbl> 16.46, 17.02, 18.61, 19.44, 17.02, 20.22, 15.84, 20.00, 22.90, 18…
## $ vs   <fct> 0, 0, 1, 1, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0,…
## $ am   <fct> 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0,…
## $ gear <fct> 4, 4, 4, 3, 3, 3, 3, 4, 4, 4, 4, 3, 3, 3, 3, 3, 3, 4, 4, 4, 3, 3,…
## $ carb <fct> 4, 4, 1, 1, 2, 1, 4, 2, 2, 4, 4, 3, 3, 3, 4, 4, 4, 1, 2, 1, 1, 2,…
```
## 按条件给观测分组，并定义为新变量{#case-when}
+ `case_when()`按条件分组
+ `mutate()`定义新变量

```r
# 以starwars数据为例
head(starwars)
## # A tibble: 6 × 14
##   name      height  mass hair_color skin_color eye_color birth_year sex   gender
##   <chr>      <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> <chr> 
## 1 Luke Sky…    172    77 blond      fair       blue            19   male  mascu…
## 2 C-3PO        167    75 <NA>       gold       yellow         112   none  mascu…
## 3 R2-D2         96    32 <NA>       white, bl… red             33   none  mascu…
## 4 Darth Va…    202   136 none       white      yellow          41.9 male  mascu…
## 5 Leia Org…    150    49 brown      light      brown           19   fema… femin…
## 6 Owen Lars    178   120 brown, gr… light      blue            52   male  mascu…
## # ℹ 5 more variables: homeworld <chr>, species <chr>, films <list>,
## #   vehicles <list>, starships <list>
# 按照身高height，体重mass，种类species给人物分类
starwars %>%
  select(name:mass, gender, species) %>%
  mutate(
    type = case_when(
      height > 200 | mass > 200 ~ "large",#大型物种
      species == "Droid" ~ "robot",#机器人
      .default = "other"#其他
    )
  )
## # A tibble: 87 × 6
##    name               height  mass gender    species type 
##    <chr>               <int> <dbl> <chr>     <chr>   <chr>
##  1 Luke Skywalker        172    77 masculine Human   other
##  2 C-3PO                 167    75 masculine Droid   robot
##  3 R2-D2                  96    32 masculine Droid   robot
##  4 Darth Vader           202   136 masculine Human   large
##  5 Leia Organa           150    49 feminine  Human   other
##  6 Owen Lars             178   120 masculine Human   other
##  7 Beru Whitesun lars    165    75 feminine  Human   other
##  8 R5-D4                  97    32 masculine Droid   robot
##  9 Biggs Darklighter     183    84 masculine Human   other
## 10 Obi-Wan Kenobi        182    77 masculine Human   other
## # ℹ 77 more rows
```




## 将不常见因子合并为“其他”{#fct-lump}
+ fct_lump_min():合并出现次数少于min的级别。
+ fct_lump_prop():将频率少于(或等于)prop的级别合并。
+ fct_lump_n():保留n个最频繁的(如果n<0，合并-n个最不频繁的)
+ fct_lump_lowfreq()将频率最低的级别集中在一起，并确保“other”仍然是最小的级别。

```r
x <- factor(rep(LETTERS[1:9], times = c(40, 10, 5, 27, 1, 1, 1, 1, 1)))
x %>% table()
## .
##  A  B  C  D  E  F  G  H  I 
## 40 10  5 27  1  1  1  1  1
#保留3个最频繁的
x %>%
  fct_lump_n(3) %>%
  table()
## .
##     A     B     D Other 
##    40    10    27    10
# 保留频率>0.10的
x %>%
  fct_lump_prop(0.10) %>%
  table()
## .
##     A     B     D Other 
##    40    10    27    10
# 保留频次>=5的
x %>%
  fct_lump_min(5) %>%
  table()
## .
##     A     B     C     D Other 
##    40    10     5    27     5
# 将频率最低的级别集中在一起，并确保“other”仍然是最小的级别。
x %>%
  fct_lump_lowfreq() %>%
  table()
## .
##     A     D Other 
##    40    27    20
```


## 用特定数据代替原始数据中的NA{#replace-na}
+ `replace_na()`

```r
# 替换成unknown
df <- tibble(x = c(1, 2, NA), y = c("a", NA, "b"))
df %>% replace_na(list(x = 0, y = "unknown"))
## # A tibble: 3 × 2
##       x y      
##   <dbl> <chr>  
## 1     1 a      
## 2     2 unknown
## 3     0 b
# 替换成平均值
df <- tibble(ID = c(1:6), age = c(20,24,NA,23,25,27))
df %>% replace_na(list(age=mean(df$age,na.rm = T)))
## # A tibble: 6 × 2
##      ID   age
##   <int> <dbl>
## 1     1  20  
## 2     2  24  
## 3     3  23.8
## 4     4  23  
## 5     5  25  
## 6     6  27
```

# Data tidying|数据整理 {#tidy}

# Data visualization|数据可视化 {#viz}
# Others|其他{#others}
## 将数据随机分为测试集和训练集{#sample-data}

```r
set.seed(123)# Set a random seed
ind<-sample(1:dim(data)[1],500) # 取样500当训练集，剩下测试
train<-data[ind,] # The train set of the model
test<-data[-ind,] # The test set of the model
```

