# Credit_Card_Financial_Dashboard
Credit Card Transaction Power BI Dashboard
Project Objective: To develop a comprehensive credit card weekly dashboard that provides real-time insights into key performance metrics and trends, enabling stakeholders to monitor and analyze credit card operations effectively
Import data to SQL database
 1. Prepare csv file 
 2. Create tables in SQL
 3. import csv file into SQL



SQL Query used-
-- SQL Query to create and import data from csv files:

-- 0. Create a database 
CREATE DATABASE ccdb;

-- 1. Create cc_detail table

CREATE TABLE cc_detail (
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
);


-- 2. Create cc_detail table

CREATE TABLE cust_detail (
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
);


-- 3. Copy csv data into SQL (remember to update the file name and file location in below query)

-- copy cc_detail table

COPY cc_detail
FROM 'D:\credit_card.csv' 
DELIMITER ',' 
CSV HEADER;


-- copy cust_detail table

COPY cust_detail
FROM 'D:\customer.csv' 
DELIMITER ',' 
CSV HEADER;

-- copy additional data (week-53) in cc_detail table (To Check Real-time Update of Power BI Dashboard)

COPY cc_detail
FROM 'D:\cc_add.csv' 
DELIMITER ',' 
CSV HEADER;


-- copy additional data (week-53) in cust_detail table (remember to update the file name and file location in below query)

COPY cust_detail
FROM 'D:\cust_add.csv' 
DELIMITER ',' 
CSV HEADER;

-----------------------------------------------------------------------------------------------------------------------------------------
DAX Queries Used
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
 'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] <70000, "Med",
 'public cust_detail'[income] >= 70000, "High",
 "unknown"
 )

 week_num2 = WEEKNUM('public cc_detail'[week_start_date])
 Revenue = 'public cc_detail'[annual_fees] + 'public cc_detail'[total_trans_amt] + 'public cc_detail'[interest_earned]
 Current_week_Reveneue = CALCULATE(
 SUM('public cc_detail'[Revenue]),
 FILTER(
 ALL('public cc_detail'),
 'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2]))) 
Previous_week_Reveneue = CALCULATE(
 SUM('public cc_detail'[Revenue]),
 FILTER(
 ALL('public cc_detail'),
 'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])-1))

 -----------------------Credit  Card  Transaction  Report--------------------------------------
 ![image](https://github.com/user-attachments/assets/5e1f7064-0cf4-4055-818d-854769080615)
![image](https://github.com/user-attachments/assets/de6a3d9b-fd83-4bd1-9636-4f35bcedb589)
![image](https://github.com/user-attachments/assets/2c67cde9-9078-4bac-b4c3-b3c922b1e3bc)



----------------------------Credit Card Customer Report----------------------------------------
![image](https://github.com/user-attachments/assets/72472309-0483-492a-bbb8-7b4d1f0531d8)
![image](https://github.com/user-attachments/assets/585f4725-a1fe-40a8-8317-95e500d8e5f0)
![image](https://github.com/user-attachments/assets/468e2702-166b-42c6-b951-d20cbd97c095)



