---
title: "Social Survey Data Analysis Report"
author: "Kunyu He"
date: "11 Aug 2018"
output:
  html_document:
    keep_md: true
  pdf_document:
    fig_caption: yes
    keep_tex: yes
    number_sections: yes
  word_document: default
---




```r
setwd("D:/My Documents/Senior Year/Social Survey")
if(!file.exists("social media.csv")){
        download.file("https://github.com/QuinninR/QuinninR-sample-analysis/blob/master/Data/social%20media.csv", "social media.csv")}

library(readr)
library(dplyr)
media <- tbl_df(read_csv("social media.csv"))
```


```r
glimpse(media)
```

```
## Observations: 307
## Variables: 40
## $ order             <int> 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 2...
## $ gender            <int> 2, 2, 1, 1, 1, 2, 1, 2, 1, 2, 2, 2, 2, 2, 2,...
## $ home              <int> 2, 2, 3, 2, 2, 1, 1, 2, 2, 3, 1, 2, 1, 2, 1,...
## $ university        <int> 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1,...
## $ major             <int> 1, 1, 1, 1, 1, 1, 1, 4, 1, 1, 1, 1, 1, 1, 1,...
## $ before.university <int> 1, 1, 3, 1, 1, 1, 1, 3, 1, 2, 1, 1, 1, 1, 1,...
## $ post.university   <int> 2, 3, 3, 2, 2, 1, 2, 3, 3, 2, 3, 2, 3, 1, 2,...
## $ qq.time           <int> 1, 2, 2, 1, 4, 4, 3, 1, 4, 1, 2, 2, 3, 4, 1,...
## $ wechat.time       <int> 3, 2, 3, 3, 4, 3, 4, 3, 4, 4, 2, 4, 4, 2, 2,...
## $ qq.strangers      <int> -3, -3, -3, -3, -3, 4, -3, -3, -3, -3, -3, -...
## $ qq.friends        <int> -3, -3, -3, -3, -3, 6, -3, -3, -3, -3, -3, -...
## $ qq.useful         <int> -3, -3, -3, -3, -3, 6, -3, -3, -3, -3, -3, -...
## $ qq.facilities     <int> -3, -3, -3, -3, -3, 7, -3, -3, -3, -3, -3, -...
## $ qq.knowledge      <int> -3, -3, -3, -3, -3, 7, -3, -3, -3, -3, -3, -...
## $ qq.easy           <int> -3, -3, -3, -3, -3, 7, -3, -3, -3, -3, -3, -...
## $ qq.privacy        <int> -3, -3, -3, -3, -3, 5, -3, -3, -3, -3, -3, -...
## $ qq.enjoyful       <int> -3, -3, -3, -3, -3, 5, -3, -3, -3, -3, -3, -...
## $ qq.habit          <int> -3, -3, -3, -3, -3, 6, -3, -3, -3, -3, -3, -...
## $ qq.social         <int> -3, -3, -3, -3, -3, 4, -3, -3, -3, -3, -3, -...
## $ qq.social.2       <int> -3, -3, -3, -3, -3, 5, -3, -3, -3, -3, -3, -...
## $ qq.important      <int> -3, -3, -3, -3, -3, 5, -3, -3, -3, -3, -3, -...
## $ qq.good           <int> -3, -3, -3, -3, -3, 5, -3, -3, -3, -3, -3, -...
## $ wechat.strangers  <int> 1, -3, -3, 6, 4, -3, 5, -3, -3, 4, -3, 1, -3...
## $ wechat.friends    <int> 6, -3, -3, 6, 6, -3, 5, -3, -3, 4, -3, 5, -3...
## $ wechat.useful     <int> 6, -3, -3, 7, 7, -3, 7, -3, -3, 5, -3, 6, -3...
## $ wechat.facilities <int> 7, -3, -3, 7, 7, -3, 7, -3, -3, 6, -3, 6, -3...
## $ wechat.knowledge  <int> 7, -3, -3, 7, 7, -3, 7, -3, -3, 6, -3, 6, -3...
## $ wechat.easy       <int> 7, -3, -3, 6, 7, -3, 7, -3, -3, 5, -3, 6, -3...
## $ wechat.privacy    <int> 2, -3, -3, 4, 6, -3, 3, -3, -3, 5, -3, 5, -3...
## $ wechat.enjoyful   <int> 4, -3, -3, 6, 7, -3, 6, -3, -3, 6, -3, 5, -3...
## $ wechat.habit      <int> 6, -3, -3, 6, 7, -3, 7, -3, -3, 7, -3, 5, -3...
## $ wechat.social     <int> 6, -3, -3, 7, 4, -3, 5, -3, -3, 4, -3, 4, -3...
## $ wechat.social.2   <int> 6, -3, -3, 5, 4, -3, 4, -3, -3, 3, -3, 4, -3...
## $ wechat.important  <int> 6, -3, -3, 6, 7, -3, 7, -3, -3, 4, -3, 4, -3...
## $ wechat.good       <int> 4, -3, -3, 6, 6, -3, 6, -3, -3, 4, -3, 5, -3...
## $ same.function     <int> -3, 0, 0, -3, -3, -3, -3, 1, 1, -3, 1, -3, 1...
## $ same.useful       <int> -3, 1, 1, -3, -3, -3, -3, 0, 1, -3, 0, -3, 1...
## $ same.social       <int> -3, 0, 0, -3, -3, -3, -3, 0, 1, -3, 0, -3, 1...
## $ same.doNotCare    <int> -3, 0, 0, -3, -3, -3, -3, 0, 1, -3, 0, -3, 0...
## $ same.others       <int> -3, 0, 0, -3, -3, -3, -3, 0, 0, -3, 0, -3, 0...
```


