# Customer-Churn-Prediction

## Business Understanding

Customer churn occurs when customers switch their service providers. For organizations like telecom companies, the churn rate is an important metric because retaining existing customers is less costly than acquiring new ones. Predicting potential churners helps companies prepare in advance with preventive measures such as giving discount. Concentrating on specific customers allows for targeted retention strategies, optimizing the budget for maximum profitability. The goal of this project is to reduce customer churn rates.

Several reasons could cause customers to churn, such as new competitors, poor customer service, network service problems or high cost. We could come up with countermeasures to address each problem. However, in this project we choose the solutions that could be deployed quickly as required in real business scenario, specifically **offering promotions and discounts**.

## Data Preparation
The data preparation can be divided into three parts: Data Cleaning and Changing Data Type.
### a. Data Cleaning:

 - Drop Null Values: Drop rows which has empty Total Charges
 - Drop Unecessary column: Drop Customer ID

### b. Changing Data Type:
 - Adjust numerical value data to type "float64" and categorical data to type "object"
 
## Data Understanding and Exploration

The dataset contains 7,043 entries and 21 distinct feature variables. The main features cover categories including demographics, billing, online services and streaming. The target variable is whether a customer will churn or not.


### K Means

We used K means as an unsupervised learning technique to segment customers and understand our existing customer base.
Below chart shows the center of each cluster, and we also extracted features that are most relevant to the analysis. From the result we could see that:

1. Internet Service is a big part among all telecom service, as we could clearly distinguish the group without internet service with others. This group is also charged the least, and accounts for around 22% of the total population.
2. The first cluster pays the highest monthly charge of all groups, and they also stays with the service provider with the longest time. This group should create the highest value to the company.


|  clusters  | InternetService_No |   tenure   | MonthlyCharge |  Count   |
|------------|--------------------|------------|---------------| -------- |
|     0      |          0         |  54.758588 |   91.517438   |  2096    |
|     1      |          1         |  30.667763 |   21.076283   |  1520    |
|     2      |          0         |  16.431287 |   74.287135   |  2736    |
|     3      |          0         |  31.830882 |   41.992500   |   680    |


### PCA Analysis

Another exploration with data is to utilize Principle Component Analysis and find out the features made up of each component.
Each component explained certain percentage of data variance.

