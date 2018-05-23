---
title: "�߼����ͳ��ѧ ��������ҵ"
author: "������"
date: "2018��5��23��"
output:
        prettydoc::html_pretty:
          theme: architect
          highlight: github
          keep_md: true
          pandoc_args: [
                 "--number-sections",
          ]
---



# ׼�������ݴ���

�����ݶ��빤��������������������ݴ���Ϳ��ӻ��������


```r
ratings <- read.csv('ratings.csv')

if(!require(plyr)){install.packages('plyr')}
if(!require(dplyr)){install.packages('dplyr')}
if(!require(ggplot2)){install.packages('ggplot2')}
if(!require(ggpubr)){install.packages('ggpubr')}
if(!require(conover.test)){install.packages('conover.test')}
```

���ȴ����ǵ������������ȡ10�������Կ������ݽṹ��


```r
set.seed(1234)
dplyr::sample_n(ratings, 10)
```

```
##    customer rate      city
## 7         A   90  Brisbane
## 33        D   68 Melbourne
## 32        D   65 Melbourne
## 52        F   65  Brisbane
## 44        E   84  Brisbane
## 51        F   64 Melbourne
## 1         A   75    Sydney
## 11        B   68    Sydney
## 31        D   60 Melbourne
## 24        C   60 Melbourne
```

���Կ�����������Ҫ������Ƿ���������ع˿����(`customer`)�ͳ���(`city`)��������(`rate`)��Ӱ�졣��ʱ����Ӧ�ÿ������ǵ������Ƿ���ƽ��(balanced)�ġ���`customer`��`city`��������������ÿһ���µ����������Ƿ���ȣ�����ȣ������Ƿ�������Ķ�����ƽ�����(balanced design)��


```r
with(ratings, table(customer, city))
```

```
##         city
## customer Brisbane Melbourne Sydney
##        A        3         3      3
##        B        3         3      3
##        C        3         3      3
##        D        3         3      3
##        E        3         3      3
##        F        3         3      3
```

# ̽�������ݷ���

���Կ��������ǵ�����������ƽ��ġ�ͨ����`customer`Ϊ���������`city`Ϊ���������`rate`Ϊ�����������ͼ�����ǵ��Գ������ӻ�����ƽ������ڳ��м��������͹˿͸�������ڲ��졣


```r
ggboxplot(ratings, x = "city", y = "rate", color = "customer")
```

