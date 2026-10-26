Customer Shopping Behavior Analysis

Dashboard

Question: do discounts, subscriptions and shipping options make customers spend more, and where does revenue actually come from?

Short answer: no. Average spend per order is about $60 in almost every group. Revenue differences between groups come from how many orders they place, not how much each order is worth.

Dataset

3,900 retail purchases, one row per customer, 18 columns (age, gender, item, category, purchase amount, location, season, review rating, subscription, shipping, discount, previous purchases, payment method, purchase frequency). Source: the public "Customer Shopping Behavior" dataset (Kaggle).

Tools
Stage	Tool
Cleaning, new columns, EDA	Python (pandas, matplotlib, seaborn) in Jupyter
Storage and business questions	MySQL (13 queries, window functions, CTEs)
Dashboard	Power BI
Key findings
Discounts don't raise spend. 43% of orders use a discount, yet discounted orders average $59.28 vs $60.13 without one. Giving discounts isn't increasing basket size.
Subscribers don't spend more. Subscribers average $59.49 vs $59.87 for non-subscribers. Only 27% of customers subscribe, but repeat buyers (more than 5 previous purchases) subscribe more often: 27.6% vs 22.4%.
Men produce 68% of revenue through volume, not spend. Men $157,890 vs women $75,191, but average order is nearly identical ($59.54 vs $60.25). Men place about twice as many orders (2,652 vs 1,248).
Clothing is the core category at $104,264, about 45% of revenue. Outerwear is the smallest and peaks in Fall.
No state or age group dominates. The top state (Montana, $5,784) is only about 2.5% of revenue, and the four age groups each bring $56K-$62K.

Recommendation: test lower discount frequency to protect margin, and focus growth on acquiring customers (especially women) rather than on upselling existing ones.

Data cleaning (Python)
Checked for duplicates (none) and confirmed customer_id is unique.
Filled 37 missing review ratings (0.9%) with the median rating of the same category.
Renamed columns to snake_case.
Dropped promo_code_used, which was identical to discount_applied in every row.
Added age_group (four equal-sized quartile groups) and purchase_frequency_days ("Bi-Weekly" assumed to mean every 2 weeks, since the data doesn't define it).
SQL questions (sql/)
Revenue by gender
Discounted orders that still beat the average
Top 5 products by review rating
Standard vs Express shipping spend
Subscriber vs non-subscriber spend and revenue
Products with the highest discount rate
Customer segments (New / Returning / Loyal)
Top 3 products in each category (DENSE_RANK)
Are repeat buyers more likely to subscribe?
Revenue by age group
Revenue by category and season
Top 10 states
Payment method breakdown
Project structure
customer-behavior-analysis/
├── README.md
├── data/        raw CSV and the cleaned CSV
├── notebooks/   Jupyter notebook (cleaning, EDA, load to MySQL)
├── sql/         the 13 queries
├── powerbi/     dashboard (.pbix)
└── images/      dashboard screenshot
How to run
Install the libraries: pip install pandas matplotlib seaborn sqlalchemy pymysql
Create the database in MySQL: CREATE DATABASE customer_behavior;
Set your MySQL password as an environment variable (never write it in the code), then run the notebook top to bottom. The last cell loads the cleaned table into MySQL as customer.
Run the queries in sql/ in MySQL Workbench.
Open the .pbix file in Power BI Desktop (free, Windows).
Limitations

The data looks synthetic: purchase amounts, ages and ratings are spread almost evenly with no extreme values, and there are no dates, so trends over time can't be studied. Findings describe this dataset, not a real retailer.
