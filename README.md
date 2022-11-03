## Customer Segmentation using RFM (Recency,Frequency,Monetary) Analysis with Python

#### In this chapter, we will work how to segment using RFM Analysis over customer data of MIGROS Market. This means that over time, we can understand which customer is truly royal and which customer is a promising customer.

### What is RFM?
#### RFM (Recency,Frequency,Monetary) analysis is a behavior-based approach by segmenting customers. It groups customers according to their previous purchases. How recently, how often, and how much a customer bought. RFM divides customers into various groups for better service. It helps managers identify potential customers to do more profitable business. For example:

•	Ability to optimize every touchpoint in customers' moments to generate more revenue and active users,

•	Which customer is really our customer,

•	How to understand our promising customers,

•	When a campaign is going to be organized, we have the data to be used in this campaign without any noise,

•	Determining the buying and selling cycle,

we can say that we aim to reach a certain goal at points like above.

It also helps managers run an effective promotional campaign for personalized service.

### What is RFM Steps?
#### RFM Steps of RFM(Recency, Frequency, Monetary):
¬	Calculate the Recency, Frequency, Monetary values for each customer.

¬	Add segment bin values to RFM table using quartile.

¬	Sort the customer RFM score in ascending order.

   ¬	For Recency:
   Who have purchased recently? Number of days since last purchase (least recency)
   Calculate the number of days between present date and date of last purchase each customer.

   ¬	For Frequency:
   Who has purchased frequently? It means the total number of purchases. (high frequency)
   Calculate the number of orders for each customer.

   ¬	For Monetary:
   Who have high purchase amount? It means the total money customer spent (high monetary value)
   Calculate sum of purchase price for each customer.

##### 1. Calculate the Recency, Frequency, Monetary values for each customer.
<img width="241" alt="Ekran Resmi 2022-11-03 21 58 08" src="https://user-images.githubusercontent.com/91700155/199810652-0ac4cc34-ae33-4831-a5d2-4207a84c6235.png">

##### 2. Add segment bin values to RFM table using quartile.
<img width="422" alt="Ekran Resmi 2022-11-03 21 58 16" src="https://user-images.githubusercontent.com/91700155/199810706-ac461907-ff31-4ce5-a988-05f465872b4b.png">

##### 3. Sort the customer RFM score in ascending order.
<img width="486" alt="Ekran Resmi 2022-11-03 21 58 26" src="https://user-images.githubusercontent.com/91700155/199810790-8392d648-2461-498f-a4e2-a5969d8998f4.png">

<p> 

### Identify Potential Customer Segments using RFM in Python

#### Data Definition
Introduction of tables and columns in the database
Tables:
The table that we use is Migros Marketing Customer Data taken from Migros

Columns:
Customer No: Unique custom value assigned to each customer
Age: Age information of customers
Gender: Gender information of customers
Day of Purchase: Day-indexed purchase information
Product Category: Product category
Indexed Turnover: Customer indexed turnover
Indexed Transaction: Transaction indexed to the customer
Price: Product prices
InvoiceNo: Helps you to count the number of time transaction performed(frequency)
InvoiceDate: Help you calculate recency of purchase
Quantity: Purchased in each transaction
UnitPrice of each unit purchased by the customer: Will help you to calculate the total purchased amount

### Importing Required Library

