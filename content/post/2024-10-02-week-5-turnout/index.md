---
title: 'Week Five: Turnout'
author: Grant Williams
date: '2024-10-02'
slug: week-5-turnout
categories: []
tags: []
---
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />

# Introduction

In this fifth blog post, I am going to discuss two areas of election forecasting: **turnout** and **demographics**.

Then, I am going to prepare a baseline model to predict the 2024 election. Over the next 4 weeks until the November 5th election, I will fine tune this model and ultimately use it to predict the next president of the United States of America.

*The code used to produce these visualizations is publicly available in my [github repository](https://github.com/grantbw4/election-blog) and draws heavily from the section notes and sample code provided by the Gov 1347 Head Teaching Fellow, [Matthew Dardet](https://www.matthewdardet.com/harvard-election-analytics-2024/).*

# Analysis

The United States' current priors and general beliefs about turnout and demographics are informed, in part, by longstanding academic literature on the subject.

Two of the most influential publications are *Who Votes?*, a 1980 book by [Professors Wolfinger and Rosenstone](https://yalebooks.yale.edu/book/9780300025521/who-votes/), and *Mobilization, Participation, and Democracy in America*, a 1993 book by [Professors Rosenstone and Hansen](https://www.amazon.com/Mobilization-Participation-Democracy-America-Politics/dp/0024036609). Both of these publications popularized theories about the connection between demographics and voter turnout that would permeate US society for decades to come. 

Wolfinger and Rosenstone ran OLS regressions on census data between 1972 and 1974 to determine that education was the key demographic variable influencing turnout; age, marital status, and the restrictiveness of voter registration laws also dispalyed high rates of correlation. In 1993, Rosenstone and Hansen expanded upon Wolfinger and Rosenstone's findings, determining that those most likely to vote tended to be white, wealthy, and educated. They also uncovered, however, using data from the [American National Election Studies (ANES)](https://electionstudies.org/) that turnout was highly affected by mobilization efforts within social networks. These two studies gave the strong impression that demographics were of significant relevance to turnout.

This prevailing narrative, however, has faced increased scrutiny in recent years. 

Professors [Shaw and Petrocik](https://global.oup.com/academic/product/the-turnout-myth-9780190089450?cc=us&lang=en&), for example, challenged *The Turnout Myth* in their 2020 book of the same title, finding no evidence in the past 50 years of presidential election data that higher rates of turnout benefit Democrats, as the conventional narrative would suggest. Instead, Shaw and Petrocik argue that "turnout does not consistently help either party" (Shaw & Petrocik 2020). 

Another two professors, using logistic regressions and random forest models on demographic data from the [American National Election Studies (ANES)](https://electionstudies.org/) between 1952 and 2020, observed results that similarly pour cold water on assumptions about demographics' high predictiveness of turnout. Leveraging public opinion surveys, [Professors Kim and Zilinsky](https://link.springer.com/article/10.1007/s11109-022-09816-z) determined that predictions using the demographic "variables of age, gender, race, education, and income" exhibited less than 64% accuracy out-of-sample, regardless of whether the predictions were made with a random forest or a logistic regression model. Including party identification, however, improves the accuracy by between 20 and 30 percentage points. The improvement possible by including even all of the additional covariates found in a voter file (marital status, homeownership, etc.), at this point, is fairly marginal.

For these reasons, in my first electoral college and national popular vote model, I am not going to explicitly include demographic variables. Instead, I will consider polling averages, fundamental economic conditions, and party registration. In future weeks, I hope to include additional analysis from voter files and more explicitly model turnout and demographics at the state level, but, for this first week, I will start with something simpler. 

# My Model 

Both [Sabato's Crystal Ball](https://centerforpolitics.org/crystalball/2024-president/) of the Center for Politics and the [Cook Political Report](https://www.cookpolitical.com/ratings/presidential-race-ratings) list the same seven states as "toss-ups." These include the following: 

- Arizona
- Georgia
- Michigan
- Nevada
- North Carolina
- Pennsylvania
- Wisconsin

While it is not inconceivable that other states/districts could unexpectedly flip (Florida, Ohio, Nebraska 2nd district, Virgina, Texas, etc), it is highly unlikely that one of these states would 'decide' the election. If Florida were to go blue, for example, other more competitive states would have likely gone blue as well, clinching the election for Harris. While there exist realities where Texas or Florida or Ohio could be the tipping point of the presidential election, for the purposes of this week's blog post, I will focus on the seven most commonly cited battleground states.

With this assumption in place, assuming other states and districts vote as they did in 2020, the base electoral map for 2024 looks as follows:



















# Citations:

Kim, Seo-young Silvia, and Jan Zilinsky. 2021. “The Divided (But Not More Predictable) Electorate: A Machine Learning Analysis of Voting in American Presidential Elections.” *APSA Preprints.* doi: 10.33774/apsa-2021-45w3m-v2.  This content is a preprint and has not been peer-reviewed.

Rosenstone, Steven J., and John Mark Hansen. *Mobilization, Participation, and Democracy in America.* Macmillan Pub. Co: Maxwell Macmillan Canada: Maxwell Macmillan International, 1993.

Shaw, Daron, and John Petrocik. *The Turnout Myth: Voting Rates and Partisan Outcomes in American National Elections.* 1st ed., Oxford University Press, 2020, https://doi.org/10.1093/oso/9780190089450.001.0001.

Wolfinger, Raymond E., and Steven J. Rosenstone. *Who Votes?* Yale University Press, 1980.

# Data Sources: 

Data are from the US presidential election popular vote results from 1948-2020 and [Kriner and Reeves 2015](https://www-cambridge-org.ezp-prod1.hul.harvard.edu/core/journals/american-political-science-review/article/presidential-particularism-and-dividethedollar-politics/962ABE4FC41A6FF3E1F95CE1B54D1ADD).