```r
# set order to track records
rownames(media) <- media$order
media <- media %>%
        select(-order)

# replace all -3 with NAs
library(naniar)

media <- media %>% 
        replace_with_na_all(condition = ~.x == -3)

# split the dataset for different analysis available
incomlete <- media %>%
        filter(post.university == 3) %>%
        select(-c(qq.strangers : wechat.good))

complete <- media %>%
        filter(post.university != 3) %>%
        select(-c(same.function : same.others))
```


```r
library(tidyr)

complete[is.na(complete)] <- ""
complete <- complete %>%
        unite("strangers", qq.strangers, wechat.strangers, sep = "") %>%
        unite("friends", qq.friends, wechat.friends, sep = "") %>%
        unite("useful", qq.useful, wechat.useful, sep = "") %>%
        unite("facilities", qq.facilities, wechat.facilities, sep = "") %>%
        unite("knowledge", qq.knowledge, wechat.knowledge, sep = "") %>%
        unite("easy", qq.easy, wechat.easy, sep = "") %>%
        unite("privacy", qq.privacy, wechat.privacy, sep = "") %>%
        unite("enjoyful", qq.enjoyful, wechat.enjoyful, sep = "") %>%
        unite("habit", qq.habit, wechat.habit, sep = "") %>%
        unite("social", qq.social, wechat.social, sep = "") %>%
        unite("social.2", qq.social.2, wechat.social.2, sep = "") %>%
        unite("important", qq.important, wechat.important, sep = "") %>%
        unite("good", qq.good, wechat.good, sep = "") %>%
        mutate(preference = ifelse(post.university == 1, qq.time, wechat.time)) %>%
        apply(2, as.numeric) %>%
        tbl_df()
```






```r
pca <- complete %>%
        select(strangers:good)

matrix(c(colMeans(pca), apply(pca, 2, sd)), byrow = F, ncol = 2, 
       dimnames = list(colnames(pca), c("Mean", "Standard Deviation")))
```

               Mean Standard Deviation
strangers  3.743842          1.1787722
friends    5.665025          1.1672547
useful     5.206897          1.1198266
facilities 6.472906          0.5477270
knowledge  6.453202          0.6065052
easy       5.502463          0.6322550
privacy    2.684729          0.8199935
enjoyful   5.463054          1.3831964
habit      6.059113          1.3557553
social     5.285714          0.7624497
social.2   5.285714          0.7360201
important  4.852217          0.9053376
good       5.546798          0.5899548


