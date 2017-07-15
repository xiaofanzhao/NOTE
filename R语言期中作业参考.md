---
title: "R语言期中作业"
output: html_document
---
# 第一题:数据输入


    prac01<- data.frame("ID" = c(1:5), 
                        "major"= c("英语", "法语", "社会工作", "应用心理学", "应用心理学"), 
                     
                        "mid" = c(83, 88, 82, 89, 86), 

                        "final" = c(85, 84, 86, 88, 86), 

                        "total"= c(84, 86, 84, 88, 86), 

                        "Male" = c("TRUE", "TRUE", "FALSE", "FALSE", "TRUE"))

# 第二题:命令练习

## (一)解释命令功能

`seq(1, 20, by = 2)`   从1到20中,间隔为2.选出数来.

`dnorm(-2, 10, 2)`   X=-2,均值等于10,标准差等于2的密度函数值

`pnorm(-2, 10, 2)`   概率值

`pnorm(2.5)-pnorm(2)` 概率值相减 求差

`rnorm(10, 20, 5)`   随机成成默认十个值

`qnorm(0.25, 10, 2`  分位数函数 quantile 

`cbind(dataA, dataB)` 横向合成 

`rbind(data, dataB)`  纵向合成

`quantile(x, probs = c(0.1, 0.6, 0.9))`  求四分位数 

`scale(x, center = T, scale = T)`  按列进行中心化 标准化  scale=FALSE时，表示对中。每个数减去平均数。

`var(x)` 求样本方差

`IQR(x)` 四分位间距 inter quartile range 

`five number summrary`  五数综合

`cor(x, y)` 做相关,cor.test(X,Y,method="")
method可以为"spearman","pearson" and "kendall",分别对应三种相关系数的计算和检验。

# 第三题

## 第一小题

读入命令：

    library(readxl)

    rs2015 <- read_excel("D:/RR/rs2015.xlsx")

(1)

    library(dplyr)

    rs2015 <- mutate(rs2015, total = mid * 0.4 + final * 0.6)

(2)

    rs2015 <- arrange(rs2015, desc(total))
    rs2015

(3)

方法一

    answer3 <- within(rs2015,{
    rank <- NA
    rank[total >= 90] <- "A"
    rank[total >= 80 & total < 90] <- "B"
    rank[total >= 70 & total < 80] <- "C"
    rank[total >= 60 & total < 70] <- "D"
    rank[total <60] <- "F"})
    answer3

方法二

    rs2015$rank[rs2015$total>= 90] = "A"
    rs2015$rank[rs2015$total>= 80 & rs2015$total < 90] = "B"
    rs2015$rank[rs2015$total>= 70 & rs2015$total < 80] = "C"
    rs2015$rank[rs2015$total>= 60 & rs2015$total < 70] = "D"
    rs2015$rank[rs2015$total < 60] = "F"`

方法三

cut函数

    library(dplyr)
    mutate(rs2015,
    rank = cut(
    total, 
    breaks = c(0,60,70,80,90,100), 
    labels = c(LETTERS[c(6, 4:1)]),
    right = FALSE,
    ordered = T
    ))
    rs2015`

(4)

    set.seed(5457)
    a = sample(nrow(rs2015), 5)
    newdata = rs2015[a,]

    sample_n(rs2015,5)

（5）

    PSYstudents <- filter(rs2015, major  == "PSY")

(6)

方法一

    answer6 <- within(rs2015,{
    psy <- NA
    psy[ rs2015$major == "PSY"] <- 1
    psy[ rs2015$major == "SOC"] <- 0
    psy[ rs2015$major == "DO"] <- 0
    psy[ rs2015$major == "SW"] <- 0
    })`

方法二

    answer6 <- within(rs2015,{
    psy <- NA
    psy[ rs2015$major == "PSY"] <- 1
    psy[ rs2015$major != "PSY"] <- 0
    })`
 
 方法三
 
    rs2015$psy[rs2015$major == "PSY"]= 1
    rs2015$psy[rs2015$major != "PSY"]= 0`
 
 比较简单的方法
 
    library(dplyr)
    rs2015<-mutate(rs2015,PSY = ifelse(major == "PSY", 1, 0))`
 
前面这个PSY是新生成的变量，可以命名成别的名字。语句意思是，如果major=PSY，则输出1，否则，0

dummies包中的dummy()函数可以把多分类变量转换成一系列虚拟变量，自行研究。

## 第二小题

（1）

     PD<-PDSurveyBasic
    library(dplyr)
    library(stringr)
    library(tidyverse)
    library(lubridate)

    PD <- mutate(PDSurveyBasic,time3 = as.numeric(str_sub(PD$time2, end = -2)))
    PD <- mutate(PD,time3 = round(time3/60))
    或者：
    str_extract(PD$time2,"[:digit:]+") %>%as.character%>% as.numeric()
    PD1 <- subset(PD,time3 >= 10 & time3 <= 60 & relationship > 1 & student != 1)
 
    nrow(PD) = 697
    nrow(PD1) = 446`
问卷有效率 = 445/696 = 64%

(2)

    ZB<-summarise(PD1,min = min(PD1$time3),max = max(PD1$time3),mean = mean (PD1$time3))
    ZB<-summarise(PD1,min = min(PD1$time3),max = max(PD1$time3),mean = mean (PD1$time3))
    plot(PD1$time3)
    hist(PD1$time3) # 频率直方图说明其分布
    
（3）

    library(readxl)
    library(stringr)
    library(tidyverse)
    PDSurveyBasic <- read_excel("PDSurveyBasic.xlsx")
    ip.location <- str_extract(PDSurveyBasic$ip, "(?<=\\().*(?=\\))") %>%
    str_split("-", n = 2, simplify = TRUE) %>%
    as_tibble %>%
    transmute(province = .[[1]], city = .[[2]])
    clean.data <- select(PDSurveyBasic,-ip) %>%
    cbind(ip.location) %>%` as_tibble

# 第四题

## 第一小题

    group_by(rs2015, major) %>% 
    summarise(mean = mean(final),sd = sd(final), median = median(final), IQR = IQR(final))

## 第二小题

（1）

    `testscore <- data.frame( "in" = c(2.0, 2.3, 1.1, -2.0, -1.9, 5.6, 2.6, 1.1, 5.6, 8.2), 
                             "out" = c(1.6, 3.1, -2.8, 0.0, 0.2, 2.9, -0.9, 3.8, 0.7, -0.1))`
 
（2）

   `t.test(testscore$in., testscore$out, alternative = "two.sided", conf.level = 0.98)`

（3）

   `t.test(testscore$in., testscore$out, alternative = "more", conf.level = 0.95)`

（4）

    `t.test(testscore$in., testscore$out, alternative = "more", conf.level = 0.95)`

# 第五题

    library(readxl)
    library(dplyr)
    library(sampling)
    RL <- read_excel("ReadingList.xlsx")
    x1 <- strata(RL, c("student"), size = c(rep(1, 10)), method = "srswor")  # 分层抽样的命令 strata(你的数据，分层依据的变量名称，各层中要抽的样本数，抽样方法)
    RL1 <- getdata(RL, x1)[, c("title", "student")]  # 选出你要的列
    x2 <- RL[!(RL$title %in% (RL1$title)),]  # 丢出去选过的
    x3 <- strata(x2, c("student"), size = c(rep(1, 10)), method = "srswor")  # 再选一回
    RL2 <- getdata(x2, x3)[, c("title", "student")]  # 得到结果


