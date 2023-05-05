---
author: "lmj"
date: "2023-05-03"
output:
  bookdown::gitbook:
    split_by: none
  md_document:
    standalone: yes
title: 'Kaggle: Titanic Survival'
---


+ 本内容为Titanic生存模型的建立，原始数据详见[Kaggle](https://www.kaggle.com/competitions/titanic)
+ 可视化参见[Dashboard: Titanic Survival](https://liumj1998.shinyapps.io/Titanic_dashboard/?_ga=2.209434129.2071371260.1683203458-2025947915.1680703749)
+ 详细的图文教程[My Rpubs: Titanic Survival](https://rpubs.com/Chris622/kaggle-titanic-survival)

# Read data
+ 采用`read.csv()`读取数据，包括训练集`train.csv`和测试集`test.csv`

```r
library(tidyverse)
test <- read.csv('titanic/test.csv')
train <- read.csv('titanic/train.csv')
# length of train set 
Lt <- dim(train)[1]
```

# Data inspection {#inspection}
+ `bind_rows()`合并`train`和`test`，并用`glimpse()`查看数据内容

```r
# combine train and test
full <- bind_rows(train,test)
# glimpse data
glimpse(full)
```

```
## Rows: 1,309
## Columns: 12
## $ PassengerId <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17,…
## $ Survived    <int> 0, 1, 1, 1, 0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 1…
## $ Pclass      <int> 3, 1, 3, 1, 3, 3, 1, 3, 3, 2, 3, 1, 3, 3, 3, 2, 3, 2, 3, 3…
## $ Name        <chr> "Braund, Mr. Owen Harris", "Cumings, Mrs. John Bradley (Fl…
## $ Sex         <chr> "male", "female", "female", "female", "male", "male", "mal…
## $ Age         <dbl> 22, 38, 26, 35, 35, NA, 54, 2, 27, 14, 4, 58, 20, 39, 14, …
## $ SibSp       <int> 1, 1, 0, 1, 0, 0, 0, 3, 0, 1, 1, 0, 0, 1, 0, 0, 4, 0, 1, 0…
## $ Parch       <int> 0, 0, 0, 0, 0, 0, 0, 1, 2, 0, 1, 0, 0, 5, 0, 0, 1, 0, 0, 0…
## $ Ticket      <chr> "A/5 21171", "PC 17599", "STON/O2. 3101282", "113803", "37…
## $ Fare        <dbl> 7.2500, 71.2833, 7.9250, 53.1000, 8.0500, 8.4583, 51.8625,…
## $ Cabin       <chr> "", "C85", "", "C123", "", "", "E46", "", "", "", "G6", "C…
## $ Embarked    <chr> "S", "C", "S", "S", "S", "Q", "S", "S", "S", "C", "S", "S"…
```

+ 查看缺失值
  + `is.na()`判断是否为`na`
  + `colSums()`对列`True`（1）的结果进行加和

```r
colSums(is.na(full))
```

```
## PassengerId    Survived      Pclass        Name         Sex         Age 
##           0         418           0           0           0         263 
##       SibSp       Parch      Ticket        Fare       Cabin    Embarked 
##           0           0           0           1           0           0
```

```r
#Age has 263 NA，Fare has 1 NA
colSums(full=="")
```

```
## PassengerId    Survived      Pclass        Name         Sex         Age 
##           0          NA           0           0           0          NA 
##       SibSp       Parch      Ticket        Fare       Cabin    Embarked 
##           0           0           0          NA        1014           2
```

```r
#Embarked has 2 empty values，Cabin has 1014 empty values
```

+ 检查可以转换为factor的变量（查看数据唯一值的数目，数目少的说明为factor因子变量)
  + `unique()` 提取唯一值
  + `length()` 计算向量长度
  + `map_dbl()` 对每个列进行计算，并返回double向量

```r
map_dbl(full,~length(unique(.)))
```

```
## PassengerId    Survived      Pclass        Name         Sex         Age 
##        1309           3           3        1307           2          99 
##       SibSp       Parch      Ticket        Fare       Cabin    Embarked 
##           7           8         929         282         187           4
```

+ 将`'Survived','Pclass','Sex','Embarked'`变量变为因子型,用`as.factor()`进行转换

```r
cols <- c('Survived','Pclass','Sex','Embarked')
for (i in cols){
  full[,i] <- as.factor(full[,i])
}
```

+ `str()`查看数据结构

```r
str(full)
```

```
## 'data.frame':	1309 obs. of  12 variables:
##  $ PassengerId: int  1 2 3 4 5 6 7 8 9 10 ...
##  $ Survived   : Factor w/ 2 levels "0","1": 1 2 2 2 1 1 1 1 2 2 ...
##  $ Pclass     : Factor w/ 3 levels "1","2","3": 3 1 3 1 3 3 1 3 3 2 ...
##  $ Name       : chr  "Braund, Mr. Owen Harris" "Cumings, Mrs. John Bradley (Florence Briggs Thayer)" "Heikkinen, Miss. Laina" "Futrelle, Mrs. Jacques Heath (Lily May Peel)" ...
##  $ Sex        : Factor w/ 2 levels "female","male": 2 1 1 1 2 2 2 2 1 1 ...
##  $ Age        : num  22 38 26 35 35 NA 54 2 27 14 ...
##  $ SibSp      : int  1 1 0 1 0 0 0 3 0 1 ...
##  $ Parch      : int  0 0 0 0 0 0 0 1 2 0 ...
##  $ Ticket     : chr  "A/5 21171" "PC 17599" "STON/O2. 3101282" "113803" ...
##  $ Fare       : num  7.25 71.28 7.92 53.1 8.05 ...
##  $ Cabin      : chr  "" "C85" "" "C123" ...
##  $ Embarked   : Factor w/ 4 levels "","C","Q","S": 4 2 4 4 4 3 4 4 4 2 ...
```

# Data analysis    
## Pclass VS survived
+ `ggplot()+geom_bar()`绘图

```r
ggplot(full[1:Lt,])+geom_bar(aes(x=Pclass,fill=Survived))
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-8-1.png" width="672" style="display: block; margin: auto;" />

```r
ggplot(full[1:Lt,])+
  geom_bar(aes(x=Pclass,fill=Survived),position = 'fill')+
  labs(y='Frequency')#Pclass为3的生存率更高
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-8-2.png" width="672" style="display: block; margin: auto;" />

## Sex VS survived
+ `ggplot()+geom_bar()`绘图

```r
ggplot(full[1:Lt,])+geom_bar(aes(x=Sex,fill=Survived))
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-9-1.png" width="672" style="display: block; margin: auto;" />

```r
ggplot(full[1:Lt,])+
  geom_bar(aes(x=Sex,fill=Survived),position = 'fill')+
  labs(y='Frequency')#女性的生存率更高
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-9-2.png" width="672" style="display: block; margin: auto;" />



## Age VS survived
+ `ggplot()+geom_histgram()`绘图

```r
ggplot(full[1:Lt,])+geom_histogram(aes(x=Age,fill=Survived))
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-10-1.png" width="672" style="display: block; margin: auto;" />

```r
ggplot(full[1:Lt,])+
  geom_histogram(aes(x=Age,fill=Survived),position = 'fill')+
  labs(y='Frequency')#老人和小孩的生存率更高
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-10-2.png" width="672" style="display: block; margin: auto;" />



## SibSp,Parch VS survived

+ `ggplot()+geom_bar()`绘图

```r
ggplot(full[1:Lt,])+geom_bar(aes(x=SibSp,fill=Survived))
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-11-1.png" width="672" style="display: block; margin: auto;" />

```r
ggplot(full[1:Lt,])+geom_bar(aes(x=Parch,fill=Survived))
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-11-2.png" width="672" style="display: block; margin: auto;" />

+ `mutate()`创建新的变量 `Fsize`

```r
full <- full%>%mutate(Fsize=SibSp+Parch+1)
ggplot(full[1:Lt,])+geom_bar(aes(x=Fsize,fill=Survived))
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-12-1.png" width="672" style="display: block; margin: auto;" />

```r
ggplot(full[1:Lt,])+
  geom_bar(aes(x=Fsize,fill=Survived),position = 'fill')+
  labs(y='Frequency') #2-4个家庭成员的存活率更高
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-12-2.png" width="672" style="display: block; margin: auto;" />

+ `case_when()`根据`Fsize`分组

```r
#categorize Fsize
full <- full%>%mutate(FsizeG=case_when(
  Fsize==1~"single",
  Fsize>1&Fsize<5~"small",
  Fsize>=5~"large"))

#FsizeD VS Survived
table(full$FsizeG, full$Survived)
```

```
##         
##            0   1
##   large   52  10
##   single 374 163
##   small  123 169
```

```r
ggplot(full[1:Lt,])+geom_bar(aes(x=FsizeG,fill=Survived))
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-13-1.png" width="672" style="display: block; margin: auto;" />


## Fare VS survived
+ `ggplot()+geom_histgram()`绘图

```r
ggplot(full[1:Lt,])+geom_histogram(aes(x=Fare,fill=Survived))
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-14-1.png" width="672" style="display: block; margin: auto;" />

```r
ggplot(full[1:Lt,])+
  geom_histogram(aes(x=Fare,fill=Survived),position = 'fill')+
  labs(y='Frequency')#高票价的生存率更高
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-14-2.png" width="672" style="display: block; margin: auto;" />


## Embarked VS survived
+ `ggplot()+geom_bar()`绘图

```r
ggplot(full[1:Lt,])+geom_bar(aes(x=Embarked,fill=Survived))
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-15-1.png" width="672" style="display: block; margin: auto;" />

```r
ggplot(full[1:Lt,])+
  geom_bar(aes(x=Embarked,fill=Survived),position = 'fill')+
  labs(y='Frequency')#看上去从C上船的生存率更高
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-15-2.png" width="672" style="display: block; margin: auto;" />

## Dig into Names
+ `Name`变量中有对人的称谓，用`gsub()`提取出来作为因子

```r
full <- full%>%
  mutate(Title=gsub('(.*, )|(\\..*)', '', Name))
table(full$Title)
```

```
## 
##         Capt          Col          Don         Dona           Dr     Jonkheer 
##            1            4            1            1            8            1 
##         Lady        Major       Master         Miss         Mlle          Mme 
##            1            2           61          260            2            1 
##           Mr          Mrs           Ms          Rev          Sir the Countess 
##          757          197            2            8            1            1
```
+ `fct_recode()`修正输入错误，`fct_lump_min()`合并少数

```r
full$Title <- fct_recode(full$Title,Miss="Mlle",Miss='Ms',Mrs='Mme')%>%
  fct_lump_min(min=8)
table(full$Title,full$Sex)
```

```
##         
##          female male
##   Dr          1    7
##   Master      0   61
##   Miss      264    0
##   Mrs       198    0
##   Mr          0  757
##   Rev         0    8
##   Other       3   10
```
+ `ggplot()+geom_bar()`绘图

```r
ggplot(full[1:Lt,])+geom_bar(aes(x=Title,fill=Survived))
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-18-1.png" width="672" style="display: block; margin: auto;" />

```r
ggplot(full[1:Lt,])+
  geom_bar(aes(x=Title,fill=Survived),position = 'fill')+
  labs(y='Frequency')#生存率与title也有关系
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-18-2.png" width="672" style="display: block; margin: auto;" />



# Missing values  
+  **Data inspection**(Section <a href="#inspection">2</a>)中提到,`Fare`有1个缺失值，`Embarked`有2个缺失值，`Age`有263个缺失值，在建模前需要对缺失值进行补齐

## Fare
+ 分析缺失值

```r
full%>%filter(is.na(Fare))
```

```
##   PassengerId Survived Pclass               Name  Sex  Age SibSp Parch Ticket
## 1        1044     <NA>      3 Storey, Mr. Thomas male 60.5     0     0   3701
##   Fare Cabin Embarked Fsize FsizeG Title
## 1   NA              S     1 single    Mr
```

```r
#可以看出这是Embarked=S，Pclass=3的乘客
```
+ `filter()`查找相同条件的乘客

```r
same_df <- full%>%filter(Pclass==3&Embarked=="S")
ggplot(same_df)+geom_density(aes(x=Fare))
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-20-1.png" width="672" style="display: block; margin: auto;" />

```r
#查看相同乘客的票价分布
```

+ 用相同乘客的票价中值代替

```r
full$Fare[is.na(full$Fare)] <- median(same_df$Fare,na.rm = T)
sum(is.na(full$Fare))
```

```
## [1] 0
```

## Embarked
+ 分析两个缺失值

```r
full%>%filter(Embarked=="")
```

```
##   PassengerId Survived Pclass                                      Name    Sex
## 1          62        1      1                       Icard, Miss. Amelie female
## 2         830        1      1 Stone, Mrs. George Nelson (Martha Evelyn) female
##   Age SibSp Parch Ticket Fare Cabin Embarked Fsize FsizeG Title
## 1  38     0     0 113572   80   B28              1 single  Miss
## 2  62     0     0 113572   80   B28              1 single   Mrs
```

```r
#可以看出这两位都是Fare=80，Cabin=B28，Pclass=1的乘客
```

+ `filter()`查找相同条件的乘客

```r
full%>%filter(Pclass==1&Fare==80)%>%select(Embarked)
```

```
##   Embarked
## 1         
## 2
```

```r
#根据相同条件查找可以发现，为C上船的乘客
full$Embarked[full$Embarked==""] <- "C"
#为两个空的Embarked赋值C
sum(full$Embarked=="")
```

```
## [1] 0
```

## Age  
### 方法1：用平均值代替

```r
full <- full%>%mutate(Age1=Age)%>%
  replace_na(list(Age1=mean(full$Age,na.rm = T)))
sum(is.na(full$Age1))
```

```
## [1] 0
```

### 方法2: Multivariate Imputation

+ `mice`包实现了一个处理丢失数据的方法。该包为多变量缺失数据创建多个估算(替换值)。See more by typing `?mice`

```r
library('mice') 
# Set a random seed
set.seed(123)
var <- c("PassengerId","Survived","Name","Ticket","Cabin","Age1")
# Perform mice imputation, excluding certain less-than-useful variables:
mice_mod <- mice(full[, !names(full) %in% var], method='rf') 
```

```
## 
##  iter imp variable
##   1   1  Age
##   1   2  Age
##   1   3  Age
##   1   4  Age
##   1   5  Age
##   2   1  Age
##   2   2  Age
##   2   3  Age
##   2   4  Age
##   2   5  Age
##   3   1  Age
##   3   2  Age
##   3   3  Age
##   3   4  Age
##   3   5  Age
##   4   1  Age
##   4   2  Age
##   4   3  Age
##   4   4  Age
##   4   5  Age
##   5   1  Age
##   5   2  Age
##   5   3  Age
##   5   4  Age
##   5   5  Age
```
+ 比较预测结果的分布

```r
# Save the complete output 
mice_output <- complete(mice_mod)
# Plot age distributions
par(mfrow=c(1,2))
hist(full$Age, freq=F, main='Age: Original Data', 
  col='darkgreen', ylim=c(0,0.04))
hist(mice_output$Age, freq=F, main='Age: MICE Output', 
  col='lightgreen', ylim=c(0,0.04))
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-26-1.png" width="672" style="display: block; margin: auto;" />
+ 添加新的Age值

```r
full <- full%>%mutate(Age2=mice_output$Age)
sum(is.na(full$Age2))
```

```
## [1] 0
```


# Build models
+ 选择用来预测的变量

```r
col <- c('Survived','Pclass','Sex','Age2','Fare',
         'SibSp','Parch','Title','FsizeG','Embarked')
my_tr <- full[1:Lt,col]
```

+ 将`my_tr`分为两个子集，用来对模型进行评价

```r
set.seed(123)# Set a random seed
ind<-sample(1:dim(my_tr)[1],500) # 取样500当训练集，剩下测试
tr1<-my_tr[ind,] # The train set of the model
tr2<-my_tr[-ind,] # The test set of the model
```

## glm model

+ `glm()`建模,方法选择`binomial`逻辑回归

```r
# build model
model <- glm(Survived ~.,family=binomial(link='logit'),data=tr1)
summary(model)
```

```
## 
## Call:
## glm(formula = Survived ~ ., family = binomial(link = "logit"), 
##     data = tr1)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -2.6656  -0.5742  -0.4146   0.5376   2.4065  
## 
## Coefficients:
##                Estimate Std. Error z value Pr(>|z|)    
## (Intercept)   1.279e+01  1.455e+03   0.009  0.99299    
## Pclass2      -9.407e-01  4.335e-01  -2.170  0.02998 *  
## Pclass3      -2.013e+00  4.284e-01  -4.699 2.62e-06 ***
## Sexmale      -1.646e+01  1.455e+03  -0.011  0.99098    
## Age2         -2.014e-02  1.187e-02  -1.698  0.08960 .  
## Fare          2.799e-03  2.931e-03   0.955  0.33953    
## SibSp         6.623e-02  2.826e-01   0.234  0.81471    
## Parch         2.550e-01  2.755e-01   0.925  0.35473    
## TitleMaster   4.215e+00  1.411e+00   2.987  0.00282 ** 
## TitleMiss    -1.288e+01  1.455e+03  -0.009  0.99294    
## TitleMrs     -1.214e+01  1.455e+03  -0.008  0.99335    
## TitleMr       7.793e-01  1.217e+00   0.640  0.52210    
## TitleRev     -1.316e+01  6.416e+02  -0.021  0.98364    
## TitleOther    7.811e-01  1.495e+00   0.522  0.60138    
## FsizeGsingle  3.488e+00  1.445e+00   2.415  0.01576 *  
## FsizeGsmall   2.789e+00  1.099e+00   2.537  0.01117 *  
## EmbarkedQ     1.541e-01  5.187e-01   0.297  0.76646    
## EmbarkedS    -4.095e-01  3.379e-01  -1.212  0.22557    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 665.03  on 499  degrees of freedom
## Residual deviance: 413.91  on 482  degrees of freedom
## AIC: 449.91
## 
## Number of Fisher Scoring iterations: 14
```

+ `predict()`预测tr2

```r
# predict tr2
pred.train <- predict(model,tr2)
pred.train <- ifelse(pred.train > 0.5,1,0)#预测值>0.5的赋值1（存活）
```

+ 与tr2实际结果进行比较，并计算模型评价参数

```r
# model evaluation
eval_model <- function(pred,actual){
  accuracy <- mean(pred==actual)
  t1<-table(pred,actual)
  #计算Presicion and recall of the model
  presicion<- t1[1,1]/(sum(t1[1,]))
  recall<- t1[1,1]/(sum(t1[,1]))
  # F1 score
  F1<- 2*presicion*recall/(presicion+recall)
  return(c(accuracy=accuracy,presicion=presicion,recall=recall,F1=F1))
}

eval_model(pred=pred.train,actual=tr2$Survived)
```

```
##  accuracy presicion    recall        F1 
## 0.8439898 0.8429119 0.9166667 0.8782435
```

## Decision tree

+ `rpart()`进行决策树建模

```r
library(rpart)
library(rpart.plot)
model_dt<- rpart(Survived ~.,data=tr1, method="class")
#plot decision tree
rpart.plot(model_dt)
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-33-1.png" width="672" style="display: block; margin: auto;" />

+ `predict()`预测tr2

```r
# predict tr2
pred.train.dt <- predict(model_dt,tr2,type = "class")
```

+ 与tr2实际结果进行比较，并计算模型评价参数

```r
# model evaluation
eval_model(pred=pred.train.dt,actual=tr2$Survived)
```

```
##  accuracy presicion    recall        F1 
## 0.8363171 0.8308271 0.9208333 0.8735178
```

## Random Forest model

+ `randomForest()`进行决策树建模

```r
library('randomForest') 
# Build the model (note: not all possible variables are used)
rf_model <- randomForest(factor(Survived) ~ .,
                         data = tr1)
```

+ `predict()`预测tr2

```r
# predict tr2
pred.train_rf <- predict(rf_model,tr2)
```

+ 与tr2实际结果进行比较，并计算模型评价参数

```r
# model evaluation
eval_model(pred=pred.train_rf,actual=tr2$Survived)
```

```
##  accuracy presicion    recall        F1 
## 0.8363171 0.8520000 0.8875000 0.8693878
```
+ 模型误差
# Show model error

```r
plot(rf_model, ylim=c(0,0.36))
legend('topright', colnames(rf_model$err.rate), col=1:3, fill=1:3)
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-39-1.png" width="672" style="display: block; margin: auto;" />

+ 因子重要性排序

```r
# Get importance
importance    <- importance(rf_model)
varImportance <- data.frame(Variables =row.names(importance), 
                            Importance = round(importance[ ,'MeanDecreaseGini'],2))

# Create a rank variable based on importance
rankImportance <- varImportance %>%
  mutate(Rank = paste0('#',dense_rank(desc(Importance))))

# Use ggplot2 to visualize the relative importance of variables
ggplot(rankImportance, aes(x = reorder(Variables, Importance), 
                           y = Importance, fill = Importance)) +
  geom_bar(stat='identity') + 
  geom_text(aes(x = Variables, y = 0.5, label = Rank),
            hjust=0, vjust=0.55, size = 4, colour = 'red') +
  labs(x = 'Variables') +
  coord_flip() + theme_bw()
```

<img src="/project/Titanic/titanic_survival_files/figure-html/unnamed-chunk-40-1.png" width="672" style="display: block; margin: auto;" />


# Prediction
+ 根据上述比较采用glm模型

```r
# build model
model <- glm(Survived ~.,family=binomial(link='logit'),data=my_tr)
summary(model)
```

```
## 
## Call:
## glm(formula = Survived ~ ., family = binomial(link = "logit"), 
##     data = my_tr)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -2.6559  -0.5312  -0.3858   0.5537   2.4554  
## 
## Coefficients:
##                Estimate Std. Error z value Pr(>|z|)    
## (Intercept)   13.390078 832.882746   0.016 0.987173    
## Pclass2       -1.030105   0.334831  -3.076 0.002095 ** 
## Pclass3       -2.011818   0.330193  -6.093 1.11e-09 ***
## Sexmale      -16.014531 832.881850  -0.019 0.984659    
## Age2          -0.018786   0.008789  -2.137 0.032562 *  
## Fare           0.004196   0.002717   1.544 0.122550    
## SibSp         -0.081662   0.212386  -0.385 0.700607    
## Parch          0.083796   0.215611   0.389 0.697541    
## TitleMaster    3.654575   1.079147   3.387 0.000708 ***
## TitleMiss    -13.138823 832.881965  -0.016 0.987414    
## TitleMrs     -12.547047 832.881995  -0.015 0.987981    
## TitleMr       -0.063341   0.922960  -0.069 0.945286    
## TitleRev     -13.863025 590.412881  -0.023 0.981267    
## TitleOther     0.180695   1.176505   0.154 0.877936    
## FsizeGsingle   3.017866   1.096069   2.753 0.005899 ** 
## FsizeGsmall    2.700415   0.836758   3.227 0.001250 ** 
## EmbarkedQ      0.009528   0.402754   0.024 0.981126    
## EmbarkedS     -0.298624   0.255843  -1.167 0.243123    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 1186.66  on 890  degrees of freedom
## Residual deviance:  711.85  on 873  degrees of freedom
## AIC: 747.85
## 
## Number of Fisher Scoring iterations: 14
```

+ 对测试集`test`进行预测

```r
test_im<- full%>%select(all_of(col))%>%slice(Lt+1:dim(full)[1])
pred.test <- predict(model,test_im)
pred.test <- ifelse(pred.test > 0.5,1,0)
res<- data.frame(test$PassengerId,pred.test)
names(res)<-c("PassengerId","Survived")
head(res)
```

```
##   PassengerId Survived
## 1         892        0
## 2         893        0
## 3         894        0
## 4         895        0
## 5         896        1
## 6         897        0
```

```r
#write.csv(res,file="glm_res.csv",row.names = F)
```