```console
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

### Loading Dataset
```console
df = pd.read_excel('RFM-Migros.xlsx')
df.set_index('Customer No')
df['Day of Purchase'] = pd.to_datetime(df['Day of Purchase'])
```
<img width="191" alt="Ekran Resmi 2022-11-03 22 03 20" src="https://user-images.githubusercontent.com/91700155/199811581-7791b0c2-79c1-4974-886a-e98aad6c15fc.png">

### Filter required Columns
```console
df = pd.read_excel('RFM-Migros.xlsx')
df.set_index('Customer No')
df['Day of Purchase'] = pd.to_datetime(df['Day of Purchase'])
```
#### 🔸 InvoiceDate
👉🏽 Help you calculate recency of purchase
🔸 InvoiceNo
👉🏽 Helps you to count the number of time transaction performed(frequency)
🔸 Quantity
👉🏽 Purchased in each transaction
🔸 UnitPrice of each unit purchased by the customer
👉🏽 Will help you to calculate the total purchased amount

```console
df_total = df.groupby('Product Category').sum()[['Price']]
df_total
```
<img width="163" alt="Ekran Resmi 2022-11-03 22 06 48" src="https://user-images.githubusercontent.com/91700155/199812149-da84ed32-33a2-45fa-925b-af0adb9ec305.png">

```console
df['Day of Purchase'] = pd.to_datetime(df['Day of Purchase'])
```

```console
import datetime
present = datetime.datetime(2022, 5, 25)
present
```

### RFM Analysis
🔸 For Recency, Calculate the number of days between present date and date of last purchase each customer.
🔸 For Frequency, Calculate the number of orders for each customer.
🔸 For Monetary, Calculate sum of purchase price for each customer.

```console
rfm= df.groupby(df.index).agg({'Day of Purchase': lambda date: (present - date.max()).days,
'InvoiceNo': lambda num: len(num),
'Price': lambda price: price.sum()})
```
```console
rfm.columns = ['recency','frequency','monetary']
```
```console
rfm.head(5)
```
<img width="241" alt="Ekran Resmi 2022-11-03 21 58 08" src="https://user-images.githubusercontent.com/91700155/199813883-27215903-dbb7-47c3-9521-dcd6d0185cb8.png">


```console
rfm['r_quartile'] = pd.qcut(rfm['recency'], 4, ['1','2','3','4'])
rfm['f_quartile'] = pd.qcut(rfm['frequency'], 4, ['4','3','2','1'])
rfm['m_quartile'] = pd.qcut(rfm['monetary'], 4, ['4','3','2','1'])
```
```console
rfm.head(5)
```
<img width="422" alt="Ekran Resmi 2022-11-03 21 58 16" src="https://user-images.githubusercontent.com/91700155/199814095-69f3a1d1-fe72-45ef-99cc-55b9f37439f9.png">

```console
rfm['RFM_Score'] = rfm.r_quartile.astype(str)+ rfm.f_quartile.astype(str) + rfm.m_quartile.astype(str)
rfm.head(5)
```
<img width="486" alt="Ekran Resmi 2022-11-03 21 58 26" src="https://user-images.githubusercontent.com/91700155/199814192-04ed6ef5-a268-4c4e-b209-08fad54619b8.png">

### RFM Result Interpretation
##### Filter out Top/Best customers
```console
rfm.sort_values('RFM_Score', ascending=False).head(15)
```
<img width="494" alt="Ekran Resmi 2022-11-03 21 58 38" src="https://user-images.githubusercontent.com/91700155/199814259-b04e6406-1366-4894-8bf7-005e6b1c6ac4.png">

#### 📋 When a campaign is organized, we can say that the RFM Score can reveal the type of customer on which the campaign is most effective, the customer satisfaction accordingly, and the promising customer portfolio that may occur in the future.

### Conclusion
In this analysis we do behavior-based approach grouping customers into segments. We did groups the customers on the basis of their previous purchase transactions. 
This analysis filters customers into various groups for the purpose of better service. It helps managers to identify potential customers to do more profitable business.  
Recency (R): Who have purchased recently? Number of days since last purchase (least recency)
Frequency (F): Who has purchased frequently? It means the total number of purchases. (high frequency)
Monetary Value(M): Who have high purchase amount? It means the total money customer spent (high monetary value)
Also the details:
-How recently, how often, and how much did a customer buy product?
-A top-spending customer segment and if they only bought the products once, or how recently? 
-Do they often purchase our product? 
As find answers to this questions with also we seen helps managers to run an effective promotional campaign for personalized service.
In this project, we covered a lot of details about Customer Segmentation. We have experienced what the customer segmentation is, need of customer segmentation, types of segmentation, RFM analysis doing implementation of RFM. 
Also, we covered some basic concepts of such as handling duplicates, group by, based on sample quantiles.


<p></br>
   <p>


Thanks for your time :)

