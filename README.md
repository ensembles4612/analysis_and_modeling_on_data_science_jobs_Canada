# Analysis on Data Science Jobs in Canada with Salary Prediction Flask API

## Project Highlights

* Built salary prediction models for various positions within data analysis area in Canada using LASSO (MAE ~ $16k), Random Forest (MAE ~ $18k) and SVM (MAE ~ $16k)  
* Deployed SVM model as a salary estimator on this [website](https://salary-estimator-shelley.herokuapp.com/) using Flask and Heroku 
* Scraped over 1000 job listings from Glassdoor using python and selenium
####################################################
* Cleaned and Visualized data   
* Optimized Linear, Lasso, and Random Forest Regressors using GridsearchCV to reach the best model. 

## Web Scraping

I adjusted the [web scraper](https://github.com/arapfaik/scraping-glassdoor-selenium) using Selenium to scrape the fulltime job postings from glassdoor.ca for 3 positions -- "data scientist", "data analyst" and "statistician" for a one-month period(2020-05-09 to 2020-06-09) respectively. With each job, I obtained the following: Job title, Salary Estimate, Job Description, Rating, Company Name, Location, Company Headquarters, Company Size, Company Founded Date, Type of Ownership, Industry, Sector, Revenue, Competitors 


## Data Cleaning

After data scraping, I did some cleaning before building the models for salary prediction as follows:
*  Combined 3 datasets and deleted the duplicate rows
*	Parsed numeric data out of Salary
*  Removed rows with NAs in Salary and created column Average Salary 
*	Removed Rating from Company Name text 
*	Transformed Founded date into Company Age
*  Transformed scraped similar job titles into one simplified job title for all positions
*  Created column Seniority from Job Title
*  Created column Job Description Length from Job Description
*	Created columns for if different skills were listed in Job Description: Python, RStudio, Excel, AWS, SAS, Pytorch, sql
 

## EDA
I looked at the distributions of the data and the value counts for the various categorical variables. Below are a few highlights from the pivot tables. 

![alt text](https://github.com/PlayingNumbers/ds_salary_proj/blob/master/salary_by_job_title.PNG "Salary by Position")
![alt text](https://github.com/PlayingNumbers/ds_salary_proj/blob/master/positions_by_state.png "Job Opportunities by State")
![alt text](https://github.com/PlayingNumbers/ds_salary_proj/blob/master/correlation_visual.png "Correlations")

## Model Building 

First, I transformed the categorical variables into dummy variables. I also split the data into train and tests sets with a test size of 20%.   

I tried three different models and evaluated them using Mean Absolute Error. I chose MAE because it is relatively easy to interpret and outliers aren’t particularly bad in for this type of model.   

I tried three different models:
*	**Multiple Linear Regression** – Baseline for the model
*	**Lasso Regression** – Because of the sparse data from the many categorical variables, I thought a normalized regression like lasso would be effective.
*	**Random Forest** – Again, with the sparsity associated with the data, I thought that this would be a good fit. 

## Model performance
The Random Forest model far outperformed the other approaches on the test and validation sets. 
*	**Lasso Regression** : MAE = 11.22
*	**Random Forest**: MAE = 18.86
*	**Support Vector Regressor**: MAE = 19.67

## Productionization 
In this step, I built a flask API endpoint that was hosted on a local webserver by following along with the TDS tutorial in the reference section above. The API endpoint takes in a request with a list of values from a job listing and returns an estimated salary. 

## Code and Resources 
**Python Version:** 3.7  
**Packages:** pandas, numpy, sklearn, matplotlib, seaborn, selenium, flask, json, pickle  
**For Web Framework Requirements:**  ```pip install -r requirements.txt```  
**Scraper Github:** https://github.com/arapfaik/scraping-glassdoor-selenium  
**Scraper Article:** https://towardsdatascience.com/selenium-tutorial-scraping-glassdoor-com-in-10-minutes-3d0915c6d905  
**Flask Productionization:** https://towardsdatascience.com/productionize-a-machine-learning-model-with-flask-and-heroku-8201260503d2
