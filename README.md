# Analysis on Data Science Jobs in Canada with Salary Prediction Flask API Deployed in Herokou

## Project Highlights

* Built salary prediction models for various positions within data analysis area in Canada using LASSO (MAE ~ $16k), Random Forest (MAE ~ $18k) and SVM (MAE ~ $16k)  
* Deployed SVM model as a salary estimator on [this website](https://salary-estimator-shelley.herokuapp.com/) using Flask and Heroku 
* Scraped over 1000 job listings from Glassdoor using python and selenium
####################################################
* Cleaned and Visualized data   
* Optimized Linear, Lasso, and Random Forest Regressors using GridsearchCV to reach the best model. 

## [Web Scraping](https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/glassdoor_scraping_tool.py)

I adjusted the [web scraper](https://github.com/arapfaik/scraping-glassdoor-selenium) using Selenium to scrape the fulltime job postings from glassdoor.ca for 3 target positions -- "data scientist", "data analyst" and "statistician" for a one-month period(2020-05-09 to 2020-06-09) respectively. With each job, I obtained the following: Job title, Salary Estimate, Job Description, Rating, Company Name, Location, Company Headquarters, Company Size, Company Founded Date, Type of Ownership, Industry, Sector, Revenue, Competitors. Because other positions such as data engineer and machine learning engineer also showed up in the search results of the 3 target positions, the following positions appreared in the search results were also chosen to be included in the dataset for analysis and modeling: data engineer and machine learning engineer, research scientist, business intelligent analyst, manager analytics, actuarial analyst.


## [Data Cleaning](https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/data_cleaning.py)

After data scraping, I did some cleaning before building the models for salary prediction as follows:
*  Combined 3 datasets and deleted the duplicate rows
*	 Parsed numeric data out of Salary
*  Removed rows with NAs in Salary and created column Average Salary 
*	 Removed unwanted Rating from Company Name text
*	 Transformed Founded date into Company Age
*  Transformed scraped similar job titles into one simplified job title for all positions
*  Created column Seniority from Job Title
*  Created column Job Description Length from Job Description
*	 Created columns for if different skills were listed in Job Description: Python, RStudio, Excel, AWS, SAS, Pytorch, sql
 

## EDA

After cleaning the scraped data, I did some brief analysis and data visualization. Based on the scraped data from Glassdoor of the one-month period, below are some of the plots. See code [here](https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/EDA.ipynb). 

* Total number of new fulltime job postings for statistician, data analyst and data scientist were 1, 16, 12 respectively per day on average  

![alt text](https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/wordcloud_img/%23job_postings_by_date.png "job_postings_by_date")

* Most jobs were unspecified regarding seniority. For jobs sepecified with seniority, senior positions were demanded around 10 times more than junior and intern positions combined

![alt text](https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/wordcloud_img/%23jobs_by_seniority.png "jobs_by_seniority")

* Boxplot of salary distribution for data analyst of seniority made most sense since there was enough data for this position. As we can see for data analyst, only approx 1k rise on median salary from intern to junior, but more than 10k from junior to senior.

![alt text](https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/wordcloud_img/boxplot_for_salary_by_job_title(color%3Dseniority).png "boxplot_for_salary_by_job_title")

* Graph below shows the percentage of different skills were list in the job descriptions for each position. 
  * Excel was the most desirable for actuarial analyst and data analyst with Excel was listed in more than 70% job discriptions for both positions. Excel was the least desirable for machine learning engineer (less than 30%) 
  * Python was the most desirable for data engineer (approx. 85%) followed by data scientist and machine learning engineer (approx. 80%)
  * R/R studio was desired for data analyst, data engineer and data scientist (all under 10%)
  * SAS was the most desirable for statistician (almost 80%) then actuarial analyst (less than 60%)
  * SQL was the most desirable for data engineer (approx. 85%) and then data analyst and business intelligent analyst (approx. 65%) 

![alt text](https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/wordcloud_img/tools_mentioned_in_job_discription(%25).png "tools_mentioned_in_job_discription(%)")

* Top10 company that released most jobs with their company ratings

<p align="center">
<img width="450" height="550" src="https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/wordcloud_img/top10_company_released_most_jobs_with_ratings.png">
</p>

* Salary distribution and total number of jobs by sector and by job title

<p align="center">
<img width="450" height="550" src="https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/wordcloud_img/salary_and_%23jobs_by_sector_by_job_title.png">
</p>

* Salary distribution and total number of jobs by location and by job title

<p align="center">
<img width="450" height="550" src="https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/wordcloud_img/salary%20distribution%20and%20count%20of%20jobs%20by%20location%20by%20title.png">
</p>

* Wordcloud

<img align="left" width="250" height="450" src="https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/wordcloud_img/wordcloud_DataAnalyst.png">
<img align="right" width="250" height="450" src="https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/wordcloud_img/wordcloud_DataScientist.png">
<p align="center">
  <img width="250" height="450" src="https://github.com/ensembles4612/analysis_and_modeling_on_data_science_jobs_Canada/blob/master/wordcloud_img/wordcloud_Statistician.png">
</p>

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
