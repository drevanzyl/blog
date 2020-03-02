---
title: Intro to Cars
author: andre
date: '2020-02-29'
slug: intro-to-cars
categories:
  - Python
tags:
  - Demo
subtitle: ''
summary: ''
authors: []
lastmod: '2020-02-29T13:44:52-05:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

# R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

You can embed an R code chunk like this:


```r
summary(cars)
```

```
##      speed           dist       
##  Min.   : 4.0   Min.   :  2.00  
##  1st Qu.:12.0   1st Qu.: 26.00  
##  Median :15.0   Median : 36.00  
##  Mean   :15.4   Mean   : 42.98  
##  3rd Qu.:19.0   3rd Qu.: 56.00  
##  Max.   :25.0   Max.   :120.00
```

```r
fit <- lm(dist ~ speed, data = cars)
fit
```

```
## 
## Call:
## lm(formula = dist ~ speed, data = cars)
## 
## Coefficients:
## (Intercept)        speed  
##     -17.579        3.932
```

# Including Plots

You can also embed plots. See Figure \@ref(fig:pie) for example:


```r
par(mar = c(0, 1, 0, 1))
pie(
  c(280, 60, 20),
  c('Sky', 'Sunny side of pyramid', 'Shady side of pyramid'),
  col = c('#0292D8', '#F7EA39', '#C4B632'),
  init.angle = -50, border = NA
)
```

<div class="figure">
<img src="2020-02-29-intro-to-cars_files/figure-html/pie-1.png" alt="A fancy pie chart." width="672" />
<p class="caption">(\#fig:pie)A fancy pie chart.</p>
</div>
