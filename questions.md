## 关于lavaan包的问题

### 目的

对医患社会情绪（积极、消极、中性）进行验证性因子分析

### 代码

    model <- 'positive=~Q7_2+Q7_4+Q7_6+Q7_14
              negative=~Q7_1+Q7_3+Q7_5+Q7_7+Q7_9+Q7_10+Q7_11+Q7_12
          neutral=~Q7_8+Q7_13'
    fit <- cfa(model, data = data_n507) 
    summary(fit,fit.measures = TRUE)


### 代码出错提示
fit那一行运行时提示：sample covariance matrix is not positive-definite

### 怎么样才会出现这种错误提示

[源代码](https://github.com/yrosseel/lavaan/blob/master/R/lav_samplestats_icov.R)

### 解决方案

[查询到的相关解决方案](https://stats.stackexchange.com/questions/134348/when-a-cfa-model-has-a-covariance-matrix-was-not-positive-definite-problem-is)

### 我的理解和疑惑

我的理解是可能数据不符合，或者说相关系数之类的有问题，但是我自己也不太清楚。就代码来看似乎是没有问题。没有特别懂解决方案里的建议是什么原理，什么意思，如何去验证，怎么解决。
