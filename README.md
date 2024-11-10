# Financial_Dashboard_Credit_Card
Credit Card Transaction and Customer Dashboard using Power BI

Project Objective
To develop a comprehensive credit card weekly dashboard that provides real-time insights into key performance metrics and trends, enabling stakeholders to monitor and analyse credit card operations effectively.
Import data to SQL database
1. Prepare CSV file
   
2.  Create tables in SQL and Import CSV file into SQL

Ceate database Credit_card_db in SQL
 1. Create table cc_detail
    CREATE TABLE cc_detail(
    Client_Num INT,
    Card_Category VARCHAR(20),
    Annual_Fees INT,
    Activation_30_Days INT,
    Customer_Acq_Cost INT,
    Week_Start_Date DATE,
    Week_Num VARCHAR(20),
    Qtr VARCHAR(10),
    current_year INT,
    Credit_Limit DECIMAL(10,2),
    Total_Revolving_Bal INT,
    Total_Trans_Amt INT,
    Total_Trans_Ct INT,
    Avg_Utilization_Ratio DECIMAL(10,3),
    Use_Chip VARCHAR(10),
    Exp_Type VARCHAR(50),
    Interest_Earned DECIMAL(10,3),
    Delinquent_Acc VARCHAR(5)
)

2. Import data into cc_detail table from csv files.
    Copy cc_detail
    FROM 'C:\credit_card.csv' 
    DELIMITER ','
    CSV HEADER

3. Create table cust_detail

    CREATE TABLE cust_detail(
    Client_Num INT,
    Customer_Age INT,
    Gender VARCHAR(5),
    Dependent_Count INT,
    Education_Level VARCHAR(50),
    Marital_Status VARCHAR(20),
    State_cd VARCHAR(50),
    Zipcode VARCHAR(20),
    Car_Owner VARCHAR(5),
    House_Owner VARCHAR(5),
    Personal_Loan VARCHAR(5),
    Contact VARCHAR(50),
    Customer_Job VARCHAR(50),
    Income INT,
    Cust_Satisfaction_Score INT
)

4. Import data into cust_detail table from csv files.
    Copy cc_detail
    FROM 'C:\cust_add.csv' 
    DELIMITER ','
    CSV HEADER

Create DAX Queries
AgeGroup = SWITCH(
  TRUE(),
  'public cust_detail'[customer_age] < 30, "20-30",
  'public cust_detail'[customer_age] >= 30 && 'public cust_detail'[customer_age] < 40, "30-40",
  'public cust_detail'[customer_age] >= 40 && 'public cust_detail'[customer_age] < 50, "40-50",
  'public cust_detail'[customer_age] >= 50 && 'public cust_detail'[customer_age] < 60, "50-60",
  'public cust_detail'[customer_age] >= 60, "60+",
  "unknown"
)

IncomeGroup = SWITCH(
  TRUE(),
  'public cust_detail'[income] < 35000, "Low",
  'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] < 70000, "Med",
  'public cust_detail'[income] >= 70000, "High",
  "unknown"
)

week_num2 = WEEKNUM('public cc_detail'[week_start_date])

Revenue = 'public cc_detail'[annual_fees] + 'public cc_detail'[total_trans_amt] + 'public cc_detail'[interest_earned]

Current_week_Revenue = CALCULATE (
	SUM('public cc_detail'[Revenue]),
         	FILTER( ALL('public cc_detail'), public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])))
    
Previous_week_Revenue = CALCULATE(
	SUM ('public cc_detail'[Revenue]),	
   	FILTER(ALL('public cc_detail'),
        'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2]) – 1))



Insights from the dashboard
•	Revenue increased by 28.8% in week 53.
Overview YTD:
•	Overall revenue is 57M
•	Total interest is 8M
•	Total transaction amount is 46M
•	Male customers are contributing more in revenue: 31M, female: 26M
•	Blue & Silver credit cards are contributing to 93% of overall transactions
•	TX, NY & CA are contributing to 68%
•	Overall Activation rate is 57.5%
•	Overall Delinquent rate is 6.06%


