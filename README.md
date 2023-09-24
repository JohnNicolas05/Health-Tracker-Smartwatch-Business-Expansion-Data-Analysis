Project Date: August 29, 2023
# Health Tracker Smartwatch Business Expansion Data Analysis

## Objective

The purpose of this project is to demonstrate proficiency in data analysis using Excel, SQL, Power BI and Python.  The objective is to identify the state with the best potential for business expansion. These are the analytical tasks for each tool:

**SQL:**
1.	Calculate the total profit, research & development expenditure, administration expenditure, and marketing expenditure for each state.
2.	Rank the states based on their total profit and identify the top 5 states.
3.	Calculate the average health spending, property prices, state income, and population for the top 5 states and compare them.

**Excel:**
1.	Create a pivot table to summarize the total profit, research & development expenditure, administration expenditure, and marketing expenditure for each state.
2.	Create a bar chart to compare the total profit for the top 5 states.
3.	Create a scatter plot to analyze the correlation between the state income and property prices for the top 5 states.

**Power BI:**
1.	Create a visual to show the total profit for each state and add a filter to show the top 5 states.
2.	Create a map to show the average health spending, property prices, state income, and population for the top 5 states.
3.	Create a dashboard to compare the health spending, property prices, state income, and population for the top 5 states.

**Python:**
1.	Calculate the correlation between the total profit and health spending, property prices, state income, and population for each state. Use the df.corr() method for this task.
2.	Create a bar chart using Matplotlib/Seaborn to compare the total profit for the top 5 states.
3.	Create a scatter plot using Matplotlib/Seaborn to analyze the correlation between the state income and property prices for the top 5 states.

**Business Metrics:**
Calculate Financial Viability Metrics from Competitor Companies.

## Overview

A startup, based in Los Angeles, California, is launching a Health Tracker Smartwatch and has gained investors for business expansion. The company's objective is to expand its operations to a new state outside of California. However, this expansion requires careful consideration of various factors such as economic, political, and social conditions of the target state. In order to ensure a successful expansion, the company decided to conduct comprehensive research involving data analysis, with the goal of identifying the most suitable location. The company's objective is to expand and foster the growth of their startup business. They aim to select one or two states within the United States for the expansion of their Health Tracker Smartwatch business.

## Dataset

The dataset came from the training platform and consist of only a .tar file. I extracted and restored the database using SQL.

**Competitor companies’ profit and expenditures** — The data set provided consists of the company's profit data and their spending.

**Corruption conviction per capita** — level of corruption per state.

**Health spending** — average, minimum, and maxim spending for healthcare-related products and services per person.

**Property prices** — average, minimum, and maximum property prices per square meter.

**State income** — average, minimum, and maximum income per person.

**Population** — population of states.

**Competitor companies’ profit and expenditures**

The provided dataset consists of profit and spending data for companies. The column containing the company names has been removed for privacy reasons.

| Column Name                 | Data Type | Description                                  |
|-----------------------------|-----------|----------------------------------------------|
| research_developnment_spent | numeric   | the amount spent on research and development |
| administration              | numeric   | the amount spent on staff                    |
| marketing_spent             | numeric   | the amount spend on of company's product     |
| state_usa                   | string    | where the company is located                 |
| profit                      | numeric   | the profit of the company                    |

**Corruption Convictions per Capita**

Corruption convictions per capita is a metric that measures the number of corruption convictions in a state divided by the state's population. It is a way to assess the level of corruption in a state by taking into account the population size.

| Column Name            | Data Type | Description                                       |
|------------------------|-----------|---------------------------------------------------|
| state_usa              | string    | the US State                                      |
| convictions_per_capita | numeric   | the number of convictions over state’s population |

**Health spending**

Average, minimum, and maximum spending for healthcare-related products and services per person.

| Column Name  | Data Type | Description                                                     |
|--------------|-----------|-----------------------------------------------------------------|
| state_usa    | string    | the US State                                                    |
| avg_spending | numeric   | the average value describing the healthcare spending per person |
| min_spending | numeric   | the minimum value describing the healthcare spending per person |
| max_spending | numeric   | the maximum value describing the healthcare spending per person |

