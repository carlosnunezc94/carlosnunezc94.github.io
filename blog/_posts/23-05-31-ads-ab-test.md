---
title:  "Reducing ads spending though A/B Testing"
classes: wide
header:
  teaser: /assets/images/ads.png
  overlay_image: /assets/images/ads_header.png
  overlay_filter: 0.7 # same as adding an opacity of 0.5 to a black background

excerpt: "Performing exploratory analysis, feature engineering and statistical techniques to address business questions regarding ads overspending"
---
 

In this blog post, I will share with you my approach for a take-home assignment I got from a company for a Data Science position in the ads data science team, which I did in January of 2021 approximately. During the article, you can expect my approach for exploratory analysis, feature engineering, data visualization, and A/B testing, providing code when I consider it appropriate. 

The audience of the solution is product managers with knowledge of data science and experimentation practices, so I'm not gonna go in-depth on the definitions of the different methods unless I find them worth describing (Particularly when we talk about statistical tests, both parametric and non-parametric).
<br>
<br>

# The Problem
---
We are on the shoes of a Data Scientist at AdsSupreme, a company with a platform where businesses can create advertising campaigns to increase awareness of their brands or to increase adoption of their products or services. Let's imagine we have a single ad product where advertisers pay us each time a user clicks on their ad.

Each campaign has a budget, and advertiser never has to pay more than it, so if we were to spend
more than the campaign’s budget, we would not be able to bill the advertiser for the additional
spend. This is called **overspending**. In practice, it's difficult to avoid overspending because generally there are delays between when the platform sends ads to users and when they click on the ads.

In the last months, our team have been noticing an **increase in overspend** on the platform. In an
attempt to reduce the amount of overspend, the product team decided to create a new product where
advertisers pay each time their ad appears in a user’s viewport rather than each time it is clicked
on.

Let's assume that our team designed an A/B test, splitting advertisings into control and treatment variants, and after a week, we have some data available for analysis.

The product team wants to know if the new product was effective at reducing overspending, and how effective depending on the company size. Apart from that, the product team is certain advertisers in the treatment group are entering lower budgets because they are wary of the new product. Is there any evidence supporting those suspicions?


<br>
<br>

# The approach
---

The project is divided into 5 stages: The data itself, the exploratory analysis, the definition overspending and creation of the feature, the A/B test analysis for over-spending, and finally the experiment regarding the treatment group's budgeting. Finally, I will briefly mention what other analysis can be performed on the dataset.


## The data 

The data is composed of 15,474 advertisers with the following information:
- treatment: The variant the advertiser was assigned to (Control or Treatment)
- company_size: Categorical variable representing the size of the company, not the spending of the company (Small, medium, and large). This is
included because businesses of different sizes use AdsSupreme ads platform in very different ways and may react differently to the new product.
- campaign_spend: Campaign spend during the experiment
- campaign_budget: Campaign budget during the experiment

As you can see in the folowing code snippets, luckily our data doesn't have null values so we can skip any decisions regarding missing values and proceed to explore the data
```
df.head()
```


<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ads_overspending/dataframe.png" alt="Daily Table">
</p>

## Exploratory Analysis
First, we are gonna review the categorical variables of our dataset, followed for the continuous, quantitave variables.

### Looking at categorical variables 
---

```
fig = plt.figure(figsize=(15,5))
vars_cat = ['treatment','company_size']
for i, var in enumerate(vars_cat):
    ax = plt.subplot(1,2,i+1)
    sns.countplot(df[var])
    plt.title("Count plot of " + var)  
```
<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ads_overspending/categorical_barplot.png" alt="Daily Table">
</p>

The treatment variable is evenly distributed across our dataset, however, **the company size is not evenly distributed across our 3 categories**. More than half of the campaigns are from small companies, whereas medium companies have the least number of campaigns. The variation across the company_size variable is important since, so far, our conclusions regarding the data will be influenced mostly from small companies, and because we don't know how well distributed is the treatment variable between different company sizes.