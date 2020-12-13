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

420 M food items purchased by 1.6 M fidelity
card owners who shopped at the 411 Tesco stores in Greater London over the course of the entire year
of 2015, aggregated at the level of census areas to preserve anonymity. F