**Property prices**

Average, minimum, and maximum property prices per square meter.

| Column Name | Data Type | Description                                                       |
|-------------|-----------|-------------------------------------------------------------------|
| state_usa   | string    | the US State                                                      |
| avg_price   | numeric   | the average value describing the property prices per square meter |
| min_price   | numeric   | the minimum value describing the property prices per square meter |
| max_price   | numeric   | the maximum value describing the property prices per square meter |

**State income**

Average, minimum, and maximum income per state.

| Column Name | Data Type | Description                                               |
|-------------|-----------|-----------------------------------------------------------|
| state_usa   | string    | the US State                                              |
| avg_income  | numeric   | the average value describing the average income per state |
| min_income  | numeric   | the minimum value describing the average income per state |
| max_income  | numeric   | the maximum value describing the average income per state |

**Population**

Population of states.

| Column Name | Data Type | Description                    |
|-------------|-----------|--------------------------------|
| state_usa   | string    | the US State                   |
| estimate    | numeric   | the estimated number of people |

## Data Analysis Process

**SQL:**

**1.	Calculate the total profit, research & development expenditure, administration expenditure, and marketing expenditure for each state.**

SQL Syntax:

```sql
SELECT 
	state_usa, 
	ROUND(SUM(profit), 2) AS total_profit, 
	ROUND(SUM(research_development_spent), 2) AS total_research_development_spent, 
	ROUND(SUM(administration), 2) AS total_admin,
	ROUND(SUM(marketing_spent), 2) AS total_marketing_spent
FROM competitors
GROUP BY state_usa;
```

Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/dc9c4259-3ff7-4501-b4d7-03566725b422)

**2.	Rank the states based on their total profit and identify the top 5 states.**

SQL Syntax:

```sql
SELECT 
    state_usa, 
    ROUND(SUM(profit), 2) AS total_profit, 
    RANK() OVER (ORDER BY SUM(profit) DESC) AS "RANK"	
FROM competitors
GROUP BY state_usa
LIMIT 5;
```
Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/af4050df-d1b8-4ee0-8af3-f821b5ffad03)

**3.	Calculate the average health spending, property prices, state income, and population for the top 5 states and compare them.**

SQL Syntax:

```sql
SELECT 
    c.state_usa, 
    ROUND(SUM(c.profit), 2) AS total_profit, 
    hs.avg_spending,
    pp.avg_price,
    si.average_income,
    p.estimate AS population
FROM competitors c
LEFT JOIN health_spending hs ON c.state_usa = hs.state_usa
LEFT JOIN property_prices pp ON c.state_usa = pp.state_usa
LEFT JOIN state_income si ON c.state_usa = si.state_usa
LEFT JOIN population p ON c.state_usa = p.state_usa
GROUP BY c.state_usa, hs.avg_spending, pp.avg_price, si.average_income, population
ORDER BY total_profit DESC
LIMIT 5;
```

Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/1066997b-1616-481b-8a2d-c1ad7e0cfe28)

**Excel:**

**1.	Create a pivot table to summarize the total profit, research & development expenditure, administration expenditure, and marketing expenditure for each state.**

Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/b1544392-aa65-4405-a2f1-0dda5547c089) ![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/fc056ae7-c91c-4535-98f3-c3d50ad48439)

**2.	Create a bar chart to compare the total profit for the top 5 states.**

Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/02249e6a-3172-413e-a98f-0f213d5b3e96)

**3.	Create a scatter plot to analyze the correlation between the state income and property prices for the top 5 states.**

Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/d9efd0e9-01c4-4b82-b7da-73530ea03221)

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/87a9c062-a90c-443e-9ca7-d5fe295a950c)

Analysis:

In correlation between property price and state income, it is proven that when property price increases the state income also increases.

**Power BI:**

