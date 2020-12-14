---
layout: post
title: Tesco Grocery 1.0 paper
subtitle: A brief overview of the Tesco Grocery 1.0 paper
cover-img: /assets/img/tesco_logo.jpg
comments: true
---

In this blog post, we'll give to the avid P-ADA-wan an overview of the paper [Tesco Grocery 1.0, a large-scale dataset of grocery purchases in London](https://www.nature.com/articles/s41597-020-0397-7). This paper was made by some experimented Jedi that used this dataset wisely. We tried to follow their path by extending the paper, as explained in the other blog posts.

## Credits

As young Jedi, we would firstly like to thank the author of this paper, [Lucas Maria Aiello](https://www.nature.com/search?author="Luca Maria+Aiello"), [Daniele Quercia](https://www.nature.com/search?author=%22Daniele+Quercia%22), [Rossano Schifanella](https://www.nature.com/search?author=%22Rossano+Schifanella%22) and [Lucia Del Prete](https://www.nature.com/search?author=%22Lucia+Del%20Prete%22) for their work on this paper. 

We would also like to thank the [Data Science Lab](https://dlab.epfl.ch/) of [EPFL](www.epfl.ch), as this project is part of the [Fall 2020 Applied Data Analysis course](https://dlab.epfl.ch/teaching/fall2020/cs401/).

## Introduction 

The Tesco Grocery 1.0 dataset is a record of over 420 M food items, purchased by 1.6M fidelity card owners across Greater London. The authors aggregated the data at different levels, using the same geographical delimitations as the [Office for National statistics](https://www.ons.gov.uk/), dividing into different granularities: LSOA (Lower Super Output Area), MSOA (Medium Super Output Area), Ward,
and borough. They computed the average food product for each areas, and linked it to health outcomes strongly linked to food consumption dataset. Then, they established a correlation between the food consumed and the prevalence of health diseases in an area.

## Data aggregation

We can divide the aggregation of the Tesco dataset in two main parts.

### A link between the product and its nutrition properties

The first aggregation scheme happened for each of the 420 M food items entry. Each entry was under the form {_customer area, GTIN, timestamp_}. The **GTIN** (_Global Trade Item Number_) is used by companies to uniquely identify their trade items globally. The authors joined each entry with its corresponding nutrition informations, such as {_total energy, net weight, fats, saturated fats, carbohydrates, free sugars, proteins, fibers_}, on top of _volume_ and _relative volume of alcohol_ for drinks.

Then, they computed different data related to the nutrients informations, such as the total energy in kilocalories (_kcal_), as the dataset provides the amount of nutrients in grams (_g_). As each nutrients has a fix _g_ / _kcal_ ratio, the authors were able to compute the total amount of energy, as well as the relative amount of energy related to each nutrients.

On top of this, each food item is associated to one category. The categorization includes 17 non-overlapping classes, available in the [Appendices](#food-categories).

### An aggregation between food purchase and geographical areas

The authors mapped the purchases to geographical areas, as mentionned in the introduction. This aggregation scheme happened for all geographical levels, from LSOA to Borough. This aggregation scheme allowed the authors to have multiple granularity levels, which enable a wider range of studies that might benefit from having either a high number of smaller areas containing fewer datapoints or a lower number of areas characterized by more robust statistics.

Those areas have basic census statistics collected by the ONS in 2015. An exhaustive list of these statistics present in the joined dataset is available in the [Appendices](#geographical-areas-statistics). The authors aggregated the Tesco data relative to each areas, and provided 3 different sets of variables.

The first group of variables expresses the Tesco **penetration** in an area. The representativeness of each area is computed 



## Appendices 

### Food categories : 

fruit & vegetables; grains (e.g., bread, rice, pasta); red meat (e.g., pork, beef); poultry; fish; dairy (e.g., milk, cheese);
eggs; fats & oils (e.g., butter, olive oil); sweets (e.g., chocolate, candies); readymade items (e.g., pre-cooked meal);
sauces (e.g., tomato sauce, soups); tea & coffee; soft drinks (e.g., carbonated sodas); bottled water; beer; wine; spirits.


### Geographical areas stastistics

_population, number of males, number of females, number residents aged [0–17], number
residents aged [18–64], number residents aged 65+, average age, surface area, density of residents_

### Tesco aggregation formula 

$h_\theta(x) = \Large\frac{1}{1 + \mathcal{e}^{(-\theta^\top x)}}$