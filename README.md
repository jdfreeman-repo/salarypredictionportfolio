# Salary Prediction Project

## DS Methodology 
As in all my projects, I follow the 4D Data Science Framework, which in brief is:
#### Define
What is the KPI we are trying to impact and how does it affect the company
#### Discover 
EDA, obtain, clean, explore data, establish baseline outcomes, hypothesize a solution
#### Develop Solution
Feature engineering, create and test models, and select the best model
#### Deploy Solution 
Automate pipeline, deploy solution, and measure efficacy - how much is it positively affecting the KPI; get feedback and improve model 

### Define
[Link to the DEFINE section](https://github.com/jdfreeman-repo/salarypredictionportfolio/blob/master/salaryPredictionNotebook.ipynb#DEFINE)
In this project, our company needs to determine the "correct" salary for new job postings based on existing job postings. 
In the existing process, each department is left to determine the salary for their postings.  
It is projected that having this model will:
Take the guesswork out of the salary-determining process for new job postings
Help standardize salaries for new job postings across departments. 


### Discover
[Link to the DISCOVER section](salaryPredictionNotebook.ipynb#DISCOVER)
We are supplied the features and target data from current job postings, and features data from a test data set.  The analysis process reveals the following features:  
jobType              
degree              
major                  
industry               
yearsExperience        
milesFromMetropolis  
And the target, salary.

As part of this process, I use IQR - Interquartile Range rule to identify potential outliers, using the definition of Minimum and Maximum to calculate these values. IQR is a somewhat arbitrary value, but has some value in spotting outliers.  

I use label encoding to create a correlation heatmap.
From this heatmap, there is shown a greater degree of correlation of jobType to salary, followed by degree, major, and yearsExperience.  Using this analysis enables me to determine if there is collinearity, and so, determine if dimension reduction is needed.  Also, there is some correlation with industy to salary, and a negative correlation betwen milesfromMetropolis to salary. This is because salaries are less in rural and suburban areas than metropolitan areas.
Overall, this shows that this is a clean dataset that can be modeled as is.  

My baseline shows the following MSE from the median salary for each category:
companyId - 1498.91
jobType - 963.93
degree - 1257.61
major - 1284.07
industry - 1367.12

## Develop Solution
[Link to the DEVELOP section](salaryPredictionNotebook.ipynb#DEVELOP)
In this project, I create new features needed to enhance the model, and also shuffle the train dataset and reindex it. This improves the cross-validation accuracy. Iuse one-hot-encoding to encode the category features. Since in the EDA, I determined that the companyId does not correlate well to the salary, i.e. there is very small difference between salaries based on company, I removed companyId from the categories.  

Based on EDA, I test 4 different models that I think will improve the results over the baseline model shown above.  Based
on the EDA, I use:  
Linear Regression - the boxplot and distplot show that there is a good degree of correlation between many of the features and the target (salary).   
make_pipeline - this will use Standard Scalar, and PCA to transform the data before feeding it to the Linear Regression.  
RandomForestRegressor - Because the features are broken into categories, a decision tree method is very likely to give good results.  The Random Forest Regressor prevents over-fitting by decision trees by random sampling of data points, and random subset of features considered when splitting nodes.  
GradientBoostingRegressor - This method is an ensemble of weak prediction models, typically decision trees, to make a strong predictor in stages (boosting).  Each subsequent model will fit to the residual of the previous model, thus providing improvement with each stage.

I run a test on these 4 model types using 5-fold cross validation to determine which model provides the lowest MSE, which I use for measure of efficacy.  

## Deploy Solution
[Link to the DEPLOY section](salaryPredictionNotebook.ipynb#DEPLOY)
The best model from the test runs above is run for Production against the test dataset.  At the conclusion of the run, the following files are saved from the run:
model.txt - details the model and hyper-parameters used for this run
features_importances.csv - the relative importance of the features in determining the predictions
predictions.csv - the predictions for the test input data.    
In the next updates of this deployment, I will create python scripts from the jupyter notebook, putting the utilities in a separate python module to be used for multiple projects.  
As part of this deployment, I create a visualization of the features importance.  This will be valuable to HR and hiring managers because it shows the relative importance of features in setting salary for job postings.  
I also create a presentation showing the graphics created during EDA, and also a feature importance bar graph.

## Results
### Conclusion
The analysis of the dataset enables us to generate a model that our company will use to determine the appropriate salary for new job postings.  The EDA analysis predicts that there will be a strong correlation between degree/major and salary, with jobType as a lesser factor.  Several Regression models were tested in this project, with the RandomForestRegressor, having the best MSE of 366.  This is much superior to the baseline model, whose best results is a MSE of 963.  As shown in the Features Importances graph, the RandomForestRegressor shows that yearsExperience and milesFromMetropolis are the most important factors, with the role of Janitor having a significant weighting, on it's own.

### Forward Plans/Improvements
In the forward plans for project, I plan to perform additional feature engineering, such as binning based on yearsExperience, jobType and Degree.  This may yield better results as these features seem to provide scatter to the regression algorithm, but can be readily binned using some intuition.  Also, I plan to try additional models such as Polynomial Linear Regression.  While the EDA shows that there is a linear relationship with each of the features, yearsExperience and milesFromMetropolis, the combination of the two may be of a higher degree.
