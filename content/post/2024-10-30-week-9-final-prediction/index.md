---
title: 'Week 9: Final Prediction'
author: Grant Williams
date: '2024-11-03'
slug: week-9-final-prediction
categories: []
tags: []
output:
  html_document:
    mathjax: default
---
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />

# Introduction

In this final blog post, published two days before the election (Sunday Nov 3), I am going to explain how I prepared my final model and the results derived from it.

Before I do so, however, I want to share what is perhaps my main takeaway from this class thus far:

Predicting elections is **hard**.

In many ways, it is a somewhat impossible task. Forecasting elections is not difficult because procuring data is challenging or because training models is computationally expensive; rather, predicting the outcomes of US elections is fundamentally hard because there are relatively few elections from which we can construct our models.

Since (and including) the United States' first election in 1789, there have been a grand total of [59 presidential elections (270 To Win)](https://www.270towin.com/historical-presidential-elections/). This means we only have 59 outcomes from which we can identify patterns and create a model to predict the two-party vote share at the national level. Once we consider that polling data only goes as far back as 1968, we are left with only 14 elections from which to base our models. While it could be tempting to believe that we actually have closer to 14 times 50 elections, or 700 elections, from which to make our models (if we used the election results from each state), this isn't quite the case. Polling can be expensive, so generally, state-level polling studies are conducted in more competitive states, and we may not have sufficient polling data to produce models for all of our states in every election cycle. For this reason, we have relatively few outcomes, especially in recent years, from which we can produce models. This is an issue because, with limited testing data, we run the risk of overfitting, creating models that generalize poorly.

Though outcome data is a significant limitation, the same cannot be said of covariate data. Through lab sections, we have explored voter file data sets, turnout data sets, demographic data sets, macroeconomic data sets, polling data sets, campaign spending data sets, campaign event data sets, and other data sets on political fundamentals (incumbent candidate, June approval rate, etc.). There is an incredible volume of predictors that we could theoretically include in our model (national unemployment rate, mean polling averages, lagged vote share, etc.) We could even imagine breaking down these predictors: should we include the latest week's polling average by state, or the latest month's, or the average in the last year? Over the past several weeks, we have also explored a variety of regression tools including OLS, Bayesian priors, logistic regression, LASSO and Ridge, and other machine learning models. 

One approach to handling this overabundance of covariate data and the various regression modelling techniques at our disposal is to use all of the outcome and predictor data that we possess, and then use cross-validation to determine which permutation of covariate features and machine learning models yields the lowest RMSE for predicting these outcomes. While this is tempting, it is also likely to produce a very complicated model that is highly sensitive to decisions in the validation process:

- Do we equally weight the model's ability to predict the winner of each state in each election year? Or do we weigh more recent elections more heavily? States with more electoral votes? More competitive states? States with more polls? How many polls must a state have for us to include it as an observation in our testing data set? Are all polls equally valid, or should we weigh them by their credibility?

- Do we believe that Americans vote differently now from how they would have in decades past? Are voters less likely to vote for a candidate of a different party now than in the past? Do voters consider either candidate incumbent in 2024? Are voters more responsive to unemployment and inflation in some years compared to others?

For something that, at face value, might seem like a scientific prediction problem, the process of constructing a 2024 election forecast is, in my opinion, in many ways, a subjective art. We are making assumptions about voter behavior that could actually vary across years.

Given how much space there is for bias, I want to keep this model as simple as possible. This choice is a bold one, but I want to test how well a model with few covariates compares to other classmates' more complex models models.

