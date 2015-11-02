---
layout: post
title: Project 3: Classifying MOOCs and D3
---

Last week I completed my 3rd Data Science Project.  I analyzed course data from 16 different MOOCs and used a variety of machine learning algorithms to fine tune my predicitve models.  Along the way I learned about several different types of classification, imputing missing data and D3 visualizations.

The main point of this project was to explore different types of classification techniques such as **Logistic Regression, Gaussian Naive Bayes, K Nearest Neighbors, Support Vector Machines, Decision Trees** and **Random Forests**.  In order to use these models, I needed to look at an aspect of the data set that was categorical.  I noticed that one feature of the data was level of education, which was classified into **less than secondary, Secondary, Bachelor's Degree, Master's Degree or Doctorate**.  I ended up drawing a line between Secondary and Bachelor's Degree and making it a binary classification exercise.  

After writing my own grid-search algorithm (Sklearn's took too long to run and didn't give me the output the way I wanted), I was able to predict level of education with an average of 85% accuracy (no lower than 78% accuracy in any course and as high as 94% in some courses).  Equipped with this fairly accurate predictor, I decided to impute whether or not a student had at least a Bachelor's degree for most of the rows for which Level of Education was previously missing.  This helped me salvage about 10% of my data set overall.  More importantly, for some of the MOOCs, imputing allowed me to restore over 25% of the incomplete data, which in turn greatly improved my prediction accuracy for student grades in those courses.




