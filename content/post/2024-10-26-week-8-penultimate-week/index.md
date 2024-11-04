---
title: 'Week 8: Penultimate Week'
author: Grant Williams
date: '2024-10-26'
slug: week-8-penultimate-week
categories: []
tags: []
---
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />

# Introduction

In this eighth blog post, in the second-to-last week before the 2024 election, I want to improve my election forecasts for the seven battleground states.

Over the past several weeks, we have explored OLS models, incumbency bias, the relationship between turnout and demographics, Bayesian approaches to predictions, and logistic regression, among other topics. 

One thing that has consistently bothered me about the models I have constructed up to this point has been my fairly arbitrary standard deviation estimate when creating simulations. After using machine learning techniques to prepare a model to predict the two-party national vote share for each candidate within a state, I have run simulations assuming a normal deviation with a two-percentage point standard deviation to evaluate Harris and Trump's chances of winning each state.

This approach has struck me as unsatisfactory, however, because surely we have more confidence in some states' polling averages than others. For this reason, I am going to explore the reliability of polling data from each of our seven battleground states this week.

*The code used to produce these visualizations is publicly available in my [github repository](https://github.com/grantbw4/election-blog) and draws heavily from the section notes and sample code provided by the Gov 1347 Head Teaching Fellow, [Matthew Dardet](https://www.matthewdardet.com/harvard-election-analytics-2024/).*

# Analysis

The other day, I read an [article by Nathaniel Rakich](https://abcnews.go.com/538/states-accurate-polls/story?id=115108709) on ABC's election site, FiveThirtyEight.com

In this article, Rakich calculates the "weighted-average error of general-election polls for president, U.S. Senate and governor conducted within 21 days of elections since 1998 in each state." Seeing Rakich systematically assess the reliability of polls at the state level gave me the idea to adopt a similar method to estimate the variability of our two-party vote share predictions for each state. 

Since standard deviation is roughly calculated by taking every value in a data set, calculating the mean of that data set, and then determining the average distance of each point away from that mean, I am curious if by calculating the average distance of the polls away from the true vote share we can get a better sense of standard deviation than the arbitrary 2 percentage points I had previously been using.

# My Model 

Both [Sabato's Crystal Ball](https://centerforpolitics.org/crystalball/2024-president/) of the Center for Politics and the [Cook Political Report](https://www.cookpolitical.com/ratings/presidential-race-ratings) list the same seven states as "toss-ups." Almost universally across news platforms and forecasting models, the following seven states are identified as the key swing states which will, most likely, determine the election

- Arizona
- Georgia
- Michigan
- Nevada
- North Carolina
- Pennsylvania
- Wisconsin

For the purposes of both this week's model and the next week's final prediction, I will forecast the two-party vote share in each of these battleground states and assume other states and districts vote as they did in 2020.





<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-2.png" width="672" />

# Preparing My Electoral College Model

Of the models I have created thus far, the elastic net, to me, seems to most fairly weigh the various covariates. For this reason, I will adapt much of my code from week five.

Using state-level polling average data since 1980 from [FiveThirtyEight](https://projects.fivethirtyeight.com/polls/) and national economic data from the [Federal Reserve Bank of St. Louis](https://fred.stlouisfed.org/), I will construct an elastic net model that uses the following polling features:

- Polling average for the Republican candidate within a state in the most recent week
- Polling average for the Democratic candidate within a state in the most recent week
- Polling average for the Republican candidate within a state in the last two months
- Polling average for the Democratic candidate within a state in the last two months
- A lag of the previous election's two-party vote share for the Democrats within a state
- A lag of the previous election's two-party vote share for the Republicans within a state
- A lag of the election previous to last election's two-party vote share for the Democrats within a state
- A lag of the election previous to last election's two-party vote share for the Republicans within a state

I am opting to only consider the polling averages within the last two months of the campaign rather than the entirety of the campaign as I believe these to be most predictive of the ultimate election outcome. I am concerned that introducing averages from earlier periods would lead to overfitting, and, considering the unique candidate swap of 2024, I do not believe Biden nor Harris's polling averages from the stage in the election before Harris could establish a proper campaign strategy are informative. I will also be rescaling these mean polling averages so that they sum to 100 and can more readily be interpreted as two-party vote shares.

While there are a number of fundamental and economic covariates I considered exploring (Whether a candidate is only a party incumbent, GDP growth in the second quarter of the election year, RDPI growth in the second quarter of the election year, Unemployment rate in the second quarter of the election year, June Approval Rating, etc.), I found that my forecasts were highly variable depending on which fundamental variables I decided to include. It is my belief that many of the trends explained by fundamental variables (incumbency bias, high growth is good, high inflation is bad, etc.) is already baked into the polls, so I will focus on constructing a polls-only model for this week. Next week, however, I will include both a polls-only and a polls + fundamentals model.

<!-- I will use economic data from Q4 of 2019 instead of Q2 of 2020 because the Q2 data is a massive outlier that skews the regressions. It is also not representative of Trump's presidency. Moreover, because of the candidate swap in 2024, I will use both Trump and Harris's approval ratings from September 2024. -->
We will train separate two-party vote share models for both the Republicans and Democrats in each of the swing states using data since 1980, and then apply this model to our 2024 data to generate predictions. To make this model, I am standardizing my features and regularizing them with elastic net linear regression.











# Visualize Feature Importance

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="672" />

<table class="table table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> state </th>
   <th style="text-align:right;"> predicted_R_pv2p </th>
   <th style="text-align:right;"> predicted_D_pv2p </th>
   <th style="text-align:left;"> pred_winner </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Arizona </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 50.99581 </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 49.00419 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> R </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Georgia </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 50.77415 </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 49.22585 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> R </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Michigan </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 49.74140 </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.25860 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> D </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Nevada </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 49.84690 </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.15310 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> D </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> North Carolina </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 50.84408 </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 49.15592 </td>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> R </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Pennsylvania </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 49.90251 </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.09749 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> D </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Wisconsin </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 49.98305 </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.01695 </td>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> D </td>
  </tr>
</tbody>
</table>

Here, we can see that Arizona, Georgia, and North Carolina are favored to vote red while the other states are favored to vote blue. In the feature importance graph, it is also clear that the latest week polling average is much more predictive than the two-month average.

I will now use a simulation to get an estimate of how confident we are in these results. I will do this by sampling new state-level polling measurements for each of our 7 states 10,000 times, assuming a normal distribution around the current polling values with a standard deviation determined by the average distance of each state's poll away from the actual outcome.

To create this standard deviation, I will take the average of the mean polling error of each state in 2016 and 2020 (weighted equally for both years) to capture the "Trump-era" polling error. 

<table class="table table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> State </th>
   <th style="text-align:right;"> Weighted Error </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Arizona </td>
   <td style="text-align:right;"> 0.7928877 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Georgia </td>
   <td style="text-align:right;"> 0.5363407 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Michigan </td>
   <td style="text-align:right;"> 2.3607260 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nevada </td>
   <td style="text-align:right;"> 1.2531877 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Carolina </td>
   <td style="text-align:right;"> 2.0568637 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pennsylvania </td>
   <td style="text-align:right;"> 1.9538101 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Wisconsin </td>
   <td style="text-align:right;"> 3.6458086 </td>
  </tr>
</tbody>
</table>

Using the above weighted errors as standard deviations yields the following simulation breakdown.

<table class="table table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;font-weight: bold;"> State </th>
   <th style="text-align:right;font-weight: bold;"> D Win Percentage </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Arizona </td>
   <td style="text-align:right;"> 6.58 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Georgia </td>
   <td style="text-align:right;"> 7.53 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Michigan </td>
   <td style="text-align:right;"> 60.49 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nevada </td>
   <td style="text-align:right;"> 61.16 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Carolina </td>
   <td style="text-align:right;"> 35.92 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pennsylvania </td>
   <td style="text-align:right;"> 59.55 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Wisconsin </td>
   <td style="text-align:right;"> 55.02 </td>
  </tr>
</tbody>
</table>

# Projections 

Using this model, our ultimate electoral college would look as follows, with Vice President Kamala Harris narrowly squeaking out a win.

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-13-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-13-2.png" width="672" />

# Citations:

Rakich, Nathaniel. “Which States Have the Most — and Least — Accurate Polls?” ABC News, ABC News Network, 25 Oct. 2024, abcnews.go.com/538/states-accurate-polls/story?id=115108709. 

# Data Sources: 

Data are from the US presidential election popular vote results from 1948-2020, [state-level polling data for 1980 onwards](https://projects.fivethirtyeight.com/polls/), and economic data from the [St. Louis Fed](https://fred.stlouisfed.org/).
