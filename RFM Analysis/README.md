
# RFM Analysis

The initial step for any successful marketing strategy is customer segmentation. This ensures that the right product or solution is delivered to the correct customer at the appropriate time and through the proper channel. One popular data-driven technique for customer segmentation is RFM analysis, which stands for recency, frequency, and monetary value. This project aims to analyze sales data of a US-based company. The primary objective is to create customer segments to identify the most valuable customers and personalize marketing campaigns, thereby improving unit economics and increasing revenue and profits.


## 🛠 Tech Stack

- Python

- Pandas

- Datetime

- Jupyter Notebook


## Data Source

[US Regional Sales Data](https://data.world/dataman-udit/us-regional-sales-data)


## Data Processing

**Importing the necessary libraries**

```bash
  import pandas as pd
  from datetime import datetime
```
**Reading the data**

```bash
  df = pd.read_csv("sales_data.csv")
```

![Initial Dataframe](https://github.com/MandarSawant18/Python_Projects/blob/main/RFM%20Analysis/Screenshots/Initial%20Dataframe.png?raw=true)


**Checking the dataset and changing datatypes of _‘OrderDate’_ and _‘CustomerID’_ to datetime & string respectively**

```bash
  df["OrderDate"] = pd.to_datetime(df["OrderDate"])

  df["_CustomerID"] = df["_CustomerID"].apply(str)
```

**Calculating _‘Revenue’_ using the _‘Discount’_, _‘Unit Price’_ and _‘Order Quantity’_ columns**

```bash
  df['Revenue'] = ((1-df["Discount Applied"])*df['Unit Price']) * df['Order Quantity']
```

**Creating a separate data frame called _sales_comp_ which will only have columns to be used for analysis like _‘OrderNumber’_ , _‘CustomerID’_, _‘OrderDate’_, _‘Revenue’_**

```bash
  columns = ["OrderNumber","OrderDate","_CustomerID","Revenue"]
  sales_comp = df[columns]
```

**Aggregating the data on _‘CustomerID’_ level and calculating the recency, frequency and monetary values and storing it in a separate data frame called _rfm_dataset_**

```bash
  rfm_dataset = sales_comp.groupby('_CustomerID').agg({
            "OrderDate": lambda v: (today_date - v.max()).days,
            "OrderNumber": 'count',
            "Revenue": 'sum'})

  rfm_dataset = rfm_dataset.rename(
            columns={
                "OrderDate":'recency',
                "OrderNumber":'frequency',
                "Revenue":'monetary'})
```

**Using qcut method call to normalize the r,f,m values in the range of 1-5 to be used for analysis**

```bash
r = pd.qcut(rfm_dataset['recency'],q=5,labels=range(5,0,-1))
f = pd.qcut(rfm_dataset['frequency'],q=5,labels=range(1,6))
m = pd.qcut(rfm_dataset['monetary'],q=5,labels=range(1,6))


#For r we use reverse list because lower the number of days in recency, better for the business because it shows the customers latest purchase
```

**Creating additional dataframe to be used for analysis**

```bash
 rfm = rfm_dataset.assign(R=r.values,F=f.values,M=m.values)
 rfm['rfm_group'] = rfm[['R','F','M']].apply(lambda v: '-'.join(v.astype(str)),axis=1)
 rfm['rfm_score'] =  rfm[['R','F','M']].sum(axis=1)
```


![](https://github.com/MandarSawant18/Python_Projects/blob/main/RFM%20Analysis/Screenshots/Final%20Dataframe.png?raw=true)


## Results & Interpretations:


- The dataset contains 7991 records of sales spanning over 2.5 years from 2018 to 2020
- The company has served a total of 50 different customers in this time period
- These customers have been further divided into different segments like Core, Loyal, Risky, Lost customers etc.

**Core Customers**
```bash
  rfm[rfm['rfm_score'] == 15]
```
These are customers who are very involved, have made the most recent purchases, have made frequent purchases, and have generated the highest amount of revenue.

 It would be advisable to prioritize loyalty programs and new product introductions as these customers have shown a greater inclination towards paying full price. It would be best to avoid using discount pricing to increase sales. Instead, offering value-added deals through personalized product recommendations based on their past purchases should be considered.

**Loyal Customers**
```bash
  rfm[rfm['F'] >= 3]
```
Customers who make the most frequent purchases.

Loyalty programs have proven to be quite effective in retaining repeat customers. Offering incentives such as free shipping or suggesting related products can be a way to express gratitude and encourage them to become Core customers.


**Whales**
```bash
  rfm[rfm['M'] >= 3]
```
The top revenue-generating customers.

These customers have shown they are willing to pay more, so the focus should be on offering premium products, or value-added options instead of giving discounts. This can increase the average order value.

**Risky Customers**
```bash
  rfm[(rfm['R']==2) & (rfm['F'] >= 3) & (rfm['M'] >= 3)]
```
They frequently made expensive purchases, but it has been a while since their last one.

Create customized reactivation campaigns to reconnect with customers, providing renewals and useful products to motivate them to make another purchase.

**Lapsed Customers**
```bash
  rfm[(rfm['R']>2)]
```
Customers who haven’t made any purchase since a very long time.

To prevent losing customers to competitors, use appropriate promotions to bring them back and conduct surveys to identify the reasons for their departure.



## Methods Used
Links to documentation of all the method calls/functions used

[pd.to_datetime](https://pandas.pydata.org/docs/reference/api/pandas.to_datetime.html)

[apply](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.apply.html)

[lambda](https://www.w3schools.com/python/python_lambda.asp)

[qcut](https://pandas.pydata.org/docs/reference/api/pandas.qcut.html)

## Related

Here are some related projects

[Python projects](https://github.com/MandarSawant18/Python_Projects)

[Power BI projects](https://github.com/MandarSawant18/Power_BI_projects)

[SQL Projects](https://github.com/MandarSawant18/SQL_Projects)
## 🔗 Links
[![portfolio](https://img.shields.io/badge/my_portfolio-000?style=for-the-badge&logo=ko-fi&logoColor=white)](https://www.datascienceportfol.io/mandarsawant)
[![github](https://img.shields.io/badge/GitHub-181717.svg?style=for-the-badge&logo=GitHub&logoColor=white)](https://github.com/MandarSawant18)
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/mandarsawant92/)

## Feedback

For any feedback or support, feel free to reach my Linkedin handle or write an email to smandar.work@gmail.com