**1.	Create a visual to show the total profit for each state and add a filter to show the top 5 states.**

Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/0d0e078e-013b-4da9-b5d9-c2038d22c499) ![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/da7ca30e-7221-445e-b398-de0a7a1c32cb)

**2.	Create a map to show the average health spending, property prices, state income, and population for the top 5 states.**

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/795e12e4-01da-4da4-aa91-9fcaa43f5f81) ![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/87f3c31a-c7a2-49a7-b964-84dcb7daae11)

**3.	Create a dashboard to compare the health spending, property prices, state income, and population for the top 5 states.**

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/3b3151ce-d93a-41c8-92fa-cfd7b15baa2e)

**Python:**

**1.	Calculate the correlation between the total profit and health spending, property prices, state income, and population for each state. Use the df.corr() method for this task.**

Python Syntax:

```python
from google.colab import drive
drive.mount('/content/drive')

import pandas as pd

df = pd.read_csv('/content/drive/MyDrive/Final Project/competitors_stateincome_property_price.csv')

# Rename Columns
df = df.rename(columns={
    'state_usa' : 'state',
    'avg_spending' : 'avg_health_spend',
    'avg_price' : 'avg_property_price',
    'average_income' : 'avg_state_income',
})
correlation_matrix = df[['total_profit', 'avg_health_spend', 'avg_propert_price', 'avg_state_income', 'population']].corr()
print(correlation_matrix)
```

**Output:**

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/b268c4b7-a0a0-4407-a353-8f4d94b19147)

**2.	Create a bar chart using Matplotlib/Seaborn to compare the total profit for the top 5 states.**

Python Syntax:

```python
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline

data = df[['state','total_profit', 'avg_health_spend', 'avg_property_price', 'avg_state_income', 'population']]

top_5_states = data.head(5)

plt. figure(figsize=(12, 6))
ax = sns.barplot(x='state', y='total_profit', data=top_5_states)
plt.xlabel('State')
plt.ylabel('Total Profit')
plt.title('Top 5 States with the Highest Profit')
plt.xticks(rotation=45)
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda x, loc: "{:,}".format(int(x))))
plt.show()
```

Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/d7b65f8c-b4d8-4fb2-a6f7-2560c28ea3a2)

**3.	Create a scatter plot using Matplotlib/Seaborn to analyze the correlation between the state income and property prices for the top 5 states.**

Python Syntax:

```python
plt.figure(figsize=(10, 6))
sns.scatterplot(x='avg_state_income', y='avg_property_price', data=top_5_states)
plt.xlabel('Average State Income')
plt.ylabel('Average Property Price')
plt.title('Correlation between State Income and Property Prices')
plt.show()
```

Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/307b1692-27e5-439b-8b95-87c795b39836)

**Business Metrics:**

**1.	Calculate Financial Viability Metrics from Competitor Companies**

**Getting state net profit and gain/loss:**

Net Profit = Profit – Total Cost

Net Profit Gain/Loss = (Net Profit / Total Profit) * 100

Output:

**Top 10 States with the Highest Profit Gain**

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/d94cd619-1e82-40b2-8271-5e033fde55bb)



**Top 10 States with the Highest Profit Loss**

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/36c4c636-9f05-462e-9035-a2cd87b993da)

**Getting Competitor Market Share:**

Estimated Customers = Profit / Average Health Spending

Market Share = (Estimated Customers / Population) * 100

Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/32b4bc08-48d7-45ca-b11a-52eaf8e53135) ![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/972e5bfa-f783-4b1b-8307-1f35f97134cd)


**Additional Task:**

**1.	Use SQL to calculate the average profit for competitors in each state.**

SQL Syntax:
SELECT state_usa AS state, ROUND(AVG(profit), 2) AS average_profit
FROM competitors
GROUP BY state_usa
ORDER BY average_profit DESC;

Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/fa01a046-2340-469d-ae69-8dbb381a94be)

**2.	Use SQL to calculate the total healthcare spending for each state, and compare the results to the state's population.**

SQL Syntax:

```sql
SELECT hs.state_usa AS state, SUM(hs.avg_spending + hs.min_spending + hs.max_spending) AS total_health_spend , p.estimate AS population
FROM health_spending hs
LEFT JOIN population p
	ON hs.state_usa = p.state_usa
GROUP BY state, population
ORDER BY population DESC;
```
Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/4995656a-9ba5-40ed-b11f-8024c86afeaf)

**Additional Questions:**

**1.	How does the level of state income impact property prices?**

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/a17ef57a-ed18-4c69-bdaf-9f4d7517e5bd)

Analysis:

Using a scatter chart with property price and state income, majority of the values shows that as the value of state income increases the value of property price increases.

**2.	How does the level of healthcare spending vary between states, and which states spend the most/least per person?**

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/c9f2f573-652f-4e7b-b75a-4cc252a30b05)

Analysis:

Using Pandas .corr function It was able to accurately indicate the strength of direction of relationship between pairs of variables. Here is what the matrix tells us:

-	The correlation coefficient between “average_health_spend” and “average_income” is 0.70. This indicates a moderately positive correlation, suggesting that as the health spend increases, the average income tends to increase as well.
-	The correlation coefficient between “average_health_spend” and “avg_property_price” is 0.71. This indicates a moderate positive correlation implying that higher health spend is associated with higher average property prices.

**Getting the state with the most health spending per person.**

SQL Syntax:

```sql
SELECT state_usa AS state, MIN(avg_spending) least_health_spend
FROM health_spending
GROUP BY state
ORDER BY least_health_spend ASC
LIMIT 3;
```

Output:

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/e381681e-4509-440a-8d25-1fee7ffd2370)

***Using SQL query, we were able to get the state with the least health spending, these are; Wyoming, Idaho, and Montana with the health spending value of $125.5.***

## Findings:

-	The top 5 states with the highest profit in descending order are; Wyoming, Oklahoma, Oregon, Montana, and Mississippi.
-	State with the largest population: California with a total of 39,512,223.
-	The highest average health spend per individual is $350 from Massachusetts, New Jersey, Hawaii, and Rhode Island.
-	The lowest average health spend per individual is $125.5 from Wyoming, Idaho, and Montana.
-	The average health spend of all states is $160.
-	As we compared the property price and state income of the 5 leading states, using a scatter chart shows us a clear picture that as the state income increases, the property price also increases.
-	Average health spending correlates with state’s average income and average property price with a correlation coefficient score of 70%.
-	Through conducting financial viability analysis, we found something interesting: Even though Wyoming had the highest profit, it ranked 12th in net profit gain at 32% with a market share of 23.01% On the other hand, Arizona performed exceptionally well, with a 68% net profit gain and a market share of 0.27%.

## Insights

-	Top 5 leading states has the highest total profit but not the highest net profit
-	Health spending, property price, and state income are interconnected because they partly represent the state’s economic status. Take note that supply and demand always affect these values.

## Recommendations