```r
library(gridExtra)
library(ggpubr)
library(FactoMineR)
```

```
## Warning: package 'FactoMineR' was built under R version 3.5.1
```

```r
library(factoextra)
```

```
## Warning: package 'factoextra' was built under R version 3.5.1
```

```r
library(scales)

pr.out <- prcomp(pca, center = T, scale = T, tol = 0.5)

# Variability of each principal component: pr.var
pr.var <- pr.out$sdev^2

# Variance explained by each principal component: pve
pve <- pr.var / sum(pr.var)

fig1 <- fviz_eig(pr.out, addlabels = TRUE, ylim = c(0, 50), 
                 barcolor = "darkgreen", barfill = "skyblue") + 
        geom_hline(aes(yintercept = 10), color = "red", linetype = 2, lwd = 1) + 
        labs(x = "Pricipal Components",
             y = "Percentage of Variances Explained",
             title = "(Scree Plot)") +
        theme_bw() +
        theme(plot.title = element_text(hjust = 0.5, vjust = 1, size = 12, face = "italic"))

# Plot cumulative proportion of variance explained
fig2 <- ggplot(tbl_df(pve), aes(x = 1:13, y = cumsum(value))) + 
        geom_line() + 
        geom_point() + 
        geom_text(aes(label = paste(round(cumsum(value), digits = 4) * 100, "%", sep = "")),
                  hjust = 0, vjust = 1.2) + 
        geom_hline(aes(yintercept = 0.75), color = "red", linetype = 2, lwd = 1) + 
        labs(x = "Pricipal Components",
             y = "Cumulative Proportion of Variance Explained",
             title = "(Cumulative Scree Plot)") + 
        scale_x_continuous(breaks = 1:13) + 
        scale_y_continuous(breaks = seq(0.3, 1, by = 0.05), labels = percent_format()) +
        theme_bw() +
        theme(plot.title = element_text(hjust = 0.5, vjust = 1, size = 12, face = "italic"))

main = text_grob("Figure : Scree Plots of Variance Explained",
                 size = 15, face = "bold", hjust = 0.5, vjust = 0.2)

grid.arrange(fig1, fig2, ncol = 2, top = main)
```

