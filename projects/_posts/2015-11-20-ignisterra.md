---
title:  "The Facility Layout Problem of Ignisterra Warehouse"
classes: wide
header:
  teaser: /assets/images/ignisterra/ignisterra.jpg
  overlay_image: /assets/images/ignisterra/ignisterra.jpg
  overlay_filter: 0.7 # same as adding an opacity of 0.5 to a black background

excerpt: "Formulating and solving an optimization problem for raw material placement and cargo transportantion within the Ignisterra warehouse"
---


Wouldn't you feel that something is wrong if you left your higher quality products mixed with those you don't care about so much? How would you feel if, in addition to that, you are leaving them in a place that you know will be damaged in the blink of an eye, such as the open air, for example? Well, that's how the managers of [Ignisterra](https://ignisterra.com/), the world's largest lenga producer, felt when they realized that the most profitable inventory was being stored outdoors in a place prone to rain, such as southern Chile. Thus, together with students of Industrial Engineering from my university, we were asked to analyze the situation associated with inventory management in the distribution and manufacturing plant of Ignisterra.

## The Problem

At the beginning, the situation was quite ambiguous: We had to completely analyze the process of receiving, storing and dispatching cargo in order to understand exactly what was happening in that warehouse. However, by working closely with the operations manager, the warehouse manager and the plant workers, we were able to understand how these processes were handled and how this company continued to operate normally in the face of this alleged inventory problem for which we were asked for advice.

Within our situation analysis process, the first question we asked ourselves was why does this company keep all its raw material, independent of quality? Clearly Ignisterra was storing material with little profitability and neglecting valuable material. At that time, it was explained to us that there were [several types of wood](https://ignisterra.com/pages/madera), based on their quality, cutting thickness, density, etc., and we were also explained that one of its main business policies is **the use of all wood extracted from Tierra del Fuego, regardless of type or quality**. This made us understand that the wood superhabit could be mainly of low quality wood, but we had to do a deeper analysis to validate that assumption.

Due to the suspicions we had, we requested the necessary information to be able to graph the stock of inventory stored during the last 4 and a half years:

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/StackPlot.png" alt="Stackplot Time Series: Stock of inventory per type of Wood per Year">
</p>

Indeed, it can be seen that the company's inventory levels have continuously increased over the last 5 years, reaching a total inventory of **5796.12 cubic meters**. Of the total, **Standard** class wood is the wood with the largest presence in Ignisterra's warehouse, contributing **75.7%** of the final inventory. In second place is the **Economic** type of wood, with **10.9%**. This is a clear indication that regular or poor quality woods have had a higher arrival rate than their departure rate in recent years.

If we add to the increase in inventory the fact that it is disorganized, with mixed woods independent of their visual quality, and considering that each order considers different types of wood, thicknesses and lengths, the problem intensifies exponentially. This has triggered increases in delivery times, mainly due to the lack of knowledge of the location of the requested woods.

Although this generalized inventory disorder has had an impact on the operation, it does not impede the operation of the company in terms of remanufacturing the material. The following diagram summarizes the information gathered in our analysis:


<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Fishbone_diagram.PNG" alt="Ishikawa Diagram">
</p>

## Problem Formulation

Due to Ignisterra's corporate culture, focusing our efforts on reducing inventory was a bad idea, as the company was already working on some proposals associated with the addition of new products to its portfolio, such as the production of biomass and the sale of firewood. That is why we focused on the second major cause of inventory disorder, which was the poor distribution of the area of the woods and consequently mixing them between areas.

### Spatial Dimensions

As can be seen in the layout shown below, the spatial dimensions of the problem are as follows:

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Layout_Company.PNG" alt="Company_Layout">
</p>

- Factory interior: 625.5 square meters.
- Exterior of the factory with roofed space: 864 square meters.
- Outside the factory without roofed space: 2698 square meters, of which 144 square meters will be considered within the scope of the problem.

### Objectives & performance measures

In order to maintain a layout that allows the fuel to be used by the storage machinery to be reduced, the different types of wood will be distributed in such a way as to **minimize the total distance traveled by these machines considering a fixed rotation level for each one of them**.

### Assumptions & Constraints

The most relevant assumptions and restrictions of the problem presented are the following:

- Only a maximum of 4 cubic meters of wood can be placed per square meter.
- The woods of first selection, prime, color and selected must be indoors.
- Standard and economy woods can be outdoors (without a roof).
- All pallets made of wood of the same quality will be considered equal, regardless of their thickness and width.
- There are two storage areas: wood for internal consumption will be stored inside the sawmill, while wood for export will be stored in the exterior sheds. That said, the wood for export is as follows: First selection, select, select color and prime (70% of the total prime wood). On the other hand, the woods of internal consumption are the following: Standard, color and prime (30% of total prime).
- Twenty-one areas have been defined for the location of wood in the sawmill, which have different sizes. However, each area can only be occupied by one type of wood.

### Formulation

The formulation of the problem, after the relevant research, is as follows:

#### Variables


<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Variables.gif" alt="Variables"> 1 if lumber i takes position j within the sawmill, and 0 if this is not the case.

#### Objective Function

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Objective_Function.gif" alt="Objective Function">
</p>

We seek to minimize the total distance traveled by the loading machinery of the warehouse given a level of rotation of each wood.

#### Constraints

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Constraint_1.gif" alt="Constraint_1">
</p>

The number of spaces dedicated to a type of wood must satisfy the amount of existing stock of that type of wood.

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Constraint_2.gif" alt="Constraint_2">
</p>

Only one type of wood can be assigned to each space.

#### Parameters
<p></p>

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Turnover.gif" alt="Turnover"> Rotability of wood type i (Dimensionless).

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Distance.gif" alt="Distance"> Distance between storage site j and loading/unloading site. Manhattan distance (meters).

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Space.gif" alt="Space"> Authorized storage space in box/location j (cubic meters).

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Stock.gif" alt="Stock"> Space needed to store all type i wood (cubic meters).

## Results & Impact

By solving the problem formulated above, we obtain the following distribution of woods within the warehouse:

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Optimized_Solution.PNG" alt="Optimal">
</p>

The impact of our solution becomes evident when calculating the distance travelled by the forklifts considering the loading and unloading of wood in the year 2014. By making this comparison with the current layout versus the optimal one, we can appreciate that the distances traveled by the cranes are 5.5% less with the optimal distribution, which leads to substantial savings in fuel for the company, not to mention the increase in the company's load dispatch capacity.

## Next Steps

- Sensitibity Analysis.
- Implementation Plan.
- Procedures to maintain the layout.