![image](https://github.com/JohnNicolas05/Health-Tracker-Smartwatch-Business-Expansion-Data-Analysis/assets/34438775/e16bb4bd-49d9-42dd-b11c-6385a69473a4)

**Investing with the states with the highest net profit gain:**

Focusing on the top 10 states with the highest net profit gain can be a strategic decision for investing health tracker smartwatch. We will elaborate why these states are worth considering for your product investment:

1.	**High Net Profit Gain:** These states have demonstrated a significant net profit gain, which indicates that businesses and consumers in these states are spending more and generating more profit. This suggests a healthy economic environment where people have disposable income to spend on products like yours.
2.	**Profitability Potential:** States with higher net profit gain typically indicate a strong market demand and economic growth. Investing in states where people are making more money could translate into higher potential sales and revenue for your product.
3.	**Customer Affordability:** States with rising net profits often suggest that consumers have more financial stability. This means they might be more willing to invest in products that enhance their quality of life, such as a health tracker smartwatch.
4.	**Market Trend Identification:** By focusing on these states, you're capitalizing on a market trend where people are willing to spend on products that align with their improved financial situation. This can help you ride the wave of positive consumer sentiment.
5.	**Competitive Advantage:** If your product offers unique features and benefits that cater to the preferences of people in these states, you have a competitive advantage. Consumers with higher net profits might be more willing to invest in premium products.
6.	**Targeted Marketing:** Concentrating your efforts on states with high net profit gain allows you to fine-tune your marketing strategies. You can create tailored campaigns that resonate with the financial aspirations and priorities of the people in these states.
7.	**ROI Potential:** Investing in states where people are already experiencing a boost in net profit could potentially yield a higher return on investment (ROI). The likelihood of your product being well-received and purchased is higher in these regions.
8.	**Market Expansion:** If health tracker smartwatch gains popularity in these states with strong net profit gain, it could serve as a springboard for expanding your market presence to adjacent regions.

**Avoiding states with the highest net profit loss:**
  
Avoiding the states with the highest net profit loss may be the best strategy for investing your product. Here are some the reasons why we need to avoid investing in these states:

1.	**Financial Instability:** States with significant net profit losses indicate economic challenges. Businesses and individuals in these states might be facing financial difficulties, which could lead to reduced spending and lower demand for non-essential products like health tracker smartwatches.
2.	**Reduced Purchasing Power:** People in states with net profit losses might have reduced disposable income. This means they are likely to cut back on discretionary spending, including on products that are not considered immediate necessities.
3.	**Market Uncertainty:** Investing in states with net profit losses could be risky due to the uncertain economic environment. There's a higher likelihood of market volatility and decreased consumer confidence, which could impact the adoption of new products.
4.	**Profitability Challenges:** If the overall business environment is struggling, companies operating in these states might also find it difficult to generate profits. This can affect partnerships, distribution channels, and potential collaborations for your product.
5.	**Resource Allocation:** Investing in markets with net profit losses might require more resources to educate consumers about the value of your product and overcome financial objections.
6.	**Competitive Landscape:** When economic conditions are tough, competition among businesses intensifies as they strive to attract fewer consumer dollars. This might make it challenging to establish a strong foothold in the market.
7.	**Delayed ROI:** Investing in states experiencing net profit losses might lead to a longer period before you see a return on your investment. Consumers may be more hesitant to adopt new products during times of economic uncertainty.

**Investing on the states with net profit gain and least market share:**
  
Focusing on the states with the highest net profit gain and their corresponding market shares can indeed be a strategic approach for investing your product. Here's why these states are appealing for your health tracker smartwatch product:

1.	**High Net Profit Gain:** These states are experiencing substantial net profit gains, indicating a positive economic environment. People in these states have increased disposable income, making them more likely to invest in products that enhance their well-being, like health tracker smartwatches.
2.	**Market Share:** The provided market share percentages suggest that your product already has a notable presence in these states. This means your product is resonating with consumers, and there's potential for further growth and adoption.
3.	**Profitability Potential:** States with high net profit gains often signify strong market demand and economic growth. Investing in states where people are experiencing financial prosperity can lead to increased sales and revenue for your product.
4.	**Consumer Affordability:** Consumers in states with rising net profits are more financially capable of purchasing non-essential items. Your health tracker smartwatch falls into the category of products that people might consider as they have the means to invest in their well-being.
5.	**Proven Success:** The existing market share percentages indicate that your product has already gained traction in these states. This suggests that your product's features and benefits align well with the preferences of consumers in these regions.
6.	**Tailored Marketing:** With a known presence and market share in these states, you can tailor your marketing efforts to specifically target and engage the consumers who are already interested in your product.
7.	**Expansion Potential:** Focusing on states where your product is already gaining popularity can serve as a strong foundation for expanding your market presence to neighboring states or regions.
8.	**Higher Conversion Rates:** Consumers in states with high net profit gains are more likely to convert into customers as they have the financial capacity to make purchases that align with their interests and needs.