![](Report_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
ggsave("Scree Plots of Variance Explained.png", device = 'png', 
       width = 14, height = 6, dpi = 900, 
       plot = grid.arrange(fig1, fig2, ncol = 2, top = main)) 
```


```r
fig.3 <- fviz_pca_var(pr.out, col.var = "cos2",
             gradient.cols = c("#00AFBB", "#E7B800", "#FC4E07"), 
             repel = TRUE) +  
        labs(x = "Principal Component 1: Sociability (31.0%)",
             y = "Principal Component 2: Accessbility (24.1%)",
             title = "Figure : Correlation Circle of Predictors") + 
        theme_bw() + 
        theme(plot.title = element_text(hjust = 0.5, vjust = 1,
                                        size = 14, face = "bold"))

fig.3
```

![](Report_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

```r
ggsave("Correlation Circle of Predictors.png", device = 'png', 
       width = 8, height = 7, dpi = 900, 
       plot = fig.3) 
```


```r
library(corrplot)
library(RColorBrewer)

cos2 <- get_pca_var(pr.out)$cos2

png("Quality of Representation.png", width = 900, height = 800)
corrplot(cos2, is.corr = FALSE, method = "color", 
         col= brewer.pal(n = 16, name = "RdYlBu"), outline = "black",
         tl.col = "black", tl.srt = 45)
dev.off()
```

```
## png 
##   2
```

```r
corrplot(cos2, is.corr = FALSE, method = "color", 
         col= brewer.pal(n = 16, name = "RdYlBu"), outline = "black",
         tl.col = "black", tl.srt = 45)
```

![](Report_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


```r
summary(pr.out)
```

```
## Importance of first k=4 (out of 13) components:
##                           PC1    PC2    PC3    PC4
## Standard deviation     2.0088 1.7709 1.3372 1.1932
## Proportion of Variance 0.3104 0.2412 0.1375 0.1095
## Cumulative Proportion  0.3104 0.5516 0.6892 0.7987
```




```r
pr.out$rotation
```

```
## # A tibble: 13 x 4
##         PC1    PC2      PC3      PC4
##       <dbl>  <dbl>    <dbl>    <dbl>
##  1  0.411   0.214  -0.178    0.0140 
##  2  0.390   0.254  -0.0872   0.00746
##  3  0.414   0.146  -0.0916   0.0277 
##  4 -0.103   0.499   0.219    0.174  
##  5 -0.105   0.487   0.270    0.124  
##  6 -0.0969  0.513   0.00216  0.172  
##  7 -0.00210 0.156  -0.622   -0.0651 
##  8 -0.00205 0.140   0.0640  -0.689  
##  9  0.0953  0.200   0.0649  -0.614  
## 10 -0.409   0.103  -0.140   -0.0246 
## 11 -0.422   0.0901 -0.157    0.0149 
## 12 -0.342   0.0812 -0.0787  -0.248  
## 13 -0.0616  0.0942 -0.621    0.0699
```



```r
pca_rotated <- tbl_df(pr.out$x)[, 1:4]

complete <- complete %>%
        select(-c(qq.time:good)) %>%
        cbind(pca_rotated) %>%
        mutate(gender = factor(gender, labels= c("Male", "Female")),
               major = factor(major, labels = c("Humanities or Social Sciences",
                                                "Business",
                                                "Science",
                                                "Engeneering",
                                                "Arts, Sports or Others")),
               type.preference = factor(ifelse(post.university == before.university,
                                               "consistent",
                                               "new"),
                                        levels = c("consistent", "new")),
               type.location = factor(ifelse(university == home,
                                             "same",
                                             "moved"), levels = c("same", "moved")),
               before.university = factor(before.university, labels = c("QQ",
                                                                        "Wechat",
                                                                        "No difference")),
               post.university = factor(post.university, labels = c("QQ",
                                                                    "Wechat")),
               home = factor(home, labels = c("East", "Mid", "West")),
               university = factor(university, labels = c("East", "Mid", "West"))
               )
```




```r
f1 <- preference ~ gender + major + home + university + before.university + post.university
lm1 <- lm(f1, data  = complete)
summary(lm1)
```

```
## 
## Call:
## lm(formula = f1, data = complete)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -2.5032 -0.6021  0.2759  0.6669  1.3745 
## 
## Coefficients:
##                                 Estimate Std. Error t value
## (Intercept)                     3.090281   0.191563  16.132
## genderFemale                    0.420365   0.142350   2.953
## majorBusiness                   0.331890   0.207187   1.602
## majorScience                   -0.384517   0.187452  -2.051
## majorEngeneering               -0.272341   0.212182  -1.284
## majorArts, Sports or Others    -0.457318   0.326315  -1.401
## homeMid                         0.095194   0.159653   0.596
## homeWest                       -0.019859   0.179161  -0.111
## universityMid                  -0.178024   0.212261  -0.839
## universityWest                 -0.143863   0.319320  -0.451
## before.universityWechat        -0.034541   0.221845  -0.156
## before.universityNo difference -0.028156   0.258861  -0.109
## post.universityWechat          -0.007469   0.157312  -0.047
##                                            Pr(>|t|)    
## (Intercept)                    < 0.0000000000000002 ***
## genderFemale                                0.00354 ** 
## majorBusiness                               0.11084    
## majorScience                                0.04161 *  
## majorEngeneering                            0.20087    
## majorArts, Sports or Others                 0.16271    
## homeMid                                     0.55171    
## homeWest                                    0.91186    
## universityMid                               0.40269    
## universityWest                              0.65284    
## before.universityWechat                     0.87644    
## before.universityNo difference              0.91350    
## post.universityWechat                       0.96218    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.9429 on 190 degrees of freedom
## Multiple R-squared:  0.1332,	Adjusted R-squared:  0.07851 
## F-statistic: 2.434 on 12 and 190 DF,  p-value: 0.005774
```




```r
f2 <- preference ~ gender + major + type.preference + type.location
lm2 <- lm(f2, data  = complete)
summary(lm2)
```

```
## 
## Call:
## lm(formula = f2, data = complete)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -2.3774 -0.3616  0.1576  0.5047  1.6384 
## 
## Coefficients:
##                             Estimate Std. Error t value
## (Intercept)                  3.69544    0.14173  26.074
## genderFemale                 0.30778    0.12325   2.497
## majorBusiness                0.09198    0.17652   0.521
## majorScience                -0.31807    0.15607  -2.038
## majorEngeneering            -0.41884    0.17687  -2.368
## majorArts, Sports or Others -0.35562    0.27943  -1.273
## type.preferencenew          -0.48080    0.13735  -3.501
## type.locationmoved          -0.53493    0.13833  -3.867
##                                         Pr(>|t|)    
## (Intercept)                 < 0.0000000000000002 ***
## genderFemale                            0.013344 *  
## majorBusiness                           0.602917    
## majorScience                            0.042893 *  
## majorEngeneering                        0.018863 *  
## majorArts, Sports or Others             0.204653    
## type.preferencenew                      0.000575 ***
## type.locationmoved                      0.000150 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.8288 on 195 degrees of freedom
## Multiple R-squared:  0.3127,	Adjusted R-squared:  0.288 
## F-statistic: 12.67 on 7 and 195 DF,  p-value: 0.0000000000002273
```


```r
f3 <- preference ~ gender + major + type.preference + type.location + PC1
lm3 <- lm(f3, data  = complete)
summary(lm3)
```

```
## 
## Call:
## lm(formula = f3, data = complete)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -2.28793 -0.19565  0.01383  0.24519  0.87990 
## 
## Coefficients:
##                             Estimate Std. Error t value
## (Intercept)                  3.42610    0.07675  44.639
## genderFemale                 0.11401    0.06648   1.715
## majorBusiness                0.02744    0.09443   0.291
## majorScience                -0.09715    0.08404  -1.156
## majorEngeneering            -0.14113    0.09540  -1.479
## majorArts, Sports or Others -0.04798    0.15005  -0.320
## type.preferencenew          -0.19487    0.07457  -2.613
## type.locationmoved          -0.29968    0.07472  -4.011
## PC1                          0.37581    0.01701  22.094
##                                         Pr(>|t|)    
## (Intercept)                 < 0.0000000000000002 ***
## genderFemale                             0.08792 .  
## majorBusiness                            0.77168    
## majorScience                             0.24912    
## majorEngeneering                         0.14067    
## majorArts, Sports or Others              0.74949    
## type.preferencenew                       0.00967 ** 
## type.locationmoved                     0.0000864 ***
## PC1                         < 0.0000000000000002 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.4431 on 194 degrees of freedom
## Multiple R-squared:  0.8045,	Adjusted R-squared:  0.7965 
## F-statistic:  99.8 on 8 and 194 DF,  p-value: < 0.00000000000000022
```


```r
f4 <- preference ~ gender + major + type.preference + type.location + PC1 + PC2
lm4 <- lm(f4, data  = complete)
summary(lm4)
```

```
## 
## Call:
## lm(formula = f4, data = complete)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -2.50787 -0.23882  0.04055  0.27513  0.81557 
## 
## Coefficients:
##                             Estimate Std. Error t value
## (Intercept)                  3.42647    0.07353  46.600
## genderFemale                 0.10395    0.06373   1.631
## majorBusiness               -0.01844    0.09109  -0.202
## majorScience                -0.11269    0.08059  -1.398
## majorEngeneering            -0.14620    0.09140  -1.599
## majorArts, Sports or Others -0.05171    0.14375  -0.360
## type.preferencenew          -0.15070    0.07218  -2.088
## type.locationmoved          -0.31197    0.07164  -4.355
## PC1                          0.37912    0.01631  23.239
## PC2                          0.07380    0.01722   4.286
##                                         Pr(>|t|)    
## (Intercept)                 < 0.0000000000000002 ***
## genderFemale                              0.1045    
## majorBusiness                             0.8398    
## majorScience                              0.1636    
## majorEngeneering                          0.1113    
## majorArts, Sports or Others               0.7195    
## type.preferencenew                        0.0381 *  
## type.locationmoved                     0.0000216 ***
## PC1                         < 0.0000000000000002 ***
## PC2                                    0.0000287 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.4245 on 193 degrees of freedom
## Multiple R-squared:  0.8215,	Adjusted R-squared:  0.8132 
## F-statistic:  98.7 on 9 and 193 DF,  p-value: < 0.00000000000000022
```


```r
f5 <- preference ~ gender + major + type.preference + type.location + PC1 + PC2 + PC3
lm5 <- lm(f5, data  = complete)
summary(lm5)
```

```
## 
## Call:
## lm(formula = f5, data = complete)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -2.48320 -0.23753  0.03918  0.27774  0.80054 
## 
## Coefficients:
##                             Estimate Std. Error t value
## (Intercept)                  3.43095    0.07434  46.154
## genderFemale                 0.10217    0.06398   1.597
## majorBusiness               -0.01661    0.09137  -0.182
## majorScience                -0.11674    0.08125  -1.437
## majorEngeneering            -0.14419    0.09170  -1.572
## majorArts, Sports or Others -0.05085    0.14406  -0.353
## type.preferencenew          -0.15492    0.07291  -2.125
## type.locationmoved          -0.31417    0.07195  -4.366
## PC1                          0.37862    0.01638  23.109
## PC2                          0.07357    0.01726   4.262
## PC3                          0.01056    0.02319   0.456
##                                         Pr(>|t|)    
## (Intercept)                 < 0.0000000000000002 ***
## genderFemale                              0.1119    
## majorBusiness                             0.8559    
## majorScience                              0.1524    
## majorEngeneering                          0.1175    
## majorArts, Sports or Others               0.7245    
## type.preferencenew                        0.0349 *  
## type.locationmoved                     0.0000206 ***
## PC1                         < 0.0000000000000002 ***
## PC2                                    0.0000317 ***
## PC3                                       0.6493    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.4254 on 192 degrees of freedom
## Multiple R-squared:  0.8217,	Adjusted R-squared:  0.8124 
## F-statistic: 88.48 on 10 and 192 DF,  p-value: < 0.00000000000000022
```


```r
f6 <- preference ~ gender + major + type.preference + type.location + PC1 + PC2 + PC3 + PC4
lm6 <- lm(f6, data  = complete)
summary(lm6)
```

```
## 
## Call:
## lm(formula = f6, data = complete)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -2.48966 -0.24095  0.02513  0.27974  0.79675 
## 
## Coefficients:
##                             Estimate Std. Error t value
## (Intercept)                  3.42893    0.07430  46.149
## genderFemale                 0.10680    0.06406   1.667
## majorBusiness               -0.01839    0.09131  -0.201
## majorScience                -0.12051    0.08125  -1.483
## majorEngeneering            -0.15404    0.09204  -1.674
## majorArts, Sports or Others -0.05599    0.14402  -0.389
## type.preferencenew          -0.15515    0.07286  -2.129
## type.locationmoved          -0.30879    0.07205  -4.285
## PC1                          0.37851    0.01637  23.120
## PC2                          0.07365    0.01725   4.269
## PC3                          0.01008    0.02317   0.435
## PC4                          0.02873    0.02531   1.135
##                                         Pr(>|t|)    
## (Intercept)                 < 0.0000000000000002 ***
## genderFemale                              0.0971 .  
## majorBusiness                             0.8406    
## majorScience                              0.1397    
## majorEngeneering                          0.0958 .  
## majorArts, Sports or Others               0.6979    
## type.preferencenew                        0.0345 *  
## type.locationmoved                     0.0000289 ***
## PC1                         < 0.0000000000000002 ***
## PC2                                    0.0000308 ***
## PC3                                       0.6640    
## PC4                                       0.2577    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.4251 on 191 degrees of freedom
## Multiple R-squared:  0.8229,	Adjusted R-squared:  0.8127 
## F-statistic: 80.68 on 11 and 191 DF,  p-value: < 0.00000000000000022
```



