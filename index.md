---
layout: page
title: Tesco paper extension
subtitle: We are three young Jedis seeking for the well being of citizens in the Galactic Republic.Too much young padawans are overweight and we want to help them understand why.
comments: true
bokeh: true
---


In this blog post, we'll give to the avid P-ADA-wan an overview of the paper [Tesco Grocery 1.0, a large-scale dataset of grocery purchases in London](https://www.nature.com/articles/s41597-020-0397-7). This paper was made by some experimented Jedi that used this dataset wisely. We tried to follow their path by extending the paper, as explained in the other blog posts.



# I - Introduction to the Tesco paper 

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

The first group of variables expresses the Tesco **penetration** in an area. The representativeness, which is the ratio between the number of customers in an area and the population, is computed. A normalized representativeness is also computed.

Then, the authors decided to compute the average product bought for each area. They computed multiple **nutrional properties** about this average product, such as the _weight_ or the _volume_, the _energy_, the _energy - density_, and the grams of each indivdual nutrients $nutrients_{i}$. Then, they compute the energy per nutrient, as well as the frequency of each nutrients, and the frequency of energey per nutrient. They managed to compute the entropy of these 2 frequencies. More details of the formulas in the [Appendices](#tesco-aggregation-formula).

The last group of variables is about the **Product categories**. The authors computed the probability distribution of items belonging to the 17 different product categories being purchased in area a and the entropy of that distribution. They also computed the relative weight of products belonging to any category compared to the total weight, and its entropy.

### Data biases and limitations

Several limitations were pointed by the authors:

+ **Representativeness**: As this study collect the grocery purchases from Tesco's customers owning a Clubcard submission, this set of Clubcard owners might not be representative of the overall population.
+ **Coverage**: The concentration of Tesco is higher in the northern part of London, thus some areas have a low penetration rate.
+ **Limited scope**: This dataset only show the grocery purchases at Tesco - thus, the food consumption in restaurants or the food bought at other grocery stores is not included in the dataset.
+ **Average product** As the data is aggregated to create the average product consumed in an area, this is a limitation to any study that requires an average representation at an individual level rather than at a geographical level.

## Technical validation

The authors wanted to show that their aggregated dataset made sense. After a quick explanation on how to select the most representative areas, they linked their created dataset to food-related health outcomes.

### Area representativeness

To ensure the representativeness of the dataset, the authors stated that we can play with the normalized representativeness in order to select the areas with the best ratio between the number of customers and the numbers of residents.

### Validation of health outcomes

The authors joined this Tesco dataset to dataset related to obesity and Type-2 diabetes, as these health diseases are strongly correlated with diet. 

The authors computed the Spearman rank correlation between the energy, the nutrients and the nutrients entropy of the average product in an area and the prevalence of obese and overweight children and adults, as well as diabetes prevalence. Those result are displayed below. We should note that only statistically significant correlations (p < 0.05) are shown.

![Correlation results](https://zupimages.net/up/20/51/rsur.png){: .mx-auto.d-block :}

To build stronger evidences of the link between the dataset and food related illnesses - thus, to show that the food descriptors provided dataset are not proxies, the authors then ran an ordinary least square regression.

Using the two highest-correlated factors and four control variables accounting for demographics, they managed to get a R² ratio of 0.613, which denotes a high goodness of fit. Using only the two factors _energy-carbs_ and $H_{_energy-nutrients}$, the R² ratio remained high (0.56)

These result showed that the dataset make sense, as they found correlations which were expected between the average food product and related health diseases.

For more details about the Paper and the Data itself, the reader is invited to refer directly to the paper. 


Now that we summarized the paper, we can present the goal of our extension and analysis. 
Our first goal is to dig deeper in the analysis of the Children Obesity dataset already presented in the paper. <strong> Ajouter du BS sur pq on fait ça </strong>

We will then geographically visualize the different food type consumption in London and then analyse the link with the Mean Income by area. <strong> Pareil ici </strong>

# II - Children overweight prevalence

In the first part of our extension, we wanted to explore more in-depth the [Tesco Grocery dataset](https://www.nature.com/articles/s41597-020-0397-7), and especially the link between food and children overweight prevalence.

We wanted to explore deeper the papaer and establish a stronger correlation between children's overweight prevalence and food consumption. Indeed, we firstly thought that obesity among children and grocery shopping were strongly correlated.

## Introduction and expectations

We decided to explore this part of the paper as we thought that the paper didn't go in-depth enough. Indeed, the paper focused on how the authors aggregated the data rather than on establishing strong links between childhood obesity and grocery shopping. The authors computed the Spearman correlation between obese and overweight children at reception (4-5 y.o) and 6 y.o, and food consumption estimators (energy, nutrients, nutrients entropy). 

Our goal was thus to find correct predictors to produce a machine learning model that would be able to estimate overweight prevalence, given the information of an area. As we had multiple machine learning models at our disposal and more than 200 potential predictors, we thought that we could find a machine learning model that 
would predict the values correctly. 

## Our first ML benchmarking

### Choice of our predictors and target

After joining the grocery dataset with the children obesity one on the area_id, we obtained a DataFrame with 202 potential predictors and 7 labels for only 544 data points.

We had to select the correct predictors and the correct target. Indeed, our model will surely overfit with 202 predictors for a sample of size 544, and we have to focus on one target value.

The data for overweight children is more sparse than the one for obese children. Indeed, obese children are included in the overweight ones, so the overweight prevalence for children will always be greater than the obesity prevalence in the same area. That's why we chose to use overweight children as our predictor. We now had to choose between reception (4-5 y.o. children) and 6 y.o. children. We chose to select 6 y.o. children data, as they have had more time to eat and to have weight changes because of their food consumption.

As the dataset is based in London, we decided to follow the guidelines of the [National Health Service](https://www.nhs.uk/conditions/obesity/causes/)
to select our predictors. 

![NHS guidelines](https://zupimages.net/up/20/51/zb78.png){: .mx-auto.d-block :}

We analyzed these guidelines and discovered that there were 4 main columns we can extract from the dataset that is potential cause to obesity: calories, fat, sugar, and alcohol. As children of 6 years old are not likely to drink alcohol, we will focus on the 3 columns _fat_, _sugar_ and _energy_tot_.

### Data preprocessing

We had to sanitize our data and preprocess it. We removed NA values, and standardize our input. We then splitthe data into a train set and a test set.

### Machine Learning Models

Our predictors, as well as our label target, are float numbers. Thus, it was a supervised Regression problem: we decided to benchmark our predictors on various ML algorithms related to the supervised regression problems. Those models are:

+ Linear regression
+ Ridge model
+ Support Vector Machine (SVM) 
+ Tree 
+ Gradient Boosted Regression (GBR)
+ ADABoost

We decided to benchmark on different models to keep the model scoring the best, and to use the [R² score](https://en.wikipedia.org/wiki/Coefficient_of_determination) to compare the different models.

### Result

Surprisingly, our results for this ML model were not good. 

![Result of our first ML model](https://zupimages.net/up/20/51/3tsv.png){: .mx-auto.d-block :}

We can see that the R² score is always negative, which means that our models have a worst prediction than if we stated that the target value for all of our test data was always the mean of the target values. Thus, our machine learning models predict poorly the target.

## Trying out with different predictors

### New predictors, same pipeline

We tried to stick with the NHS guidelines about obesity. This time, we decided to stick with the relative amount of energy for fat and sugar instead of their absolute values in the average product. It might produce different results, and we wanted to test it out.

Our predictors for this model are _f\_energy\_sugar_, _f\_energy\_fat_ and _energy\_tot_. We decided to stick with the same target, overweight prevalence for 6 y.o. children.

After sanitizing, standardizing, and splitting the data into train and test sets, we applied our ML algorithms to it.

### Results and analysis

Again, our ML models predict poorly the target, as they all have a negative R² score. 

![Result of our second ML model](https://zupimages.net/up/20/51/922o.png){: .mx-auto.d-block :}

But why do we have bad results while we follow NHS guidelines? Does that mean that we did something wrong?

We tried to analyze our result and explain why we didn't find any machine learning model fitting our data.

+ NHS warns about the number of calories eaten for obesity, but not for type-2 diabetes. It is because gaining weight is due to having more calories ingested than burnt. Thus, lacking some physical activity data might be more damageable for our study, as it is paramount in obesity analysis.
+ We have the average product for each area, but we don't know how much "average product" will be eaten by the children. As gaining weight is a matter of calories ingested, it is crucial information that we lack.
+ We analyze children's overweight prevalence but our data is the average product bought at Tesco. On top of the limitations pointed out in the Tesco paper (TODO: add a link to the tesco presentation blog, limitation section), we can add that we don't know if the children eat differently than their parents.
+ When babies are born, some of them weigh higher than others. It is a natural condition, that is "beaten" by one's diet and physical activity over time. However, by capturing overweight children prevalence at 6 y.o., maybe the children haven't lived long enough to have their eating habits changed significantly their weight.

## Using food categories

We continued to work on this dataset and we wanted to try out using food categories proportions to have a correct ML model. As we decided to focus on those food categories for the other part of our extension, it made sense to use them in this project.

After the usual data processing and splitting, we found these results: 

![Result of our ML model with food categories](https://zupimages.net/up/20/51/87se.png){: .mx-auto.d-block :}

With positive R², we have done better with this third ML model than with the 2 previous ones. We decided to perform an ordinary least squares statistical analysis. 
After discarding the non statistically significant product categories, we plotted the result with the 95% confidence interval. 

![Plot of the OLS analysis with food categories](https://zupimages.net/up/20/51/yp1a.png){: .mx-auto.d-block :}

These results were quite surprising. Even though it makes sense for some food categories, for example, the soft drinks, other results are in contradiction with standard food advice. For example, we find that the highest correlated food category is fish.

Instead of overinterpreting these results, we wanted to use them to give a warning about data analysis in general. It is important for a data scientist to not try to find meaning when there's none, as one can always find meaning if he desires. We'll encourage young padawans to try out the [Spurious correlations website](https://www.tylervigen.com/spurious-correlations) for examples.

In our case, even though we have statistically significant correlations between food categories and children's overweight prevalence, we don't want to state that one causes another, as we don't agree with it. 

We also learnt that our intuition about correlation might be wrong sometimes. We thought that obesity among children and grocery shopping was correlated, but it was not the case.

# III - Geographical Visualization
As presented in the previous part, we have at our disposal a huge dataset containing the food purchases made in the Tesco shops within the boundaries of London. Faced with all this data, we are a little lost and ask ourselves where to begin. And suddenly we remember that we learned, during our Padawan formation, how to become a data vizard. We take out our most beautiful tools and begin to make some visualizations to understand better our data.
We have many features available for purchases. We notice that the Tesco paper did not deeply explore the different **food categories**. Therefore, we decide to focus on these.

## What we want to visualize
We wonder if the Tesco consumers buy the same products and in the same quantity all over the city. In particular, we wonder :
- Are there some food categories that are preferred in certain regions? 
- Are there some others that are equally bought in the city? 
- Can we draw a pattern from this visualization?

To try to answer all these questions, we make plots of the distribution of a certain food category all over London. We consider a mean of the fraction of purchase of each food category in a given ward.


## Visualizations 
Each plot below represents the mean proportion of purchase of a certain food category per ward. To see all the plots, you can click on this [link](maps.html).
On top of that, by moving your mouse all over the map you can discover the exact mean proportion of purchase of a certain food category, the mean income and the median income in each ward.

{% include test.html %}

## Observations
We see that the distributions of proportion of purchases of food category are pretty different.
Some food categories shows uniform distributions whereas some others show very disparate distributions.
For instance, the comsuption of beer, soft drinks and tea/coffee seem to be the same all over the city. In contrary, the consumption of meat is 6 times higher in the northern and centered wards of London compared to some southern ward. We observe almost the same thing for the poultry.
 The distribution of ready made, sweets and grains seem to be more important in the southern, eastern and western edge of the city. It is particularly true for ready made where the consumption of this category is almost 50 time higher between some edge parts and some city centered parts.

## What are the causes of these observations ?

Many factors can influence these observations. 
First, let’s not forget that the data we use comes from the Tesco shops only . Indeed, people coming to Tesco does not represent all the inhabitants of a ward and the Tesco paper has shown that the representativeness of this data is to some extent limited.
Therefore, we should not exclude the hypothesis that if we had the data of all food retail markets, we would observe different results. 
However, some patterns seem to appear clearly in our plots. For instance, we can’t help but notice that the distribution of meat (read meat and poultry) is higher in the northern and centered parts of London.
Meat is a pretty expensive product and we wonder if there can be a link between the income of people and their consumption of meat (at least at Tesco shops). It would not be so surprising to observe that in the wards where people are wealthier, they buy more meat. 
In addition, it is possible that the observation about the important readymade comsuption in some wards is also linked with the income of the people. Middle-class people generally have healthier diets than lower-class people and would then consume less readymade products.

## A new hope 
Intrigued by these observations, we will try to see now if there exist a correlation between the income of people and the proportion of purchases of some food category in the Tesco shops.

# IV - Analysis of the link between Food category proportion and Income

## Presentation of the dataset

Our data comes from the [Household Income Estimates for Small Areas](https://data.london.gov.uk/dataset/household-income-estimates-small-areas) dataset. 
This dataset presents Mean and median average gross annual household income for, Lower SOAs, Middle SOAs, Wards and Boroughs, London, 2001/02 to 2012/13

We vizualize here the data presented in the dataset. In an optic to see how the Mean and Median income evolves between the years. 

![Mean, and median ](./assets/img/Mean_Median_WARD.png){: .mx-auto.d-block :}

As we can see on the graph above, there are some outliers but we can see that overall the Mean and Median outcome tend to go up ! 

![Mean of mean](./assets/img/Mean_Of_Mean_WARD.png){: .mx-auto.d-block :}

When taking the mean of all areas we observe a similar constant augmentationwe assume that an overall increasing trend is exected and we decide to use the revenue for year 2012/13 as it is the closest data we have to 2015. Right below, we present an interactive map to visualize the income.

{% include income.html %}

## Correlation between Fraction of each Category and Income 

### Different categories 

So in this part we compute the correlations between the percentage of each food category and the mean income. Note that we obtain similar R values with median. We thus can conduct the same analysis on both dataframes and choose to do it on this one. 
We present below the graph showing the correlations between Food category fraction and Mean Income, for all three censuses available. 

![Correlation_results_income](./assets/img/Correlations_cat_3_census.png){: .mx-auto.d-block :}

First it's worth noting what these correlations mean. A positive correlation means that having a greater fraction of all the products belonging to a category relates to a higher Mean Income.
So it does not mean that people with high revenues don't buy any product of a negatively correlated category or even that they buy less. It means that the fraction of their shopping bag item belonging to that category is smaller. 
Taking the example of grains, this category being negatively correlated with income means that people with lower income overall have a bigger part of their alimentation composed of grains. 

So let's analyse these results. 
- The *negatives correlation* of Income with  **Sweets**, **Spirits**, **Soft Drinks**, **Fats Oils** and **Grains** is quite interesting because it shows that poorer neighbourhood show a higher part of the groceries dedicated to less healthy food. The correlation with spirits is also quite interesting as it might induce that poorer neighbourhood might be more prone to alcoholism or at least a higher consumption of stronger alcohol. 

- The *positive correlation* of Income with **Fruit and Vegetables**, **Fish** and **Dairy** product is also quite interesting as we here find that 'healthier' products take a bigger part of the shopping bag in areas where the income is higher. Fruit and vegetables can also be explained as people with higher incomes might be more prone to be vegetarian. **Wine** is also one of those positively correlated food categories and we can explain that as wine being a 'fancy drink'. 

- The fraction of **water** being negatively correlated with water seems quite surprising at first but we can actually make a point by finding that tap water in poorer neighbouhoods might be of lower quality forcing people there to buy more bottled water. People in richer neighbourhood might also be more likely to buy a water filtering system. It might also be that people might tend to buy big packs of water bottles in huge malls situated in industrial areas where the global Income might be lower and thus correlating it with lower Income.

- Fraction of tea and coffee being negatively correlated with income is not very, so is the positive one with beer. 

- We let aside the correlation that are not statistically significant or very low, namely eggs, poultry, red meat and ready-made food. Although for the latter, we can suppose that any worker is sensible to buying ready-made food for lunch thus it does not really correlates with the Income. 

### Different Nutrient analysis

Even though this part was already quite developped in the original paper, we computed the same correlation as above using this time nutrients energy instead of food categories. Here are the results : 

![Correlation_nutrients_results_income](./assets/img/Correlations_nutrients_msoa.png){: .mx-auto.d-block :}

 According to the World Health Organization, the three best dietetary habits to prevent conditions associated with the metabolic syndrome are:
 - limiting the intake of calories 
 - having a nutrient-diverse diet
 - favoring the consumption of fibers and proteins over sugars, carbohydrates, and fat.

 The correlations we find with income shows that people that areas where people follow these recommandation food-wise tend to have higher Income, as they are directly correlated. 

### Important to qualify these remarks : 

- The people buying food in a given area don't necessarily live in the area. 

- It might be that some big shopping center are situated in industrial places outside the city, thus not much people live there so the mean income would be quite low even though all sorts of people go shopping there. 

- We also can note that areas with really high median incomes might not be the most populated ones (finacial districts ... ) and might also not be the shopping place of a lot of people. Thus this most likely gives skewed data.


## The Income strikes back 

So we decided to run some regression techniques to see if we can predict the Income of the Area based on the fraction of consumed food categories. 
We'll study the case for MSOA as it is the middle sized and we can witness the same phenomena for the three areas ! 

We will use this samples for training as they are the one we found having a statistically significant correlation with Mean Income.
**f_beer, f_dairy, f_fats_oils, f_fish, f_fruit_veg, f_grains, f_meat_red, f_soft_drinks, f_spirits, f_sweets, f_tea_coffee, f_water and f_wine**.


We tried the same Machine Learning Regressors to predict Mean Income and see how well it performs. We kept 70% of our data for training. 
We found that the Linear regressor gave the best R-squared value, so we will continue our analysis with this method. This R-squared value being high indicates that these features allow a good prediction of the Mean Income. 

![Residuals_Regression](./assets/img/residuals_regression.png){: .mx-auto.d-block :}

As we can see, the probability of being off by more than 10000 pounds is quite low. This shows us that on top of being quite accurate, our regression is rarely off by more than 10000 pounds. Thus we can see that the select food categories allow us to get a quite good prediction of the income.  

![Plot_Regression](./assets/img/Regression_Plot.png){: .mx-auto.d-block :}

Finally 
# Conclusion 

# Notes 
- Preciser que on prend pas en compte l'inflation 
- All precise data are available in the note book 