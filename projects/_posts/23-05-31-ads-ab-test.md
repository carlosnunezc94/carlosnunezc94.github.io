---
title:  "Reducing ads overspending though AB Testing"
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

# Increasing overspending and lower budgets
---
We are on the shoes of a Data Scientist at AdsSupreme, a company with a platform where businesses can create advertising campaigns to increase awareness of their brands or to increase adoption of their products or services. Let's imagine we have a single ad product where advertisers pay us each time a user clicks on their ad.

Each campaign has a budget, and advertiser never has to pay more than it, so if we were to spend
more than the campaign’s budget, we would not be able to bill the advertiser for the additional
spend. This is called **overspending**. In practice, it's difficult to avoid overspending because generally there are delays between when the platform sends ads to users and when they click on the ads.

In the last months, our team have been noticing an **increase in overspending** on the platform. In an
attempt to reduce the amount of overspend, the product team decided to create a new product where
advertisers pay each time their ad appears in a user’s viewport rather than each time it is clicked
on.

Let's assume that our team designed an AB test, splitting advertisings into control and treatment variants, and after a week, we have some data available for analysis.

The product team wants to know if the new product was effective at reducing overspending, and how effective depending on the company size. Apart from that, the product team is certain advertisers in the treatment group are entering lower budgets because they are wary of the new product. Is there any evidence supporting those suspicions?


<br>
<br>

# From EDA to AB Testing
---

The project is divided into 5 stages: The data itself, the exploratory analysis, the definition overspending and creation of the feature, the A/B test analysis for over-spending, and finally the experiment regarding the treatment group's budgeting. Finally, I will briefly mention what other analysis can be performed on the dataset


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
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ads_overspending/dataframe.png" alt="Dataframe">
</p>

## Exploratory Analysis
First, we are gonna review the categorical variables of our dataset, followed for the continuous, quantitave variables

### Categorical variables - Size of our clients
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
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ads_overspending/categorical_barplot.png" alt="Barplots">
</p>

The **treatment** variable is evenly distributed across our dataset, however, **the company size is not evenly distributed across our 3 categories**. More than half of the campaigns are from small companies, whereas medium companies have the least number of campaigns. The variation across the **company_size** variable is important since, so far, our conclusions regarding the data will be influenced mostly from small companies, and because we don't know how well distributed is the **treatment** variable between different company sizes


### Continuous variables - Distribution of spending and budget
---
```
plt.figure(figsize=(15,5))
vars_quant = ['campaign_spend','campaign_budget']
for i, var in enumerate(vars_quant):
    ax = plt.subplot(1,2,i+1)
    ax.tick_params(axis='x', colors=fg_color, labelsize=12)    #setting up X-axis tick color to red
    ax.tick_params(axis='y', colors=fg_color, labelsize=12)  #setting up Y-axis tick color to black
    plt.hist(np.log(df[var]),bins = 50,color = 'grey')
    plt.title("Histogram of " + var, color = 'white')
    plt.xlabel('Log of ' + var, fontsize=12, color = 'white')
    plt.ylabel('Frequency', fontsize=12, color='white')
    plt.gca().xaxis.set_major_formatter(mticker.FormatStrFormatter('%d'))
```

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ads_overspending/histogram.png" alt="Histogram">
</p>

```
plt.figure(figsize=(15,5))
vars_quant = ['campaign_spend','campaign_budget']
for i, var in enumerate(vars_quant):
    ax = plt.subplot(1,2,i+1)
    ax.tick_params(axis='x', colors=fg_color, labelsize=12)    #setting up X-axis tick color to red
    ax.tick_params(axis='y', colors=fg_color, labelsize=12)  #setting up Y-axis tick color to black
    sns.boxplot(x=np.log(df[var]))
    plt.title("Boxplot of " + var, color = 'white')
    plt.xlabel('Log of ' + var, fontsize=12, color = 'white')
    plt.gca().xaxis.set_major_formatter(mticker.FormatStrFormatter('%d'))
```

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ads_overspending/boxplot.png" alt="Daily Table">
</p>


The spending and budget of the clients are skewed-right. This is not surprising since our clients are mostly small businesses. The level of skewedness is such that even applying a log function to the variables the shape of the curves are skewed to the right. It will be important to keep in mind that a couple of clients are the ones contributing the most to the spending in our platform. Let's look at some descriptive stats of our data:

```
pd.options.display.float_format = '{:,.2f}'.format
for i in ['campaign_spend','campaign_budget']:
    print('Statistics for ' + i)
    print(df[i].describe())
```

<!-- | Variable    |campaign_spend | campaign_budget |
| ----------- | ------------- | --------------- |
| Count       | 15,474.00     | 15,474.00       |
| mean        | 4,903.04      | 5,772.61        |
| std         | 65,166.92     | 99,033.81       |
| min         | 0.36          | 0.09            |
| 25%         | 15.18         | 12.79           |
| 50%         | 50.09         | 48.82           |
|75%          | 236.55        | 252.32          |
| max         | 5,289,216.88  | 10,242,888.21   | -->


<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ads_overspending/stats_table.png" alt="Daily Table">
</p>

The mean is about 100 times bigger than the median for both spending and budget. This is not surprising when we compare the different percentiles with the maximum spending and budget made for a single campaign. It is important to keep in mind the level of skewness when defining the kind of test we are gonna implement and what exactly we want to compare between variants (Mean vs Median)

A similar analysis can be performed by company size. To avoid the overhead of content in the post, I will skip deeper exploration, but the steps are not different to what we have already done