![](Homework3_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

ͬʱ��Ϊ�˿����������������Ӱ�����ֵ�ͬʱ�Ƿ���ڽ���ЧӦ�������Գ���Ϊ����������÷�Ϊ����������˿����Ϊ׷�ٱ���(trace variable)���ƽ���ЧӦͼ��


```r
ggline(ratings, x = "city", y = "rate", color = "customer",
       add = c("mean_se", "dotplot"))
```

```
## `stat_bindot()` using `bins = 30`. Pick better value with `binwidth`.
```

![](Homework3_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

�����Ǵ���ͼ�й۲쵽���Ե�ƽ�����ֲ��컹�ǽ���ͼ�н�����ߣ����Ƕ���Ϊ����������������к͹˿���ݣ���������ֲ���Ӱ�졣Ҳ����˵������Ԥ����������ж����������������˿Ͳ��������ڲ����������

# ͳ�Ʒ���(Two-way ANOVA for Balanced Design)

����̽�����о����õ��Ľ����Ǵ��Եģ�����Ӧ�ý�һ��ʹ�÷����������֤��һ�ƶϡ�


```r
anova <- aov(rate ~ city + customer, data = ratings)
summary(anova)
```

```
##             Df Sum Sq Mean Sq F value      Pr(>F)    
## city         2   1512   756.2   24.17 0.000000067 ***
## customer     5   1795   359.1   11.47 0.000000319 ***
## Residuals   46   1440    31.3                        
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

������ķ����������У����ǿ��Է��ֹ˿�����ͬʱ�ܵ�����������к͹˿���ݵ�Ӱ�죬���Ͻ���˵���˿����־�ֵ���ɳ��к͹˿���ݽ���綨�ķ����е������������ڲ��졣

��Ȼ�����ǽ��ɽ���ͼ���ַ������������н���ЧӦ������Ӧ������ģ���п��ǽ���ЧӦ��


```r
anova2 <- aov(rate ~ city*customer, data = ratings)
summary(anova2)
```

```
##               Df Sum Sq Mean Sq F value             Pr(>F)    
## city           2   1512   756.2   84.55 0.0000000000000250 ***
## customer       5   1795   359.1   40.14 0.0000000000000953 ***
## city:customer 10   1118   111.8   12.49 0.0000000055531288 ***
## Residuals     36    322     8.9                               
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

���ѷ��֣���0.05��������ˮƽ�£�����������佻������������������Ӱ�졣��Ҳ��һ��˵���˳��ж������ֵ�ЧӦ�ܵ��˿͵�Ӱ�죬Ҳ����˵���˿͵������жϻ�Ӱ�������֣����ֲ�����ȫ�ɸ������ǰͿ˵�ˮƽ���������

# ���飺����Ƚ��Լ���������������

## ����Ƚ�

���Ƿ����������ʾ������ֵ����Ĵ��ڣ���û��˵������Щ��֮�䲻ͬ��Ϊ��̽�����������ʹ��Tukey Honest Significant Differences��


```r
TukeyHSD(anova2, which = "city")
```

```
##   Tukey multiple comparisons of means
##     95% family-wise confidence level
## 
## Fit: aov(formula = rate ~ city * customer, data = ratings)
## 
## $city
##                          diff         lwr       upr     p adj
## Melbourne-Brisbane -12.388889 -14.8256302 -9.952148 0.0000000
## Sydney-Brisbane     -9.500000 -11.9367413 -7.063259 0.0000000
## Sydney-Melbourne     2.888889   0.4521476  5.325630 0.0170733
```

���ַ����ڿ��ǹ˿͵����ڲ����������������Ľ���ЧӦ��ǰ���£��Ը����е����־�ֵ���������Ƚϡ�**��ʱ���Ƿ���Ϥ��Ͳ���˹�࣬ī�����Ͳ���˹�����־�ֵ���������죬��ī������Ϥ���ǰͿ˵ĵ÷־����ڲ���˹�ࡣ**

__ע������ͬʱ����������������飬���ؼ������Ӧ�ö�$p-value$�����������˴�����ʹ��[Bonferroni correction](https://en.wikipedia.org/wiki/Bonferroni_correction)����У��������ԭ$p$ֵ���Լ����������õ��µ�pֵ��__

**Ϥ���ī������������$p$ֵУ���󳬹�0.05���ж�Ϥ���ī����������֮��û���������죨��Ȼ���ܹ�����Bonferroni correction���ڱ��أ���**

����ͬ���ķ�������ƽ��������˿���ݵ������졣


```r
TukeyHSD(anova2, which = "customer")
```

```
##   Tukey multiple comparisons of means
##     95% family-wise confidence level
## 
## Fit: aov(formula = rate ~ city * customer, data = ratings)
## 
## $customer
##           diff         lwr          upr     p adj
## B-A  -8.444444 -12.6860593  -4.20282962 0.0000102
## C-A -17.333333 -21.5749482 -13.09171850 0.0000000
## D-A  -6.111111 -10.3527259  -1.86949628 0.0014620
## E-A  -3.222222  -7.4638371   1.01939261 0.2262870
## F-A -12.666667 -16.9082815  -8.42505184 0.0000000
## C-B  -8.888889 -13.1305037  -4.64727406 0.0000039
## D-B   2.333333  -1.9082815   6.57494816 0.5690490
## E-B   5.222222   0.9806074   9.46383705 0.0085583
## F-B  -4.222222  -8.4638371   0.01939261 0.0516396
## D-C  11.222222   6.9806074  15.46383705 0.0000000
## E-C  14.111111   9.8694963  18.35272594 0.0000000
## F-C   4.666667   0.4250518   8.90828150 0.0239713
## E-D   2.888889  -1.3527259   7.13050372 0.3360457
## F-D  -6.555556 -10.7971704  -2.31394073 0.0005817
## F-E  -9.444444 -13.6860593  -5.20282962 0.0000012
```

ͨ���ϱ�$p$ֵУ�����֣�

* ���ع˿�1�������������ڳ������ع˿�5����������˿ͣ�
* ���ع˿�2�����ֽ������������ع˿�3�������ع˿�4��5û���������죻
* ���ع˿�4�����������������ع˿�6��
* ���ع˿�5�����������������ع˿�6

## ��������������

���Է�������ļ�����м��飬�Լ������Ƿ������ڲ�Ч�ȡ�

���ȼ��鷽�����ԣ�Ӧ����[Levene��s test](https://en.wikipedia.org/wiki/Levene%27s_test)��


```r
library(car)
leveneTest(rate ~ city*customer, data = ratings)
```

```
## Levene's Test for Homogeneity of Variance (center = median)
##       Df F value Pr(>F)
## group 17  0.4286 0.9677
##       36
```

__��ʱLevene����Ľ����������˵�����鷽��û���������죬�뷽���Լ�����0.05��������û�б�Υ����__

ͬʱҲ���Բ��ÿ��ӻ�������顣


```r
plot(anova2, 1)
```

![](Homework3_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

�����й��������۲챻���Ϊ�쳣ֵ���ֱ�Ϊ�۲�ֵ20, 23��24����ȡ����Щ�۲�ֵ���й۲졣


```r
ratings[c(20, 23, 24),]
```

```
##    customer rate      city
## 20        C   60    Sydney
## 23        C   50 Melbourne
## 24        C   60 Melbourne
```

__���Ƿ�����Щ�����ع˿�3��Ϥ���ī���������δ�֣�Ϊ�˻�ø��õķ���Ч��������Ӧ����ɾȥ�������쳣ֵ��__

�ٽ�һ��������̬�ֲ������Ƿ���������Ȳ���Q-Qͼ���ӻ���


```r
plot(anova2, 2)
```

![](Homework3_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

���Ƶأ����Ǽ��ͬ���������쳣ֵ���۲�ֵ20, 23��24�������������ݻ���������̬�ֲ���

��󣬿��Խ�һ��ʹ��Shapiro-Wilk test�������Ƿ�������Ĳв


```r
residuals <- residuals(object = anova2)

shapiro.test(x = residuals)
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  residuals
## W = 0.98265, p-value = 0.6197
```

__����������������̬�ֲ�������0.05��������û�б�Υ����__

__�������������������綨�ĸ����й۲�ֵ������С��5��Ϊ3�������������������Լ��衣���Ϸ���������Ч��__

# ����

**��ͬ���ع˿ͶԲ�ͬ���е��������������졣������������У��������Ժ���̬�ֲ��Լ�������������������Լ��費�������������������Ч��**