![download](https://github.com/Katherineweiting/Customer-Churn-Prediction/assets/58812052/4afe18fa-d695-4db2-bf3b-b189e7a91262)

1. First Component: Below shows the 10 largest features made up of the first component. It is obvious that internet service is an important variable.

| Feature                               | Value      |
|---------------------------------------|------------|
| InternetService_No                    | 0.290471   |
| OnlineSecurity_No internet service    | 0.290471   |
| OnlineBackup_No internet service      | 0.290471   |
| DeviceProtection_No internet service  | 0.290471   |
| TechSupport_No internet service       | 0.290471   |
| StreamingTV_No internet service       | 0.290471   |
| StreamingMovies_No internet service   | 0.290471   |
| PaperlessBilling_No                   | 0.123803   |
| PaymentMethod_Mailed check            | 0.116226   |
| MultipleLines_No                      | 0.115541   |

2. Second Component: Below shows the 10 largest features made up of the second component. Tenure, Total charges, and Contract features are all related to customer loyalty.

| Feature               | Value      |
|-----------------------|------------|
| tenure                | 0.332016   |
| TotalCharges          | 0.320498   |
| Contract_Two year     | 0.239735   |
| Partner_Yes           | 0.215425   |
| DeviceProtection_Yes  | 0.205186   |
| TechSupport_Yes       | 0.188910   |
| StreamingMovies_Yes   | 0.178788   |
| StreamingTV_Yes       | 0.178376   |
| OnlineBackup_Yes      | 0.174636   |
| OnlineSecurity_Yes    | 0.165281   |


 
## Modeling

The core task of the analysis is to predict whether a customer will chrun (switch their service providers) given the observed characteristics. Since the prediction is binomial, we will conduct data mining for classification prediction. In this analysis, we will be using different supervised learning models including logistic regression, SVM, and Random Forest. We also apply grid search with cross-validation to find the best hyperparameters and make use of all data points to lower model performance bias. The data set is split into 80% training data and 20% testing data. The performance metrics we chose are OOS Accuracy.

The result showed that the Logistic Regression model has the best OOS performance with an Accuracy of 81.2%, therefore, we will apply the Logistic Regression model for our prediction.

### Random Forest
- Best Parameters: {'max_depth': None, 'n_estimators': 150}
- Test Accuracy: 79.32%

### SVM
- Best Parameters: {'C': 1, 'gamma': 'scale'}
- Test Accuracy: 80.75%

### Logistic Regression
- Best Parameters: {'C': 10, 'solver': 'lbfgs'}
- Test Accuracy: 81.2%
 
## Implementation
We first run the logistic regression as a baseline model and then build the LSTM model to see how well the model is performing. The implementation includes two parts: hyperparameter tuning and up-sampling:  
 - Hyperparameters Tuning: One of the challenges we faced was model overfitting. We handled this issue by reducing the embedding dimension from 400 to 100, adding L2 regularization, and reducing epochs as early stops. After tuning, the validation loss curve performs a better-decreasing trend.
 - Up-sampling: Another challenge that we encountered during modeling was handling the proportion of positive sentiment. As we see from the visualization, over 80% of the reviews recommended the items, and the imbalanced distribution impacted the model's ability to learn from diverse data. To address the issue, we tackled it by increasing the sample proportion of unrecommend reviews. After up-sampling, the percentage of recommended and unrecommend reviews is 54.4% to 45.6%, and the testing accuracy for binary classification also increased from 87% to 92%.
 
## Results and Evaluation
This analysis is a classification problem, so we use OOS Accuracy to evaluate the result. The baseline model (logistic regression) accuracy for binary classification and multi-class classification is 87% and 55.43% respectively. With the implementation of the LSTM model in both cases, the results turned out to be better than the baseline model. Please see the plot below:
### a. LSTM for Binary Prediction
 - Test Loss: 0.163
 - Test Accuracy: 93%

![Picture1](https://github.com/Katherineweiting/E-Commerce-Review-Sentiment-Analysis/assets/58812052/719fe51a-7c32-437e-8795-a9b9f23b2e8b)


### b. LSTM for Multi-Class Prediction
 - Test Loss: 0.860
 - Test Accuracy: 61.7%

![Picture2](https://github.com/Katherineweiting/E-Commerce-Review-Sentiment-Analysis/assets/58812052/89caee5d-9c09-4185-b53c-f7d956f2129a)


It is clear that the LSTM model more accurately identifies whether customers recommend the item compared to classifying its rating. We noticed that the customer reviews often contain a blend of positive and negative aspects. For example, in a 2-star review, a customer mentioned, "it's soft and fits okay, but it has zero support or shape." We believe that such mixed reviews pose a challenge for the model to classify ratings.
In addition, from the visualization of the Recommendation Rate by Age Group for Department Name, we could see that the “trend” category has the lowest rating among age groups 40 to 49 and 50 to 59. To deep dive into this issue, we use word cloud to identify the most frequently used words in low-rating customer reviews on "trend” items in that group. This approach helps the business get insights from customer feedback and make improvements accordingly.

<img src="https://github.com/Katherineweiting/E-Commerce-Review-Sentiment-Analysis/assets/58812052/7ce05746-1ef9-4b8c-9f1d-af77b03c703b" width=50% height=50%>


“Look”, “pattern”, “fabric”, “picture”, “small”, and “waist” are the words that appear more frequently than others, indicating that those customers who give lower ratings have concerns regarding size, material quality, and correspondence with product images. We suggest that the business could focus on those areas to enhance customer ratings.
 
## Deployment
With data mining techniques such as the LSTM model, businesses can gain insights from customers by identifying the sentiment of their feedback. Word clouds also allow companies to delve deeper into specific categories such as age groups and departments.
From the positive reviews, a word cloud reveals key terms like “Color”, “Look”, “Fit”, “Dress”, “Perfect”, and “Soft”, suggesting that customers particularly value the design and sizing of our clothing, especially our dresses.

<img src="https://github.com/Katherineweiting/E-Commerce-Review-Sentiment-Analysis/assets/58812052/eb4a7208-ccae-4b84-997b-adf910791a58" width=50% height=50%>

Conversely, the word cloud for negative reviews highlights words like “Fabric”, “Material”, “Ordered”, “Retailer”, and “Dress”. This indicates a need for improvement in product quality and after-sales service. The frequent mention of “Dress” in negative reviews is likely due to higher sales volumes in this category, leading to a proportional increase in negative feedback.

<img src="https://github.com/Katherineweiting/E-Commerce-Review-Sentiment-Analysis/assets/58812052/b7ba6bd9-8309-457f-ae4a-ab5fc3b7dc88" width=50% height=50%>

However, our current model has limitations, as some reviews contain both positive and negative comments (e.g., “I like the size, but the material is disappointing”). This can lead to misleading results in our word cloud. To address this, we propose that the e-commerce platform should refine its review system to include more specific feedback options. For instance, at the beginning of the review section, a question such as “Why did you not like our product?” could be accompanied by choices like “Quality”, “Size”, “Design”, etc. This would facilitate more accurate analysis and aid in the company’s ongoing development and improvement.

## References
1.	Winter, Dayna. [What Is Ecommerce? A Comprehensive Guide (2024)](https://www.shopify.com/blog/what-is-ecommerce#1)
2. 	Kaggle.com [Women’s E-Commerce Clothing Reviews](https://www.kaggle.com/datasets/nicapotato/womens-ecommerce-clothing-reviews)
