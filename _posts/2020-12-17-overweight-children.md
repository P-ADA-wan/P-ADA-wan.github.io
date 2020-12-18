---
layout: post
title: A correlation between overweight children and food ?
subtitle: Our researches to link overweight children prevalence and food consumption
cover-img: /assets/img/children-obese.jpeg
comments: true
---

In this part of the extension, we wanted to explore more in depth the [Tesco Grocery dataset](https://www.nature.com/articles/s41597-020-0397-7), and especially the link 
between food and children obesity or overweight.

We wanted to explore deeper and make stronger correlation between children overweight prevalence and food consumption. Indeed, our first thought about this question
was that obesity among the children and grocery shopping was strongly correlated.

# Introduction and expectations

We decided to explore this part of the paper as we thought that the paper didn't go in depth into it. Indeed, the paper focused on how the authors aggregated the data
rather than on establishing strong links between children obesity and grocery shopping. The authors computed the Spearman correlation between obese and overweight 
children at reception (4-5 y.o) and at 6 y.o, and food consumption estimators (energy, nutrients, entropy of nutrients). 

Our goal was thus to find correct predictors to produce a machine learning model that would be able to estimate overweight prevalence, given the informations of an area.
As we had multiple machine learning models at our disposal and more than 200 potential predictors, we thought that we could find a machine learning model that 
would predict correctly the values.

# Our first ML benchmarking

## Choice of our predictors and target

After joining the grocery dataset with the children obesity one on the area_id, we obtained a DataFrame with 202 potential predictors and 7 labels for only 544 datapoints.

We had to select the correct predictors and the correct target. Indeed, our model will surely overfit with 202 predictors for a sample of size 544, and we have to focus 
on one target value.

The data for the overweight children is more sparse than the one for the obese children. 
Indeed, obese children are included in the overweight children, so the overweight prevalence for children will always be greater than the obesity prevalence in the same area.
That's why we chose to use overweight children as our predictor. We now had to choose between reception (4-5 y.o. children) and 6 y.o. children.
We chose to select 6 y.o. children data, as they have had more time to eat and to have weight changes because of their food consumption.

As the dataset is based on London, we decided to follow the guidelines of the [National Health Service](https://www.nhs.uk/conditions/obesity/causes/)
to select our predictors. 

![NHS guidelines](https://zupimages.net/up/20/51/zb78.png){: .mx-auto.d-block :}

We analyzed these guidelines and discovered that there were 4 main columns we can extract from the dataset that is potential cause to obesity: calories, fat, sugar and alcohol.
As children of 6 years old are not likely to drink alcohol, we will focus on the 3 columns _fat_, _sugar_ and _energy_tot_.

## Data preprocessing

We had to sanitize our data and preprocess it. We removed NA values, and standardize our input. We then splitted the data into a train set and a test set.

## Machine Learning Models

Our predictors, as well as our label target, are float numbers. Thus, it was a supervised Regression problem: we decided to benchmark our predictors on various ML algorithms related to the supervised regression problems. Those models are:

+ Linear regression
+ Ridge model
+ Support Vector Machine (SVM) 
+ Tree 
+ Gradient Boosted Regression (GBR)
+ ADABoost

We decided to benchmark on different models to take the model having the best score. We decided to use [R² score](https://en.wikipedia.org/wiki/Coefficient_of_determination) to compare the differents models.

## Result

Surprisingly, our results for this ML model were not good. 

![Result of our first ML model](https://zupimages.net/up/20/51/3tsv.png){: .mx-auto.d-block :}

We can see that the R² score is always negative, which means that our model have a worst prediction than if we established that the target value for all of our test data was the mean of the targets values. Thus, our machine learning models predict poorly the target.

# Trying out with different predictors

## New predictors, same pipeline

We tried to stick with the NHS guidelines about obesity. This time, we decided to stick with the relatives amount of energy for fat and sugar instead of their absolute values in the average product. It might produces different result, and we wanted to test it out.

Our predictors for this model are _f\_energy\_sugar_, _f\_energy\_fat_ and _energy\_tot_. We decided to stick with the same target, overweight prevalence for 6 y.o. children.

After sanitizing, standardizing and splitting the data into train and test set, we applied our ML algorithms on it.

## Results and analysis

Again, our ML models predicts poorly the target, as they all have a negative R² score. 

![Result of our second ML model](https://zupimages.net/up/20/51/922o.png){: .mx-auto.d-block :}

But why do we have bad results while we follow NHS guidelines ? Does that mean that we did something wrong, or NHS were false in their guidelines ?

We tried to analyze our result and explain why we didn't find any machine learning model fitting on our data.

+ NHS warns about the amount of calories eaten for obesity, but not for type-2 diabetes. It is because gaining weight is due to having more calories ingested than burnt. Thus, lacking some physical activities data might be more dommageable for our study, as it is paramount in obesity analysis.
+ We have the average product for each areas, but we don't know how much "average product" will be eaten by the children. As gaining weight is a matter of calories ingested, it is a crucial information that we lack.
+ We analyze children overweight prevalence but our data is the average product bought at Tesco. On top of the limitations pointed out in the Tesco paper (TODO: add a link to the tesco presentation blog, limitation section), we can add that we don't know if the children eat differently than their parents.
+ When babies are born, some of them weights higher than others. It is a natural condition, that is "beaten" by one's diet and physical activity over time. However, by capturing overweight children prevalence at 6 y.o., maybe the children haven't lived long enough to have their eating habits changed significantly their weight.

# Using food categories

As we continued to work on this dataset, we wanted to try out using food categories proportions to have a correct ML models. As we decided to focus on those food categories for the other part of our extension, it made sense to use them in this project.

After the usual data processing and splitting, we found these results: 

![Result of our ML model with food categories](https://zupimages.net/up/20/51/87se.png){: .mx-auto.d-block :}

With positive R², we have done better with this third ML model than with the 2 previous ones. We decided to perform an ordinary least squares statistical analysis. 
After discarding the non statistically significant product categories, we plotted the result with the 95% confidence interval. 

![Plot of the OLS analysis with food categories](https://zupimages.net/up/20/51/yp1a.png){: .mx-auto.d-block :}

These results were quite surprising. Even though it makes sense for some food categories, for example the soft drinks, other results are in contradiction with 
the standard food guidelines. For examples, we find that the highest correlated food category is fish.

Instead of overinterpreting these results, we wanted to use them to give a recap about data analysis in general. It is important for a data scientist to not try to find a meaning when there's none, as one can always find a meaning if he desires. We'll encourage young padawans to try out the [Spurious correlations website](https://www.tylervigen.com/spurious-correlations) for examples.

In our case, even though we have statistically significant correlations between food categories and children overweight prevalence, we don't want to state that one causes another. 