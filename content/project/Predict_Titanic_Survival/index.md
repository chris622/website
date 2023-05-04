---
date: "2023-05-03"
image:
  caption: "The Titanic"
  focal_point: 'center'
links:
- icon: database
  icon_pack: fas
  name: Rawdata
  url: https://www.kaggle.com/competitions/titanic
- icon: chart-line
  icon_pack: fas
  name: Dashboard
  url: https://liumj1998.shinyapps.io/Titanic_dashboard/?_ga=2.209434129.2071371260.1683203458-2025947915.1680703749
- icon: book
  icon_pack: fas
  name: Tutorial
  url: https://rpubs.com/Chris622/kaggle-titanic-survival

summary: Predict the survival on Titanic based on the passenger information
tags:
- Model
- Kaggle dataset
- R
title: Predict Titanic Survival
---

-   本内容为Titanic生存模型的建立，原始数据详见[Rawdata](https://www.kaggle.com/competitions/titanic)
-   可视化参见[Dashboard](https://liumj1998.shinyapps.io/Titanic_dashboard/?_ga=2.209434129.2071371260.1683203458-2025947915.1680703749)
-   详细的图文教程[Tutorial](https://rpubs.com/Chris622/kaggle-titanic-survival)

# Read data

-   采用`read.csv()`读取数据，包括训练集`train.csv`和测试集`test.csv`

<!-- -->

    library(tidyverse)
    test <- read.csv('titanic/test.csv')
    train <- read.csv('titanic/train.csv')
    # length of train set 
    Lt <- dim(train)[1]

# Data inspection

-   `bind_rows()`合并`train`和`test`，并用`glimpse()`查看数据内容

<!-- -->

    # combine train and test
    full <- bind_rows(train,test)
    # glimpse data
    glimpse(full)

-   查看缺失值
    -   `is.na()`判断是否为`na`
    -   `colSums()`对列`True`（1）的结果进行加和

<!-- -->

    colSums(is.na(full))
    #Age has 263 NA，Fare has 1 NA
    colSums(full=="")
    #Embarked has 2 empty values，Cabin has 1014 empty values

-   检查可以转换为factor的变量（查看数据唯一值的数目，数目少的说明为factor因子变量)
    -   `unique()` 提取唯一值
    -   `length()` 计算向量长度
    -   `map_dbl()` 对每个列进行计算，并返回double向量

<!-- -->

    map_dbl(full,~length(unique(.)))

-   将`'Survived','Pclass','Sex','Embarked'`变量变为因子型,用`as.factor()`进行转换

<!-- -->

    cols <- c('Survived','Pclass','Sex','Embarked')
    for (i in cols){
      full[,i] <- as.factor(full[,i])
    }

-   `str()`查看数据结构

<!-- -->

    str(full)

# Data analysis

## Pclass VS survived

-   `ggplot()+geom_bar()`绘图

<!-- -->

    ggplot(full[1:Lt,])+geom_bar(aes(x=Pclass,fill=Survived))
    ggplot(full[1:Lt,])+
      geom_bar(aes(x=Pclass,fill=Survived),position = 'fill')+
      labs(y='Frequency')#Pclass为3的生存率更高

## Sex VS survived

-   `ggplot()+geom_bar()`绘图

<!-- -->

    ggplot(full[1:Lt,])+geom_bar(aes(x=Sex,fill=Survived))
    ggplot(full[1:Lt,])+
      geom_bar(aes(x=Sex,fill=Survived),position = 'fill')+
      labs(y='Frequency')#女性的生存率更高

## Age VS survived

-   `ggplot()+geom_histgram()`绘图

<!-- -->

    ggplot(full[1:Lt,])+geom_histogram(aes(x=Age,fill=Survived))
    ggplot(full[1:Lt,])+
      geom_histogram(aes(x=Age,fill=Survived),position = 'fill')+
      labs(y='Frequency')#老人和小孩的生存率更高

## SibSp,Parch VS survived

-   `ggplot()+geom_bar()`绘图

<!-- -->

    ggplot(full[1:Lt,])+geom_bar(aes(x=SibSp,fill=Survived))
    ggplot(full[1:Lt,])+geom_bar(aes(x=Parch,fill=Survived))

-   `mutate()`创建新的变量 `Fsize`

<!-- -->

    full <- full%>%mutate(Fsize=SibSp+Parch+1)
    ggplot(full[1:Lt,])+geom_bar(aes(x=Fsize,fill=Survived))
    ggplot(full[1:Lt,])+
      geom_bar(aes(x=Fsize,fill=Survived),position = 'fill')+
      labs(y='Frequency') #2-4个家庭成员的存活率更高

-   `case_when()`根据`Fsize`分组

<!-- -->

    #categorize Fsize
    full <- full%>%mutate(FsizeG=case_when(
      Fsize==1~"single",
      Fsize>1&Fsize<5~"small",
      Fsize>=5~"large"))

    #FsizeD VS Survived
    table(full$FsizeG, full$Survived)
    ggplot(full[1:Lt,])+geom_bar(aes(x=FsizeG,fill=Survived))

## Fare VS survived

-   `ggplot()+geom_histgram()`绘图

<!-- -->

    ggplot(full[1:Lt,])+geom_histogram(aes(x=Fare,fill=Survived))
    ggplot(full[1:Lt,])+
      geom_histogram(aes(x=Fare,fill=Survived),position = 'fill')+
      labs(y='Frequency')#高票价的生存率更高

## Embarked VS survived

-   `ggplot()+geom_bar()`绘图

<!-- -->

    ggplot(full[1:Lt,])+geom_bar(aes(x=Embarked,fill=Survived))
    ggplot(full[1:Lt,])+
      geom_bar(aes(x=Embarked,fill=Survived),position = 'fill')+
      labs(y='Frequency')#看上去从C上船的生存率更高

## Dig into Names

-   `Name`变量中有对人的称谓，用`gsub()`提取出来作为因子

<!-- -->

    full <- full%>%
      mutate(Title=gsub('(.*, )|(\\..*)', '', Name))
    table(full$Title)

-   `fct_recode()`修正输入错误，`fct_lump_min()`合并少数

<!-- -->

    full$Title <- fct_recode(full$Title,Miss="Mlle",Miss='Ms',Mrs='Mme')%>%
      fct_lump_min(min=8)
    table(full$Title,full$Sex)

-   `ggplot()+geom_bar()`绘图

<!-- -->

    ggplot(full[1:Lt,])+geom_bar(aes(x=Title,fill=Survived))
    ggplot(full[1:Lt,])+
      geom_bar(aes(x=Title,fill=Survived),position = 'fill')+
      labs(y='Frequency')#生存率与title也有关系

# Missing values

-   **Data inspection**(Section
    @ref(inspection))中提到,`Fare`有1个缺失值，`Embarked`有2个缺失值，`Age`有263个缺失值，在建模前需要对缺失值进行补齐

## Fare

-   分析缺失值

<!-- -->

    full%>%filter(is.na(Fare))
    #可以看出这是Embarked=S，Pclass=3的乘客

-   `filter()`查找相同条件的乘客

<!-- -->

    same_df <- full%>%filter(Pclass==3&Embarked=="S")
    ggplot(same_df)+geom_density(aes(x=Fare))
    #查看相同乘客的票价分布

-   用相同乘客的票价中值代替

<!-- -->

    full$Fare[is.na(full$Fare)] <- median(same_df$Fare,na.rm = T)
    sum(is.na(full$Fare))

## Embarked

-   分析两个缺失值

<!-- -->

    full%>%filter(Embarked=="")
    #可以看出这两位都是Fare=80，Cabin=B28，Pclass=1的乘客

-   `filter()`查找相同条件的乘客

<!-- -->

    full%>%filter(Pclass==1&Fare==80)%>%select(Embarked)
    #根据相同条件查找可以发现，为C上船的乘客
    full$Embarked[full$Embarked==""] <- "C"
    #为两个空的Embarked赋值C
    sum(full$Embarked=="")

## Age

### 方法1：用平均值代替

    full <- full%>%mutate(Age1=Age)%>%
      replace_na(list(Age1=mean(full$Age,na.rm = T)))
    sum(is.na(full$Age1))

### 方法2: Multivariate Imputation

-   `mice`包实现了一个处理丢失数据的方法。该包为多变量缺失数据创建多个估算(替换值)。See
    more by typing `?mice`

<!-- -->

    library('mice') 
    # Set a random seed
    set.seed(123)
    var <- c("PassengerId","Survived","Name","Ticket","Cabin","Age1")
    # Perform mice imputation, excluding certain less-than-useful variables:
    mice_mod <- mice(full[, !names(full) %in% var], method='rf') 

-   比较预测结果的分布

<!-- -->

    # Save the complete output 
    mice_output <- complete(mice_mod)
    # Plot age distributions
    par(mfrow=c(1,2))
    hist(full$Age, freq=F, main='Age: Original Data', 
      col='darkgreen', ylim=c(0,0.04))
    hist(mice_output$Age, freq=F, main='Age: MICE Output', 
      col='lightgreen', ylim=c(0,0.04))

-   添加新的Age值

<!-- -->

    full <- full%>%mutate(Age2=mice_output$Age)
    sum(is.na(full$Age2))

# Build models

-   选择用来预测的变量

<!-- -->

    col <- c('Survived','Pclass','Sex','Age2','Fare',
             'SibSp','Parch','Title','FsizeG','Embarked')
    my_tr <- full[1:Lt,col]

-   将`my_tr`分为两个子集，用来对模型进行评价

<!-- -->

    set.seed(123)# Set a random seed
    ind<-sample(1:dim(my_tr)[1],500) # 取样500当训练集，剩下测试
    tr1<-my_tr[ind,] # The train set of the model
    tr2<-my_tr[-ind,] # The test set of the model

## glm model

-   `glm()`建模,方法选择`binomial`逻辑回归

<!-- -->

    # build model
    model <- glm(Survived ~.,family=binomial(link='logit'),data=tr1)
    summary(model)

-   `predict()`预测tr2

<!-- -->

    # predict tr2
    pred.train <- predict(model,tr2)
    pred.train <- ifelse(pred.train > 0.5,1,0)#预测值>0.5的赋值1（存活）

-   与tr2实际结果进行比较，并计算模型评价参数

<!-- -->

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

## Decision tree

-   `rpart()`进行决策树建模

<!-- -->

    library(rpart)
    library(rpart.plot)
    model_dt<- rpart(Survived ~.,data=tr1, method="class")
    #plot decision tree
    rpart.plot(model_dt)

-   `predict()`预测tr2

<!-- -->

    # predict tr2
    pred.train.dt <- predict(model_dt,tr2,type = "class")

-   与tr2实际结果进行比较，并计算模型评价参数

<!-- -->

    # model evaluation
    eval_model(pred=pred.train.dt,actual=tr2$Survived)

## Random Forest model

-   `randomForest()`进行决策树建模

<!-- -->

    library('randomForest') 
    # Build the model (note: not all possible variables are used)
    rf_model <- randomForest(factor(Survived) ~ .,
                             data = tr1)

-   `predict()`预测tr2

<!-- -->

    # predict tr2
    pred.train_rf <- predict(rf_model,tr2)

-   与tr2实际结果进行比较，并计算模型评价参数

<!-- -->

    # model evaluation
    eval_model(pred=pred.train_rf,actual=tr2$Survived)

-   模型误差 \# Show model error

<!-- -->

    plot(rf_model, ylim=c(0,0.36))
    legend('topright', colnames(rf_model$err.rate), col=1:3, fill=1:3)

-   因子重要性排序

<!-- -->

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

# Prediction

-   根据上述比较采用glm模型

<!-- -->

    # build model
    model <- glm(Survived ~.,family=binomial(link='logit'),data=my_tr)
    summary(model)

-   对测试集`test`进行预测

<!-- -->

    test_im<- full%>%select(all_of(col))%>%slice(Lt+1:dim(full)[1])
    pred.test <- predict(model,test_im)
    pred.test <- ifelse(pred.test > 0.5,1,0)
    res<- data.frame(test$PassengerId,pred.test)
    names(res)<-c("PassengerId","Survived")
    head(res)
    #write.csv(res,file="glm_res.csv",row.names = F)
