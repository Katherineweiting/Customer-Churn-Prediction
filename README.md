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

The result showed that the Logistic Regression model has the best OOS performance%, therefore, we will apply the Logistic Regression model for our prediction.

![download](https://github.com/Katherineweiting/Customer-Churn-Prediction/assets/58812052/58e7cdb9-8f2d-4a5c-b3bf-5dc4fcb1cedc)

The accuracies for the models are as follows 
- Logistic Regression 80.52%
- Random Forest Classifier 79.52%
- Support Vector Machine 79.73%

We retrained the re-trained the Random Forest model based on the whole training dataset and got the ROC curve as below:

![download (1)](https://github.com/Katherineweiting/Customer-Churn-Prediction/assets/58812052/56e021f4-2b49-428a-8c78-a427cdad5ebc)

 
## Results and Deployment
Since the optimal goal of a business is to increase its profits, we need to maximize the expected profit formula

![Picture1](https://github.com/Katherineweiting/Customer-Churn-Prediction/assets/58812052/cae9b694-2c64-4616-a5d7-d64d057682e0)


Our Logistic Regression model can be used to predict the probability of customers churning, and the company can rank its customers with a the probability of churning.
We then maximize the profit by multiplying the values in the cost-benefit matrix and confusion matrix. Since there is an asymmetry between the benefit of correct predictions and the cost of incorrect predictions, we accounted for this through the weights in the cost-benefit matrix. We assume that on average, if company gave discounts to customer it could make $0.1 if he does not churn, but it lose $0.3 if they churn.

<img src= "https://github.com/Katherineweiting/Customer-Churn-Prediction/assets/58812052/fc83c54b-9651-4850-90ae-ea090bbe9e77" width="50%" height="50%" />

Based on the ranking function, each customer receives a ranking score ordered in descending order. We then find the individual customer(s) above a certain percentile point(threshold). All customers above that percentile point will receive a discount. We picked the threshold that gives us the maximum score, which means that the threshold will have a good level of profits but not at the cost of lower accuracy, which could reduce the campaign's success. Based on our test data, we found that sending coupons to the top 61% of customers will lead to a maximized score, which is the maximized profits ($250), while ensuring a good level of accuracy.

![download](https://github.com/Katherineweiting/Customer-Churn-Prediction/assets/58812052/fa94994e-10c7-41cd-8ba8-c760be483404)


## References
1.	 Duke University, The Fuqua School of Business - Data Science for Business Class Materials
2. 	Kaggle.com [Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn/data)
