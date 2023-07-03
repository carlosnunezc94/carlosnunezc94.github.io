---
title:  "Inventory Management Challenges at Ignisterra"
classes: wide
header:
  teaser: /assets/images/ignisterra/ignisterra.jpg
  overlay_image: /assets/images/ignisterra/ignisterra.jpg
  overlay_filter: 0.7 # same as adding an opacity of 0.5 to a black background

excerpt: "Optimizing Raw Material Placement and Cargo Transportation: Formulation and Solution for Ignisterra's Warehouse Operations"
---

The managers at [Ignisterra](https://ignisterra.com/), the globally renowned producer of lenga and the world's largest in its industry, were disconcerted to discover that their highest-quality inventory was being stored in an unfavorable manner. This predicament arose from the fact that valuable products were being mixed with those of lesser importance and left exposed to potential damage. Due to an unexpected increase in inventory, part of its high-end raw materials were stored outdoors, left exposed to potential damage in a region where rainfall is common. Due to the urgency of the matter, I was called to conduct a comprehensive analysis of the inventory management practices and to propose improvements in the storage of materials within the facility.

## The Problem in depth

Initially, the situation presented a considerable degree of ambiguity, necessitating a comprehensive analysis of the cargo reception, storage, and dispatch processes to gain a thorough understanding of the warehouse operations. Collaborating closely with the operations manager, warehouse manager, and plant workers proved instrumental in unraveling the intricacies of these processes and comprehending how the company managed to maintain its regular operations despite the purported inventory issue that prompted our involvement.

Our initial inquiry revolved around the rationale behind Ignisterra's practice of storing all its raw materials, regardless of their varying levels of profitability. During this phase, we were informed about the existence of [various wood types](https://ignisterra.com/pages/madera), characterized by their quality, cutting thickness, density, and other factors. Furthermore, we were apprised of Ignisterra's key business policy, which emphasized the **utilization of all wood sourced from Tierra del Fuego**, irrespective of its type or quality.

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/StackPlot.png" alt="Stackplot Time Series: Stock of inventory per type of Wood per Year">
</p>

From the figure above, it is evident that  Ignisterra's inventory has exhibited a persistent upward trend over the past five years, culminating in a substantial total volume of **5796.12 cubic meters**. Notably, the inventory composition reveals that the largest portion, accounting for 75.7%, is comprised of wood classified as **"Standard"**. In second place is the **"Economic"** wood category, constituting 10.9% of the overall inventory. These statistics unmistakably indicate that the arrival rate of regular or lower-quality wood has surpassed its departure rate in recent years.

In addition to the mounting inventory levels, the disorganized nature of Ignisterra's warehouse further compounds the problem. The lack of segregation based on visual quality results in mixed woods being stored together, irrespective of their respective characteristics. Moreover, each order entails a combination of different wood types, thicknesses, and lengths. Consequently, this exponential combination of factors has led to significant challenges. One notable consequence is the prolonged delivery times experienced by Ignisterra, primarily attributable to the difficulty in locating the specific woods requested due to the lack of organized storage.

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Fishbone_diagram.PNG" alt="Ishikawa Diagram">
</p>

The diagram above effectively captures the key findings and insights derived from our analysis, allowing for a visual representation of the inventory management challenges faced by Ignisterra. It serves as a valuable tool for comprehending the complexities of the current situation and identifying potential areas of improvement.

## Cleaning up the kitchen (I mean the warehouse)

Given Ignisterra's corporate culture and its ongoing endeavors to expand its product portfolio, concentrating solely on inventory reduction proved impractical. The company was actively exploring opportunities to introduce new offerings like biomass production and firewood sales. Therefore, we redirected our attention towards tackling the secondary root cause of inventory disorder: the inadequate allocation of space for different wood types, resulting in their inadvertent mixing across different areas within the warehouse

### Spatial Dimensions
---
<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Layout_Company.PNG" alt="Company_Layout">
</p>

As per the layout provided above, the spatial dimensions of the problem can be delineated as follows:

- Factory interior: 625.5 square meters.
- Exterior of the factory with roofed space: 864 square meters.
- Exterior of the factory without roofed space: 2698 square meters, of which 144 square meters will be considered within the scope of the problem.

### What are we optimizing for?
---
To ensure a layout that minimizes the fuel consumption of the storage machinery, a strategic distribution of the various wood types will be implemented. The objective is to **reduce the total distance traveled by the machines, taking into account a fixed rotation level for each of the wood types.**

By optimizing the placement of different wood types within the warehouse, the goal is to minimize the travel distance required for the storage machinery to access and transport the materials. This approach aims to enhance operational efficiency and reduce fuel consumption by reducing unnecessary movement and optimizing the routing of the machinery.

### Assumptions & Constraints
---

The problem at hand is subject to certain assumptions and restrictions that are vital for its formulation. The most relevant ones are as follows:

- **Maximum Wood Placement**: The maximum allowable amount of wood that can be placed per square meter is limited to 4 cubic meters.

- **Indoor Requirements**: Woods categorized as first selection, prime, color, and selected must be stored indoors.

- **Outdoor Placement**: Standard and economy woods are permitted to be stored outdoors without a roof.

- **Pallet Equivalence**: All wooden pallets of the same quality are considered equal, regardless of their thickness and width.

- **Storage Areas**: Two distinct storage areas exist: one for wood intended for internal consumption, located inside the sawmill, and another for wood designated for export, housed in exterior sheds. The wood for export includes first selection, select, select color, and prime (70% of total prime wood), while woods for internal consumption consist of standard, color, and prime (30% of total prime).

- **Defined Storage Areas**: Twenty-one predefined areas within the sawmill are allocated for wood placement, each with different sizes. Each area can only accommodate one type of wood.

These assumptions and restrictions serve as fundamental guidelines for formulating an optimization problem that addresses the efficient distribution and placement of different wood types within Ignisterra's warehouse.

### Formulation
---
After conducting thorough research and analysis, the formulation of the problem can be summarized as follows:

#### Variables
<br>

![equation](https://latex.codecogs.com/gif.image?\LARGE&space;\bg{black}x_{ij}&space;\mapsto&space;) if lumber i is placed in position j within the sawmill

#### Objective Function
<br>
<center>
<img src="https://latex.codecogs.com/gif.image?\LARGE&space;\dpi{110}\bg{black}min\sum_{i=1}^{n}\sum_{j=1}^{m}r_{i}d_{j}x_{ij}" title="min\sum_{i=1}^{n}\sum_{j=1}^{m}r_{i}d_{j}x_{ij}" />
</center>
<br>

We seek to minimize the total distance traveled by the loading machinery in the warehouse, considering a fixed rotation level for each wood.

#### Constraints

<br>
<center>
<img src="https://latex.codecogs.com/gif.image?\inline&space;\LARGE&space;\dpi{110}\bg{black}\sum_{i=1}^{n}x_{ij}s_{j}\geq&space;N_{i}&space;\rightarrow&space;\bigtriangledown&space;i" title="min\sum_{i=1}^{n}\sum_{j=1}^{m}r_{i}d_{j}x_{ij}" />
</center>
<br>

The allocation of spaces for each type of wood should align with the existing stock quantity of that particular wood type.

<br>
<center>
<img src="https://latex.codecogs.com/gif.image?\inline&space;\LARGE&space;\dpi{110}\bg{black}\sum_{i=1}^{n}x_{ij}&space;=&space;1\rightarrow&space;\bigtriangledown&space;j" />
</center>
<br>

Each space can be assigned to only one type of wood.

#### Parameters
<p></p>


<img src="https://latex.codecogs.com/gif.image?\inline&space;\LARGE&space;\dpi{110}\bg{black}r_{i}&space;\rightarrow&space;" /> Rotability of wood type i (Dimensionless).

<img src="https://latex.codecogs.com/gif.image?\inline&space;\LARGE&space;\dpi{110}\bg{black}d_{j}&space;\rightarrow&space;" /> Distance between storage site j and loading/unloading site. Manhattan distance (meters).

<img src="https://latex.codecogs.com/gif.image?\inline&space;\LARGE&space;\dpi{110}\bg{black}s_{j}&space;\rightarrow&space;" /> Authorized storage space in box/location j (cubic meters).

<img src="https://latex.codecogs.com/gif.image?\inline&space;\LARGE&space;\dpi{110}\bg{black}N_{i}&space;\rightarrow&space;" alt="Stock"> Space needed to store all type i wood (cubic meters).

## Results & Impact

Upon solving the formulated problem, the resulting optimized distribution of woods within the warehouse is as described in the picture below:

<p align="center">
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ignisterra/Optimized_Solution.PNG" alt="Optimal">
</p>

The effectiveness of our solution becomes apparent when evaluating the forklifts' travel distance during the loading and unloading of wood in the year 2014. By comparing the current layout with the optimized distribution, the cranes travel 5.5% less distance in the optimal scenario. This reduction in travel distance results in significant fuel savings for the company while also increasing the load dispatch capacity. The benefits include not only cost savings but also improved operational efficiency for Ignisterra.

## Next Steps

Moving forward, the next steps involve conducting a **sensitivity analysis** to assess the robustness of the optimized solution and its performance under various scenarios, enabling us to identify potential areas for further improvement. Additionally, an **implementation plan** will be developed, outlining the necessary steps and resources required to implement the optimized wood distribution layout within Ignisterra's warehouse. This plan will consider factors such as timeline, budget, and resource allocation to ensure a smooth transition. Furthermore, procedures will be established to maintain the optimized layout, including regular monitoring, periodic evaluations, and potential adjustments based on changing requirements or operational needs. These steps aim to ensure the long-term sustainability and effectiveness of the implemented solution, maximizing the benefits for Ignisterra's warehouse operations.