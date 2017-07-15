## R语言日常学习

| 名称      |R语言日常学习         | 
| ------------- |:-------------:| 
| 参与者1号可能也是最后一号     | 赵晓繁 | 

>今天和慧娟姐一起吃饭的时候，慧娟姐说到“哪有什么胜利可言，坚持就是一切”。突然感觉这句话就是最近的写照啊，哈哈。
一直在拖着不肯学R语言，就像小康老师说的，非要等到最后的时刻才行。感觉可不能这样下去了，**主要是到用的时候这个东西一时半会学不会啊**。
所以郑重决定（可能自己不督促自己或者没有人督促又会郑重反悔吧）要好好地尽快地学习一下R语言。

*前言*：
1. 主要根据小康老师的PDF文档学习的
2. 里面内容均为个人后续理解，可能有不对的地方，各种求共同学习。
3. 该文档主要为了加深记忆以及方便复习，毕竟我只有七秒钟记忆。

**目录**

- [dplyr包](###20170330)
- [reshape包](###20170404)
- [T检验](###20170504)


### 20170330 

今日主要学习内容*dplyr包*

[dplyr包](https://www.douban.com/note/516282666/)

 `library(dplyr)
df <- data.frame(color=c("blue","black","blue","blue","black"), value=1:5)
tbl <- tbl_df(df)
filter(tbl, color=="blue")`

这里的tbl_df是为了让数据文件更小一些，仅有的认知资源记得老师上课时这么讲的。
这一步主要是用来选取数据的，比如分为控制组和实验组，我就想要控制组的数据，谁还不是个笛宝儿了。
**注意**filter函数里面是俩等于号 *这里的==是严格等于*

* `filter(tbl, value%in%c(1, 4)`

### 20170404 

dplyr的练习和reshape2包的使用

>严重反思自己放个假就不知道学习了.今日学习积极主要原因在于,看了一天论文,实在看不下去了.遂,R语言.

上面提到的`summarise`函数，是因为我打错了标点符号,所以运行出现错误.用mtcars_tbl的数据来举例，`summarise(mtcars_tbl, min = min(mpg), max = max(mpg), mean = mean(mpg))`意识是在该表格汇总,mpg的最小值、最大值、平均值是多少.

*上课补充*  group_by(rs2015, major) %>% summarise(mean = mean(final),sd = sd(final), median = median(final), IQR = IQR(final))

以major分组，算出每组的mean sd meidan IQR


#### PDF的57页练习。

`x1 <- rnorm(1000, 5, 2)`从均值为5,标准差为2的正态分布中随机抽取1000个数

`x2 <- sample(x1, 100, replace = FALSE)`从x1中不放回抽取100个数,组成x2样本.

`x3 <- log(x2)`对x2样本取对数,我统计学的不是很好,一直不知道为啥要取对数.

`boxplot(x3, col = "blue")`画一个颜色为忧郁的蓝色的箱型图.

`rnorm(1000, 5, 2) %>% sample( 100, replace = FALSE)  %>% log  %>% boxplot( col = "blue")`当然,也可以采用%>%这种管道式的操作方式,让整个代码连贯起来,打起来也很顺溜.

#### reshape2包的使用

reshape2包的使用主要是为了将宽格式的数据转换为长格式（窄格式）。个人觉得，在excel里这种转换也很痛苦，我们要学会利用工具。哈哈。
运用reshape2的包之前,需要了解宽格式和长格式的数据.宽格式是我们常见的格式.如下图.

图一

| person  |grade  | score |
| --------- |:--------:| :-----:| 
| 王二狗   | 2 | 71 |
| 李三熊   | 2 | 72 |
| 王小芳   | 1 | 75 |

但是在R语言里,许多函数都要用长格式,如下图.

图二

| person  |variable  | value |
| ------- |:--------:| :----:| 
| 王二狗   |grade | 2 |
| 李三熊   |grade | 2 |
| 王小芳   |grade | 1 |
| 王二狗   |score | 71|
| 李三熊   |score | 72|
| 王小芳   |score | 75|

现在，让我们来正式学习reshape2包的使用。

首先，依照日常习性，建立一个名称为miao的表格。`miao <- data.frame("person" = c( "王二狗", "李三熊", "王小芳"),"grade" = c(2, 2, 1), "score" = c(71, 72, 75))`建立表格的函数还记得咩?反正我是忘了,刚刚百度了一下建立的.这里没有采用老师所用的表格,因为我们要自力更生,举一反三(主要是我觉得还得去找老师的表格,懒)

*好气,刚刚不知道点了哪里,打了好大一段,全都删除了,这可是十一点多了啊,好气好气.来继续*

其次,reshape2包里包含了`melt函数`和`dcast函数`.

* melt函数可以将宽格式的数据改为长格式的数据.举个栗子,运用`miao2 <- melt(miao, id = "person")`函数,可以实现上述表格的转换.这个函数的意思是生成一个名称为miao2的表格，person是行变量（我不是很能分清楚行变量和列变量，看上图吧。。。）

* `dcast函数`是对数据进行还原,还原为数据框格式.(不过老师也没介绍怎么个还原法,暂时先不展开)老师介绍了下分类汇总的方式.举个栗子,`dcast(miao,score~grade, margins = c("score", "grade"))`就是算一下分数在各年级上的分布,顺便求一下行列的边缘和.

reshape2包的使用暂且先到这里结束.总体感觉还不错,除了一天也就学习一个安装包的那种惭愧感在心中盘旋.......

### 20170405 tidyr包的使用
* gather函数的使用。gather函数相当于melt函数，将宽格式的数据转换成长格式的数据。`miao2 <- gather(miao, variable, value, -person)`也会出现melt函数一样的效果，如图二。函数里的-person是指不把person变成长数据.

### 20170426 T检验

### 20170504 

T检验

> 中间浪了漫长的一段时间,恍惚间已经进入五月份.自己对自己应该多些要求,少一些顺从.非天才的人,多要苦练,莫要耍小聪明,踏实一些.

T检验这一块比较简单.

假设你要做miao数据中的A列和B列，双侧检验，单侧检验是less或者more,

>t.test(miao$A, miao$B,alternative = "two.sided", conf.level = 0.99)

假设你要做配对样本

>t.test(miao$A, miao$B,paired = TRUE)

假设你想做的数据在A列中，是以B列为区分的。例如你想检验不同专业的学生成绩是否有显著差异

>t.test(A~B,data = miao)

subset函数

从表格中挑选出来一些符合条件的。从student中挑选出来符合性别为女，且年龄小于三十岁的人。也可以使用filter函数

>subset(student,Gender=="F" & Age<30 ,select=c("Name","Age"))

如果想要**合并表格**，如果有相同的列，可以直接rbinb(A,B).如果有相同的列，但是名字不一样（为什么不直接改了==），可以这样:student里面的id和score里面的sid是一种东西，就可以merge.

>result<-merge(student,score,by.x="ID",by.y="SID")

>Demo_3<-rbind(Demo_1[,c(1,2)], Demo_2[,c(1,2)])  合并两个表格中的列.

对**数据框进行去重** >unique(miao)

### 20170508

> 今天晚上困得想睡觉，不过多少还是要学习一些R才好。

主要是复习一下。因为感觉在反复地忘记。

>a[c(2,4] 访问向量中第二个和第四个元素  a[2:6] 第二到六个元素

### 20170509

**卡方检验**

第一种方法

>stock <- rbind(c(53,77), c(29,16),c(19,6))

>chisq.test(stock)

第二种方法

选出来表格中要做的两列

>x<-table(miao$A,miao$B)

>chiq.test(x)

警告信息:近似算法有可能不准 那么就要用修正性卡方检验  不能再用卡方检验

>fisher.test()

**比例差的精确检验**

>prop.test(c(x1,x2),c(n1,n2),alternative = "greater")



### 20170522

**合成表格**

>记录日期暴露了自己中间没有学习R的本质，唉。

我们可以把描述性统计量整合成一个表格
```{r}
stattable <- rbind(c("weight",mean(dataclass1$weight),sd(dataclass1$weight)),
                   c("height",mean(dataclass1$height),sd(dataclass1$height)))
library(knitr)
kable(stattable, col.names = c("变量","均值","标准差"),caption = "Table 1", align = "c")

```

kable函数中caption参数定义的是表的名字. 该函数会自动统计pdf文件中有多少个表格,并默认在表名前面加上Table1,Table2.

**画图**

plot(x轴变量,y轴变量,main="图名",xlab="横轴名称",ylab="纵轴名称",pch = 点形状, cex = 缩放倍数, type = 线点类型)

**线性回归**

R中自带了lm函数可以用于线性回归.用法:lm(y变量~x1变量+x2变量+.... , data = 变量所在的数据集), 对lm回归后的对象(fit)使用summary可以得到线性回归的各个统计量

>fit <- lm(weight ~ height, data = women)
summary(fit)

我们可以尝试加入height变量的平方项:

>fit2 <- lm(weight ~ height + I(height^2), data=women)
summary(fit2)

获取结果

我们可以用$引用符直接引用其中的参数如:

fit$coefficients[2]


### 20170524 

### jiebaR包


`library(jiebaR)
engine <- worker()
words <- "想学R语言，那就赶紧拿起手机，打开微信，关注公众号《跟着菜鸟一起学R语言》"`

`segment(words,engine)
engine<=words
`
固定词

`engine_new_word <- worker()
new_user_word(engine_new_word, c("公众号","R语言"))
segment(words,engine_new_word)`

从文本中导入

`enginer_user <- worker(user = `dictionary.txt`)
segment(words,enginer_user)`

删除停用词

`engine_s <-worker(stop_word = "stopwords.txt")
segment(words,engine_s)`

统计词频

`freq(segment(words,engine_s))`

词性标注

`qseg[words]

qseg <= words

tagger<- worker(type = "tag")
tagger <= words

提取关键字

`type = keywords
keys <- worker(type = "keywords", topn = 2)
keys<= words`

## 画图

hist（a$b,) 频率分布直方图

barplot(table(a$b) 条形图

boxplot(y~x,data = )

plot(x,y)  散点图

yarrr::pirateplot(y~x, data = )  海盗图 

*ggplot*

ggplot(data)+geom_point(aes(x = , y = )


ggplot(data)+geom_point(aes(x = , y = ,colour = )+stat_smooth(aes(x= , y = )+ facet_grid(group ~ .)

加颜色+ 加线+加分组

加颜色的时候 线条是colour 箱线图是fill 注意代码的转换

+ggthemems::theme_economist()  主题的改变

ggplot2.tidyverse.org/reference

### 奶牛问题

**合并行数不同的列**

id.T <- c(1:length(T0)) 生成序列号
 
 merge(x,y,by="id",all.x = TRUE)
 
 dplyr join 函数 
 
 left_join(d1,d2,by=”id”)
 
 full_join(d1,d2,by=”id”) 
 
 **minor问题**有某些字段的赋值问题
 library(tidyverse)
library(dplyr)
str_detect(x$id,"minor")
x<-mutate(x,minor = as.numeric(str_detect(x$id,"minor")))
table(x$minor)
count(x,minor)


### 医患数据提取字符问题

`library(readxl)
library(stringr)
library(tidyverse)
PDSurveyBasic <- read_excel("PDSurveyBasic.xlsx")
ip.location <- str_extract(PDSurveyBasic$ip, "(?<=\\().*(?=\\))") %>%
  str_split("-", n = 2, simplify = TRUE) %>%
  as_tibble %>%
  transmute(province = .[[1]], city = .[[2]])
clean.data <- select(PDSurveyBasic,-ip) %>%
  cbind(ip.location) %>%` as_tibble`
  
  第一题

nrow(PD) = 697
nrow(PD1) = 446

问卷有效率 = 445/696 = 64%

第二题

ZB<-summarise(PD1,min = min(PD1$time3),max = max(PD1$time3),mean = mean (PD1$time3))
plot(PD1$time3)
hist(PD1$time3) # 频率直方图说明其分布
  
 

