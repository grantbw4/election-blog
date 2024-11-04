---
title: 'Week 6: Bayesian Approach'
author: Grant Williams
date: '2024-10-12'
slug: week-6-bayesian-approach
categories: []
tags: []
---
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />

# Introduction

In this sixth blog post, I am going to discuss the role that **ads** play in elections, and, then, I will discuss how a **Frequentist** approach to polling compares to a **Bayesian** approach.

I will also be updating my model from last week.

*The code used to produce these visualizations is publicly available in my [github repository](https://github.com/grantbw4/election-blog) and draws heavily from the section notes and sample code provided by the Gov 1347 Head Teaching Fellow, [Matthew Dardet](https://www.matthewdardet.com/harvard-election-analytics-2024/).*

# Analysis

After reading that the Harris campaign had reached over [$1 billion in campaign donations,](https://www.cbsnews.com/news/kamala-harris-campaign-fundraising-1-billion/), I did a deep dive into campaign advertising throughout history.

By using data from the [Wesleyan Media Project](https://mediaproject.wesleyan.edu/), I was able to visualize the *tone* of television advertisements for the presidential elections between 2000 and 2012.





<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

As is visible in the graph above, the election years between 2000 and 2012 saw a variety of tones within advertisements. The 2012 election cylce appeared to be pretty heated given the high incidences of "attack[ing]" tones among both candidates' advertisements (as classified by the Wesleyan Media Project).

I will now prepare another visualization of the *content* of political advertisements from the same source, this time including 2016. Publicly available data only exists up until 2012, so I am using non-public data to provide the estimates for 2016.

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

Immediately striking from this graph is the high incidence of "personal" content from the Democratic aisle in 2016. [Many have noted](https://www.brookings.edu/articles/why-hillary-clinton-lost/) this as the Clinton campaign's most significant mistake: her insistence on criticizing the language, behavior, and character of Trump to voters at the potential expense of clearly articulating and evidencing her policy positions

The following graph explores the 2012 election and, for a variety of topics, the breakdown of the percentage of ads discussing those topics aired by each party. 

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

Though this election took place in 2012 in a pre-MAGA America, many of the basic dynamics between the Democratic and Republican parties still remain. For example, Republicans remain more likely to air ads on crime and Democrats more likely to air ads on child care, though it is interesting that immigration ads appear evenly split between both parties — a subject that has become much more partisan and racially charged since 2012. 

Now, I am going to prepare two more graphs that evaluate campaigns' election spending.

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-2.png" width="672" />

From these two graphs, we can observe that campaigns spend immense amounts of money on advertising and that this expense only increases as the election date nears. As reported by [*Open Secrets*](www.opensecrets.org/news/2021/02/2020-cycle-cost-14p4-billion-doubling-16/), TV ads are the single-largest expense of presidential campaigns, and the cost of presidential elections has only ballooned in recent cycles. The cost of the 2020 presidential election was near $5.7 billion (*Open Secrets*). The bulk of this spending is also concentrated in more competitive swing states. 

Given the sheer volume of money that is spent on presidential elections, I am interested in constructing a regression to measure if there is any statistically significant relationship between campaign spending and two-party vote share. I will focus on the Democratic aisle between 2008 and 2020 using campaign spending data from the [FEC](https://www.fec.gov/campaign-finance-data/campaign-finance-statistics/).


<table style="text-align:center"><caption><strong>Effect of Campaign Spending on Democratic Vote Share</strong></caption>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="3"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="3" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="3">D</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Log(Contribution Amount)</td><td>4.659<sup>***</sup></td><td>1.091</td><td>0.343</td></tr>
<tr><td style="text-align:left"></td><td>(0.460)</td><td>(0.678)</td><td>(1.234)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">State Fixed Effects</td><td>No</td><td>Yes</td><td>Yes</td></tr>
<tr><td style="text-align:left">Year Fixed Effects</td><td>No</td><td>No</td><td>Yes</td></tr>
<tr><td style="text-align:left">Observations</td><td>200</td><td>200</td><td>200</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.341</td><td>0.938</td><td>0.959</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.338</td><td>0.918</td><td>0.944</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="3" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

While this is admittedly a very rough regression table, it is still telling that, even before controlling for time and entity fixed effects, the effect of campaign spending on democratic vote share is exceptionally minimal. And, once we have considered these two fixed effects, the effect of campaign spending is no longer statistically significant. This isn't to suggest that advertisement spending is not consequential — it more likely evidences how campaign spending is like an arms race where the spending of one party is negated by the spending of the other. 

# Improving My Electoral College Model

Last week, I constructed an [elastic model](https://grantbw4.github.io/election-blog/post/2024/10/02/week-5-turnout/) of the 2024 election using both fundamental and polling data. 

This week, I will modify this model by exploring a Bayesian linear model in addition to the frequentist elastic net model. My elastic net model, this week, will be slightly different too as I will only consider the polling data from the past 8 weeks and I will not simultaneously predict both Republican and Democratic vote share. I am only considering polling data from the past 8 weeks as I believe constructing an "average polling average" for weeks when Biden was the nominee or before Harris had been cemented as the nominee could introduce inaccuracies to the projection. The Bayesian linear regression model will assume that the two-party Democratic vote share is normally distributed around the mean as calculate by the linear combination of the same variables initially included in the elastic net, and, then, I will construct a posterior distribution using Markov Chain Monte Carlo before ultimately offering a final prediction.

As was the case last week, I will use state-level polling average data since 1980 from [FiveThirtyEight](https://projects.fivethirtyeight.com/polls/) and national economic data from the [Federal Reserve Bank of St. Louis](https://fred.stlouisfed.org/). I will construct an elastic net model that uses the following fundamental and polling features:

- Latest polling average for the Democratic candidate within a state 
- Average polling average for the Democratic candidate within a state 
- A lag of the previous election's two-party vote share for the Democrats within a state
- A lag of the election previous to last election's two-party vote share for the Democrats within a state
- Whether a candidate was incumbent 
- GDP growth in the second quarter of the election year

There are only 19 states for which we have polling averages for 2024. These 19 states include our 7 most competitive battleground states, a few other more competitive states, and a handful of non-competitive states (California, Montana, New York, Maryland, Missouri, etc.)

We will train a model using all of the state-level polling data that we have access to since 1980, and then test this data on our 19 states on which we have 2024 polling data. We can then evaluate how sensible the predictions are given what we know about each state.

Here are the results from our elastic-net model:



<table class="table table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> state </th>
   <th style="text-align:right;"> simp_pred_dem </th>
   <th style="text-align:left;"> winner </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> arizona </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 49.66918 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> california </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 61.23450 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> florida </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 47.61852 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> georgia </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.22281 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> maryland </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 64.67678 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> michigan </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.74988 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> minnesota </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 52.99454 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> missouri </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 44.08608 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> montana </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 42.47695 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> nevada </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.09261 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> new hampshire </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 53.89531 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> new mexico </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 53.06882 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> new york </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 56.44713 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> north carolina </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 49.56832 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> ohio </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 45.53835 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> pennsylvania </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.31533 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> texas </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 47.22080 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> virginia </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 53.50393 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> wisconsin </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.43930 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
</tbody>
</table>

And here are the predictions from our Bayesian linear regression model:



<table class="table table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> state </th>
   <th style="text-align:right;"> bayes_pred_dem </th>
   <th style="text-align:left;"> bayes_winner </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> arizona </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 49.54172 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> california </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 61.07469 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> florida </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 47.45563 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> georgia </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.11338 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> maryland </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 64.58503 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> michigan </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.61986 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> minnesota </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 52.88878 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> missouri </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 43.96308 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> montana </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 42.35801 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> nevada </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 49.95015 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> new hampshire </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 53.79866 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> new mexico </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 52.92628 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> new york </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 56.26583 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> north carolina </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 49.43565 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> ohio </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 45.38914 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> pennsylvania </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.18232 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> texas </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 47.09161 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Republican </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> virginia </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 53.39610 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> wisconsin </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.29852 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Democrat </td>
  </tr>
</tbody>
</table>

Apart from slightly different polling predictions, the only significant departure in this Bayesian prediction from the frequentist prediction is the winner of Nevada, which, per the Bayesian model, is Trump.

These electoral maps are visible below.



<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-13-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-13-2.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-13-3.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-13-4.png" width="672" />

If we also wanted to model the national popular vote, we could use what we did in Week 3, using an elastic net on both fundamental and polling data, weighting such that the polls closer to November matter more. This was Nate Silver's approach. Again, I will only be considering polls within 8 weeks of the election.



Doing so, we find that the Democrats are projected to have a narrow lead in the two-party popular vote nationally (after scaling so that the estimates sum to 100%).


```
## Democrat two-party vote share:  50.93 %
```

```
## Republican two-party vote share:  49.07 %
```

# Citations:

Cavazos, Nidia, et al. “Kamala Harris Campaign Surpasses $1 Billion in Fundraising, Source Says.” *CBS News*, CBS Interactive, 10 Oct. 2024, www.cbsnews.com/news/kamala-harris-campaign-fundraising-1-billion/. 

Evers-Hillstrom, Karl. “Most Expensive Ever: 2020 Election Cost $14.4 Billion.” *OpenSecrets* News, 11 Feb. 2021, www.opensecrets.org/news/2021/02/2020-cycle-cost-14p4-billion-doubling-16/. 

Kamarck, Elaine, et al. “Why Hillary Clinton Lost.” *Brookings*, 20 Sept. 2017, www.brookings.edu/articles/why-hillary-clinton-lost/. 

# Data Sources: 

Data are from the US presidential election popular vote results from 1948-2020, [polling data from fivethirtyeight](https://projects.fivethirtyeight.com/polls/), economic data from the [St. Louis Fed](https://fred.stlouisfed.org/), [campaign spending data from the FEC](https://www.fec.gov/campaign-finance-data/campaign-finance-statistics/) between 2008 and 2024, and campaign advertisement data from [the Wesleyan Media Project](https://mediaproject.wesleyan.edu/).

