---
title: "Analysing Instacart transactions using Association Rules and Graphs"
date: 2020-12-28
tags: [Data Science, Pandas, Matplotlib]
category: "Data Science"
excerpt: "Data Science, Market Basket Analysis, Pandas, Matplotlib, Seaborn, Customer Behavior"
classes: wide
header: 
    image: "/images/market_basket/shopping.jpg"
---

## Motivation

Some time ago, I was curious to be able to analyze data in more ingenious ways. For many, it is enough to be able to handle tables with structured data, which, thanks to a good acquisition process, and in many cases, thanks to the availability of people, do not have much missing data. This is good for the analyst. However, I believe that handling structures of these characteristics is the starting point, and seeing the data in more creative ways, such as using graph theory and clusters, is the next step to having a greater understanding of the acquired data, and also adds versatility to the toolkit at my (and perhaps your) disposal.


Having said that, I wanted to apply these concepts in the grocery delivery industry, to understand how certain products relate to each other. Specifically, I will analyze how 3 departments (Deli, international food and alcohol) interact with each other based on transactions of a well known company. Some of the questions we will answer today are:


- What data are we analyzing?
- Which products are the most requested? Which department and aisle are they from?
- Which product A is more likely to be in the same order if it is product B?
- Which aisles should go together in a supermarket?


## The Dataset

The dataset to be utilized is “The Instacart Online Grocery Shopping Dataset 2017”. The dataset is anonymized and contains a sample of over 3 million grocery orders from more than 200,000 Instacart users. For each user, Instacart provide between 4 and 100 of their orders, with the sequence of products purchased in each order. However, I extracted a subset of the groceries ordered equivalent to the products from the Deli, International and Alcohol department for computing purposes. The dataset consist of the following files:

1) order_products__prior.csv: File with the relationship order-product with:
- order_id: Unique identifier of the order.
- product_id: Unique identifier of the product.
- add_to_cart_order: Position in which the product was added to the cart (Ex: 1-First, 4-Fourth, etc.)
- reordered: Boolean value. It's True if the product was reordered considering the previous order.

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/images/market_basket/order_data.JPG" alt="Daily Table">
</p>

2) products.csv: File with product identifiers and relationship product-aisle-department. Each row contains product identifier, product name, aisle identifier and department identifier.

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/images/market_basket/product_data.JPG" alt="Daily Table">
</p>

3) aisles.csv: File containing the aisle identifiers and names. I won't add a screenshot since I don't think it is needed (You get the point).

4) departments.csv: File containing the aisle identifiers and names. 

## Analyzing orders

Let's start understanding our structured data. 
- The data has 977869 different orders with 3515 different products from the departments mentioned above. In total, there are 1474198 products sold.
- More than 71% of the products comes from the Deli department, whereas just 11% comes from the alcohol section. This makes sense from the point of view that food is more sold than drinks in general. This tendency can be seen in the whole dataset as well.

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/images/market_basket/graph_1.JPG" alt="Daily Table">
</p>