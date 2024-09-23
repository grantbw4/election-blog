---
title: Week Three - Polls
author: 'Grant Williams'
date: '2024-09-23'
slug: week-three-polls
categories: []
tags: []
---

# Introduction

In this third blog post, I want to explore **how we can *best* use polls to predict election outcomes.** I will do this through the following steps:

## 1) First, I am going to visualize poll accuracy over time.

## 2) Next, I will use various *regularization techniques* to create a model that will predict the 2024 election.

## 3) Then, I will use *ensemble learning* methods to combine models that incorporate both fundamental (economic) and polling data.

## 4) Lastly, I will discuss G. Elliott Morris and Nate Silver's contrasting approaches when it comes to weighting fundamental and poll-based forecasts.

The code used to produce these visualizations is publicly available in my [github repository](https://github.com/grantbw4/election-blog) and draws heavily from the section notes and sample code provided by the Gov 1347 Head Teaching Fellow, [Matthew Dardet](https://www.matthewdardet.com/harvard-election-analytics-2024/).

# Analysis 

# Citations:

Achen, C. H., & Bartels, L. M. (2016). Democracy for realists: *why elections do not produce responsive government.* Princeton University Press.

# Data Sources: 

Data are from the US presidential election popular vote results from 1948-2020, [FiveThirtyEight](https://projects.fivethirtyeight.com/polls/), and [the Federal Reserve Bank of St. Louis](https://fred.stlouisfed.org/).



# Poll averages in 2020







<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-2.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-3.png" width="672" />

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-2.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-3.png" width="672" />
# Coefficient Plot


```
## [1] 9.575001
```

```
## [1] 4.642786
```

```
## [1] 2.325853
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="672" />

# Polling for 2024







# Elastic Net using only Fundamental Data


```
##            s1
## [1,] 51.23438
## [2,] 47.63135
```


```
##            s1
## [1,] 50.57824
## [2,] 47.98181
```

```
##            s1
## [1,] 51.51353
## [2,] 49.14507
```

```
## [1] 0.8556624
```

```
## [1] 0.1443376
```

```
##            s1
## [1,] 51.71210
## [2,] 50.22182
```

```
## [1] 0.1443376
```

```
## [1] 0.8556624
```

```
##            s1
## [1,] 51.31497
## [2,] 48.06832
```
