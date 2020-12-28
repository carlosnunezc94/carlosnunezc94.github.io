---
title: "Analysing Instacart transactions using Association Rules"
date: 2020-12-28
tags: [Data Science, Pandas, Matplotlib]
category: "Data Science"
excerpt: "Data Science, Market Basket Analysis, Pandas, Matplotlib, Seaborn, Customer Behavior"
classes: wide
header: 
    image: "/images/market_basket/shopping.png"
---

## Motivation

Some time ago, I was curious to be able to analyze data in more ingenious ways. For many, it is enough to be able to handle tables with structured data, which, thanks to a good acquisition process, and in many cases, thanks to the availability of people, do not have much missing data. This is good for the analyst. However, I believe that handling structures of these characteristics is the starting point, and seeing the data in more creative ways, such as using graph theory and clusters, is the next step to having a greater understanding of the acquired data, and also adds versatility to the toolkit at my (and perhaps your) disposal.


Having said that, I wanted to apply these concepts in the grocery delivery industry, to understand how certain products relate to each other. Specifically, I will analyze how 3 departments (Deli, international food and alcohol) interact with each other based on transactions of a well known company. Some of the questions we will answer today are:


- What data are we analyzing?
- Which products are the most requested? Which department and aisle are they from?
- Which product A is more likely to be in the same order if it is product B?
- Which aisles should go together in a supermarket?