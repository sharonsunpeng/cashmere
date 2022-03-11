# cashmere
First Chapter of Ph.D. research on the impact of Cashmere School Zone Downsizings on Housing Prices
#Load packages
```{r load packages, message=FALSE, warning=FALSE}
library(readr)
library(did)
library(fixest)
library(DRDID)
library(tidyverse)
library(plm)
library(stargazer)
library(sandwich)
library(lmtest)
library(jtools)
```


#read data
```{r}
df2012<-read_csv("df2012_20210920.csv", show_col_types = FALSE)
# set levels of timedummy variables so that Y2012 will be used as 
names(df2012)
unique(df2012$timedummy)


df2012$timedummy<-fct_relevel(df2012$timedummy,levels="Y2012","Y2013","Y2014","Y2015","Y2016","Y2017","janapr18","t1","t2")
df2012$groups<-fct_relevel(df2012$groups, levels="C","A","B","D")
#set levels of groups dummies so that Group C will be used as base



unique(df2012$groups)
# set levels of period dummies so that t0 period will be used as base
df2012$periods<-fct_relevel(df2012$periods, levels="t0","t1","t2")
df2012<-df2012%>%select(-1)
names(df2012)

# check on data errors
```{r eval=FALSE, include=FALSE}
df2012%>% group_by(salesid) %>% filter (A+B+C+D !=1)  #0
df2012%>% group_by(periods) %>% filter (t0+t1+t2 !=1) #0
df2012%>% filter(B==1)%>% count() #1236
df2012%>% filter(C==1)%>% count() #6492
df2012%>% filter(D==1)%>% count() #5412
1598+5412+6492+1236  #14738
df2012%>% filter(t0==1)%>% count() #10057
df2012%>% filter(t1==1)%>% count() #2484 
df2012%>% filter(t2==1)%>% count()  #2197
10057+2484+2197  #14738 



```