*The code used to produce these visualizations is publicly available in my [github repository](https://github.com/grantbw4/election-blog) and draws heavily from the section notes and sample code provided by the Gov 1347 Head Teaching Fellow, [Matthew Dardet](https://www.matthewdardet.com/harvard-election-analytics-2024/).*

# My Model 

Both [Sabato's Crystal Ball](https://centerforpolitics.org/crystalball/2024-president/) of the Center for Politics and the [Cook Political Report](https://www.cookpolitical.com/ratings/presidential-race-ratings) list the same seven states as "toss-ups." Almost universally across news platforms and forecasting models, the following seven states are identified as the key swing states which will, most likely, determine the election:

- Arizona
- Georgia
- Michigan
- Nevada
- North Carolina
- Pennsylvania
- Wisconsin

Interestingly, both of these preeminent forecasting websites, as of November 3rd, do not rank a single state as either lean Republican or lean Democrat. There are only "solid"s, "likely"s, and "toss-up"s. Thus, for the purposes of both this week's final prediction, I will forecast the two-party vote share in each of these battleground states and assume other states and districts vote as they did in 2020.

This gives us the following initial electoral college map:





<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-2.png" width="672" />

# Preparing My Electoral College Model

Using national-level polling average data since 1968 from [FiveThirtyEight](https://projects.fivethirtyeight.com/polls/), I will construct a model that uses the following polling features to predict the national two-party vote share.

- Polling average for the Republican candidate within a state in each of the ten most recent weeks
- Polling average for the Democratic candidate within a state in each of the ten most recent weeks

I am opting to only consider the polling averages within the last ten weeks of the campaign rather than the entirety of the campaign as I believe these to be most predictive of the ultimate election outcome. I am concerned that introducing averages from earlier periods would lead to overfitting, and, considering the unique candidate swap of 2024, I do not believe Biden nor Harris's polling averages from the stage in the election before Harris could establish a proper campaign strategy are informative. These averages also occur after both parties have held their respective conventions and show the leanings of a more settled electorate. I will also be rescaling all polling averages so that the two-party vote share sums to 100. After doing this, I will only use Democratic polls to create a model (which is possible because Republican two-party vote share is just 100 - Democratic two-party vote share).

While there are a number of fundamental and economic covariates I considered exploring (whether a candidate is only a party incumbent, GDP growth in the second quarter of the election year, RDPI growth in the second quarter of the election year, unemployment rate in the second quarter of the election year, June approval rating, etc.), I found that my forecasts were highly variable depending on which fundamental variables I decided to include. It is my belief that many of the trends explained by fundamental variables (incumbency bias, high growth is good for candidates, high inflation is bad for candidates, etc.) is already baked into the polls, so I will focus on constructing a polls-only model.

We will train a two-party vote share model for to predice Democratic vote share, and then apply this model to our 2024 data to generate predictions. 

First, I want to assess whether modern national polls are systematically more accurate than older polls.





<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" />

Because the year with the highest polling error was the first year in which polls were conducted in our data set, there is reason to think the science of polling was still developing. Since this outlier could skew the results of our regression and is plausibly not representative of current possibilities, I feel comfortable removing the year 1968 from our sample. Otherwise, the polling errors look reasonably uncorrelated with time.



Now, I want to find the optimal way to average my week by week polling averages into an overall estimate for the national two-party Democratic vote share. Intuitively, I can assume that more recent polls are more accurate than less recent polls. I can model the weight of each column (week 1 through 10) with a decay factor, alpha, that compounds exponentially each week further away from week 1 that I get. 

![Image 1](image_2.png)

I will test all decay value within the set of {1, 0.99, 0.98,..., 0.51, 0.5}.

For each decay value, I will use leave-one-out cross validation to calculate the overall RMSE of a linear regression that estimates the two-party Democratic vote share using the weighted average vote share from each election cycle except the cycle that is currently being left out.

This process yields the following RMSE graph. As we can see, 0.78 is the optimal decay value.



<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="672" />

This yields the following weights for each week before the election:

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> Weeks Left Until Election </th>
   <th style="text-align:right;"> Weight </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 0.7800000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 0.6084000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0.4745520 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 0.3701506 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 0.2887174 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 0.2251996 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 0.1756557 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 0.1370114 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.1068689 </td>
  </tr>
</tbody>
</table>

The mean In-Sample RMSE is 1.410265.
The mean Out-of-Sample squared error is 2.626053.

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> Fold Left Out </th>
   <th style="text-align:right;"> In-Sample RMSE </th>
   <th style="text-align:right;"> Out-of-Sample SE </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.455863 </td>
   <td style="text-align:right;"> 1.7409968 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 1.478439 </td>
   <td style="text-align:right;"> 0.0230401 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 1.169652 </td>
   <td style="text-align:right;"> 10.7116157 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.395906 </td>
   <td style="text-align:right;"> 3.7339550 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1.479025 </td>
   <td style="text-align:right;"> 0.0000835 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 1.458749 </td>
   <td style="text-align:right;"> 0.8666411 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 1.461536 </td>
   <td style="text-align:right;"> 0.7915443 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 1.409345 </td>
   <td style="text-align:right;"> 2.6173176 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 1.471802 </td>
   <td style="text-align:right;"> 0.2778467 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 1.304142 </td>
   <td style="text-align:right;"> 6.4688561 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 1.310783 </td>
   <td style="text-align:right;"> 6.1094295 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 1.472901 </td>
   <td style="text-align:right;"> 0.2409560 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 1.465302 </td>
   <td style="text-align:right;"> 0.5564014 </td>
  </tr>
</tbody>
</table>

This model yields the following Harris-Trump national popular vote prediction.

<table class="table table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> Year </th>
   <th style="text-align:right;"> Predicted Democratic Two-Party Vote Share </th>
   <th style="text-align:right;"> Predicted Republican Two-Party Vote Share </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 2024 </td>
   <td style="text-align:right;"> 51.15365 </td>
   <td style="text-align:right;"> 48.84635 </td>
  </tr>
</tbody>
</table>

# State-level estimates

I will now use this same decay factor to predict the two-party vote share for each of our battleground states. I chose to find this decay factor from national popular vote data because I have higher confidence in the consistent quality of national polls over time than the consistent quality of state polls over time, especially across states. It also seems like a reasonable assumption to me that this decay factor would remain largely constant over time. 

For the same reasons as those I have already mentioned, I want to make this model as simple as possible and will only be considering the polling averages in each of the 10 weeks leading up to the election for each of our battleground states. I will use these 10 polling averages to construct a weighted average for each state. The formula for our weighted average (and our model) is as follows:

![Image 2](image_1.png)

The weighted average for the Democratic two-party vote share for each state in 2024 is displayed below:





<table class="table table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> State </th>
   <th style="text-align:right;"> Predicted Democratic Two-Party Vote Share </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Arizona </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 49.24514 </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> Georgia </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 49.36078 </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Michigan </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.57455 </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Nevada </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.19946 </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(255, 48, 48, 255) !important;"> North Carolina </td>
   <td style="text-align:right;background-color: rgba(255, 48, 48, 255) !important;"> 49.56858 </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Pennsylvania </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.18314 </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Wisconsin </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.53039 </td>
  </tr>
</tbody>
</table>

Here, we can see that Arizona, Georgia, and North Carolina are favored to vote red while the other states are favored to vote blue. 

For comparison, let's see how our model would have fared for these same states in 2020.

<table class="table table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> State </th>
   <th style="text-align:right;"> Predicted Democratic Two-Party Vote Share </th>
   <th style="text-align:right;"> Actual pv2p </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Arizona </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 51.96036 </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.15683 </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Georgia </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.16966 </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.11933 </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Michigan </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 54.11059 </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 51.41356 </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Nevada </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 53.14477 </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 51.22312 </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> North Carolina </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 51.12033 </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 49.31582 </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Pennsylvania </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 53.03826 </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.58921 </td>
  </tr>
  <tr>
   <td style="text-align:left;background-color: rgba(16, 78, 139, 255) !important;"> Wisconsin </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 53.73649 </td>
   <td style="text-align:right;background-color: rgba(16, 78, 139, 255) !important;"> 50.31906 </td>
  </tr>
</tbody>
</table>

All of our states would have been projected to vote blue, which was the case with the exception of North Carolina, which voted Republican. This is an RMSE of 2.24 percentage points.



Across the 442 state elections since 1968 in which we have polling averages for each of the ten weeks leading up to the election, our model correctly predicts the winner of the state 91.12% of the time with an RMSE of roughly 3.02 percentage points.

I will now use a simulation to get an estimate of how confident we are in these results. I will do this by sampling new state-level polling measurements for each of our 7 states 10,000 times, assuming a normal distribution around the current polling values with a standard deviation determined by the average distance of each state's poll away from the actual outcome in the past two elections.

To create this standard deviation, I will take the average of the mean polling error of each state in 2016 and 2020 (weighted equally for both years) to capture the "Trump-era" polling error. This assumes that there is no systematic polling error in favor of either candidate, which is plausible given how the 2022 midterm favored Democrats. I will also use a stein adjusted error since I find it more realistic than simply averaging the polling error of a stat in the last two cycles.

<table class="table table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> State </th>
   <th style="text-align:right;"> Average Error </th>
   <th style="text-align:right;"> Stein Adjusted Error </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Arizona </td>
   <td style="text-align:right;"> 1.5458014 </td>
   <td style="text-align:right;"> 1.7387023 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Georgia </td>
   <td style="text-align:right;"> 0.4857592 </td>
   <td style="text-align:right;"> 0.9464523 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Michigan </td>
   <td style="text-align:right;"> 3.5667802 </td>
   <td style="text-align:right;"> 3.2491329 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nevada </td>
   <td style="text-align:right;"> 1.1831851 </td>
   <td style="text-align:right;"> 1.4676916 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Carolina </td>
   <td style="text-align:right;"> 2.3973182 </td>
   <td style="text-align:right;"> 2.3751053 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pennsylvania </td>
   <td style="text-align:right;"> 3.1957976 </td>
   <td style="text-align:right;"> 2.9718696 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Wisconsin </td>
   <td style="text-align:right;"> 3.7910859 </td>
   <td style="text-align:right;"> 3.4167736 </td>
  </tr>
</tbody>
</table>

Using the above weighted errors as standard deviations yields the following simulation breakdown.

<table class="table table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;font-weight: bold;"> State </th>
   <th style="text-align:right;font-weight: bold;"> D Win Percentage </th>
   <th style="text-align:right;font-weight: bold;"> 2.5th Percentile </th>
   <th style="text-align:right;font-weight: bold;"> 97.5th Percentile </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Arizona </td>
   <td style="text-align:right;"> 26.84 </td>
   <td style="text-align:right;"> 45.90000 </td>
   <td style="text-align:right;"> 52.65498 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Georgia </td>
   <td style="text-align:right;"> 16.66 </td>
   <td style="text-align:right;"> 47.47985 </td>
   <td style="text-align:right;"> 51.19328 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Michigan </td>
   <td style="text-align:right;"> 60.81 </td>
   <td style="text-align:right;"> 44.21533 </td>
   <td style="text-align:right;"> 57.12646 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nevada </td>
   <td style="text-align:right;"> 57.06 </td>
   <td style="text-align:right;"> 47.33077 </td>
   <td style="text-align:right;"> 53.03849 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Carolina </td>
   <td style="text-align:right;"> 40.15 </td>
   <td style="text-align:right;"> 44.86851 </td>
   <td style="text-align:right;"> 54.21899 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pennsylvania </td>
   <td style="text-align:right;"> 53.48 </td>
   <td style="text-align:right;"> 44.32085 </td>
   <td style="text-align:right;"> 55.93782 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Wisconsin </td>
   <td style="text-align:right;"> 59.62 </td>
   <td style="text-align:right;"> 43.84012 </td>
   <td style="text-align:right;"> 57.29537 </td>
  </tr>
</tbody>
</table>

We also get the following distribution of simulations.

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-20-1.png" width="672" />

This distribution is somewhat misleading, however, because it assumes that the outcomes of each of the seven battleground states are independent from one another (and that the polling errors across the states are not correlated with one another). For this reason, I will project the winner of the election based on which candidate is most likely to win each state. This distribution does show, however, that part of Harris's weakness is that she is performing well in states with higher average polling errors.

# Projections 

Using this model, our ultimate electoral college would look as follows, with Vice President Kamala Harris narrowly squeaking out a win.

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-21-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-21-2.png" width="672" />

# Citations:

“Historical U.S. Presidential Elections 1789-2020 - 270towin.” *270toWin*, www.270towin.com/historical-presidential-elections/. Accessed 3 Nov. 2024. 

# Data Sources: 

Data are from the US presidential election popular vote results from 1948-2020 and [state-level polling data for 1968 onwards](https://projects.fivethirtyeight.com/polls/).
