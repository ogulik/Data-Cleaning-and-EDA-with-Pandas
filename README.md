# Data Cleaning and EDA with Pandas

**Data Source:** [github.com/allanvc/onlineretail](https://github.com/allanvc/onlineretail)

**Data Description:** ~542k transactional records of a UK-based online gift store

## The Goal of the Project
- Dirty data in → Clean data out
- EDA with a set business objective

#

## Data Cleaning
![Data In](https://raw.githubusercontent.com/ogulik/Data-Cleaning-and-EDA-with-Pandas/refs/heads/main/images/dirty_data.png)  --->  ![Data Out](https://raw.githubusercontent.com/ogulik/Data-Cleaning-and-EDA-with-Pandas/refs/heads/main/images/clean_data.png)

✅ Taken into account the common retail data processing practice to mark returned products by creating a separate transaction record with a negative quantity. Therefore, simply deleting rows with values below zero will leave cancelled transactions in place, incorrectly impacting the analytics outcome.

Handled by separating affected products into a different dataframe:

> `subset_1 = df[df.Quantity <0][['StockCode', 'UnitPrice', 'CustomerID']]`
>
> `neg_tuples = [tuple(x) for x in subset_1.to_numpy()]`
> 
> `subset_2 = df[df.Quantity > 0][['StockCode', 'UnitPrice', 'CustomerID']]`
> 
> `pos_tuples = [tuple(x) for x in subset_2.to_numpy()]`
> 
> `matched = set(neg_tuples) & set(pos_tuples)`
> 
> `df_matched = df[pd.Series(list(zip(df.StockCode, df.UnitPrice, df.CustomerID)), index=df.index).isin(matched)]`

then looping over it to find matching pairs:

![Python loop](https://raw.githubusercontent.com/ogulik/Data-Cleaning-and-EDA-with-Pandas/refs/heads/main/images/python_loop.png)

Follow my process on [this Jupyter notebook](https://github.com/ogulik/Data-Cleaning-and-EDA-with-Pandas/blob/main/Online-Retail-Cleaning.ipynb)

#

## Exploratory Data Analysis
**Set Goal for Analysis:**  Identify the most popular products and time intervals, using past transactions data, to launch a cost-efficient marketing campaign.

**Key Objectives**:
1. Find out the most popular products to target;
2. Identify the peak shopping hours to display ads at the appropriate time;
3. Reveal the week days that have most transactions on.

**Key Findings:**

**1) Top Products by Frequency, Quantity and Revenue (combined):**

![Top Products](https://raw.githubusercontent.com/ogulik/Data-Cleaning-and-EDA-with-Pandas/refs/heads/main/images/result_products.png)

**2) Peak Shopping Hours:**

![Best Time](https://raw.githubusercontent.com/ogulik/Data-Cleaning-and-EDA-with-Pandas/refs/heads/main/images/result_time.png)

**3) Best Day of the Week:**

![Best Day](https://raw.githubusercontent.com/ogulik/Data-Cleaning-and-EDA-with-Pandas/refs/heads/main/images/result_day.png)

⚠️ No transactions on Saturdays. The reason - unknown. Require more information from business owner.

#

### EDA Conclusion 

![Conclusion](https://raw.githubusercontent.com/ogulik/Data-Cleaning-and-EDA-with-Pandas/refs/heads/main/images/eda_conclusion.png)

Follow my process on [this Jupyter notebook](https://github.com/ogulik/Data-Cleaning-and-EDA-with-Pandas/blob/main/Online-Retail-EDA.ipynb)
