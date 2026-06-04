📊 NeoBank Customer Churn: Data Analytics Case Study

🔗 **[View the Interactive Tableau Dashboard Here](https://public.tableau.com/app/profile/alex.stephen5722/viz/NeoBank_Customer_Churn_Analysis/Dashboard1?publish=yes)**

📋 1. Executive Summary
NeoBank is facing a critical retention crisis, losing 20.37% of its overall customer base. However, a deeper multi-dimensional analysis reveals that this churn is not random. The bank is systematically bleeding its highest-value clients: older, wealthier customers based in Germany who hold multiple financial products. By isolating geographic anomalies, analyzing product-holding correlations, and mapping customer demographics, this project identifies critical service gaps and provides automated data-driven recommendations to stabilize the bank's core revenue segments.

🔍 2. Deep-Dive Findings & SQL Evidence
📍 Finding A: The Multi-Product Paradox
The Insight: While standard banking strategies dictate that cross-selling more products increases customer "stickiness," NeoBank experiences the exact opposite. Customers with 3 products exhibit an 82.71% churn rate, while 100% of customers with 4 products leave the bank.

Python
# SQL Query: Customer counts and churn percentages by product brackets
query = """
SELECT NumOfProducts, COUNT(*) AS total_customers, SUM(Exited) AS churned_customers,
       ROUND(AVG(Exited) * 100, 2) AS churn_rate_percent
FROM 'Churn_Modelling.csv'
GROUP BY NumOfProducts ORDER BY NumOfProducts;
"""
| Number of Products | Total Customers | Churned Customers | Churn Rate (%) |
| :--- | :---: | :---: | :---: |
| 1 Product | 5,084 | 1,409 | 27.71% |
| **2 Products (Sweet Spot)** | **4,590** | **348** | **7.58%** |
| 3 Products | 266 | 220 | 82.71% |
| **4 Products (Critical Danger)** | **60** | **60** | **100.00%** |

📍 Finding B: High-Value Capital Bleeding (Germany)
The Insight: The geographic churn is concentrated heavily in Germany. While Germany matches Spain in total customer volume, German clients maintain nearly double the average capital balance ($119,730.12) of the other regions, and leave at exactly double the rate (32.44%).

Python
# SQL Query: Geographic breakdown of market value and loss
query = """
SELECT Geography, COUNT(*) AS total_customers, SUM(Exited) AS churned_customers,
       ROUND(AVG(Exited) * 100, 2) AS churn_rate_percent, ROUND(AVG(Balance), 2) AS avg_country_balance
FROM 'Churn_Modelling.csv'
GROUP BY Geography ORDER BY churn_rate_percent DESC;
"""
| Country | Market Volume | Churned Accounts | Churn Rate (%) | Avg Account Balance |
| :--- | :---: | :---: | :---: | :---: |
| **Germany** | **2,509** | **814** | **32.44%** | **$119,730.12** |
| Spain | 2,477 | 413 | 16.67% | $61,818.15 |
| France | 5,014 | 810 | 16.15% | $62,092.64 |

📍 Finding C: The Regional Product Interaction
The Insight: Cross-referencing geography with product counts demonstrates that while the 4-product total failure is a global systemic issue, Germany suffers from a unique onboarding failure. A massive 42.85% of single-product users in Germany churn immediately, compared to just ~22% in France and Spain.

| Country | 1-Product Churn | 2-Product Churn | 3-Product Churn | 4-Product Churn |
| :--- | :---: | :---: | :---: | :---: |
| **Germany** | **42.85%** 🚨 | **12.12%** | **89.58%** | **100.00%** |
| France | 22.43% | 5.70% | 78.85% | 100.00% |
| Spain | 21.87% | 7.35% | 78.79% | 100.00% |

🛠️ 3. Strategic Action Plan (Recommendations)
Based on the intersection of our Tableau dashboards and SQL deep dives, NeoBank management should deploy the following three-pronged intervention strategy:

🛑 Immediate Product Onboarding Freeze: Halt the automated sales pipelines pitching 3rd and 4th financial products to existing users. The 100% churn rate indicates a devastating user experience, hidden fee trigger, or severe backend bug associated with high-tier accounts.

🇩🇪 German Market Intervention Taskforce: Since 42.85% of German clients leave after purchasing their first product, local leadership must audit regional welcome offers, onboarding customer service, and localized competitor interest rates to identify why high-balance European clients reject the bank early on.

🎯 Premium Loyalty Shield for Age Bracket 40-50: The average age of churning clients is 44.8 with high net worth ($91k+ balances). Introduce a targeted "Premium Diamond Tier" retention program offering competitive yield rates on high-balance savings accounts to lock down capital before users exit.
