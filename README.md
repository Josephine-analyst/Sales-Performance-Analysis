# SALES PERFORMANCE ANALYSIS SQL PROJECT

## PROJECT DESCRIPTION
   This project demonstrates advanced **SQL querying skills** by analyzing a relational sales database. The goal is to extract actionable business insights 
   from tables containing orders, customers, products, stores, regions, and sales teams.

### Key business questions answered include:
    - What is the **total profit** and **quantity sold**?
    - Which **regions**, **products**, **sales channels**, and **sales team members** drive the most profit/revenue?
    - Who are the **top-performing customers**?
    - How is profit distributed **geographically** and **by channel**?

### TECHNOLOGY & TOOLS USED
    - **Database**: MySQL
    - **IDE**: MySQL Workbench
    - **Data Format**: CSV imports


### STEPS FOLLOWED
    * Step 1: Open sql workbench
    * Step 2: Create a database (schema), titled "my_sql_project"
    * Step 3: Load data `sales order` into the database  by right-clicking on the database>Table Data Import Wizard>Browse through the file path, to select
              dataset needed, the dataset is a csv(comma separated value) file.
    * Step 4: Query the table to ensure the dataset is loaded correctly
               
               select * from `sales order`;
               select * from store_sales;

### DATA CLEANING
    * Step 5: Check the dataset for any duplicates, missing values,inconsistent datas and empty cells.
    * Step 6: Write a query to modify (change) the column's data types.


### ADDING A NEW COLUMN
    Write a query to add the following columns to your database:
    * Total Selling Price (TSP)
    * Total Cost Price (TCP)
    * Profit
    * Total Discount Applied (TDA)


### UPDATING THE COLUMNS TCP,TSP,PROFIT AND TDA
    update `sales order`
    set TSP=Unit_Price*Order_Quantity;

    update `sales order`
    set TCP=Unit_Cost*Order_Quantity;

    update `sales order`
    set PROFIT=(TSP-TDA)-TCP;

    update `sales order`
    set TDA=Discount_Applied*Order_Quantity;


### CALCULATE THE TOTAL PROFIT (TSP-DISCOUNT)-TCP
    It determines the profit earned from selling goods by accounting for the sales price, discount applied and the cost price.

         Query: select sum(PROFIT) as Total_Profit from `sales order`;

<img width="1366" height="768" alt="Screenshot (125)" src="https://github.com/user-attachments/assets/7b7e9aaf-83fd-45a3-8700-f42dc5837e9c" />
  

### TOTAL QUANTITY SOLD
    Refers to the toatal number of units of a product sold over a specific period.

         Query:  select sum(Order_Quantity) as Total_Quantity_Sold from `sales order`;

<img width="1366" height="768" alt="Screenshot (126)" src="https://github.com/user-attachments/assets/d3240244-2b4f-4ded-b395-fa7d48e5ba8c" />

### TOTAL PROFIT BY REGION
    Refers to profit earned from sales in different geographic areas. This analysis helps businesses understand which region are most profitable,
    identify regional performance trends and inform decisions like pricing strategies or marketing campaigns.

         Query: with masterdata as(
                select store_sales.Store_ID,store_sales.City_Name,store_sales.County,store_sales.State,
                store_sales.Latitude,store_sales.Longitude,store_sales.Population,store_sales.Household_Income,
                store_sales.Median_Income,Region.Region from store_sales
                join Region
                on store_sales.State=Region.State
                )
                select * from masterdata;
<img width="1366" height="768" alt="Screenshot (118)" src="https://github.com/user-attachments/assets/24ff4338-0f7a-4846-a0c6-397b984a6cb1" />

                

### PRODUCT THAT CONTRIBUTE THE MOST TO PROFIT IN EACH REGION
    The product that contribute the most to profit in each region is the one with the highest total profit after applying the query below.
    
        Query: select * from masterdata1;
               select Product_ID,Region,round(sum(PROFIT),1) as `Top Product by Region` from masterdata1
               group by Product_ID,Region
               order by `Top Product by Region` desc
               limit 1;

 <img width="1366" height="768" alt="Screenshot (119)" src="https://github.com/user-attachments/assets/b7d80f4d-daf2-41c8-9b0e-d746f9c2c851" />
      

### HOW DIFFERENT SALES CHANNELS AFFECTS STORE PROFIT
    Each sales channel affects total profit by influencing the total sales price, discount, total cost price and total quantity sold as these
    factors varies across channels due to differences in pricing, costs, customer reach and operational requirements.

        Query: select * from masterdata1;
               select Sales_Channel,round(sum(PROFIT),1) as 'Profit by Sales_Channels' from masterdata1
               group by Sales_Channel
               order by 'Profit by Sales_Channels' desc;

<img width="1366" height="768" alt="Screenshot (120)" src="https://github.com/user-attachments/assets/5132c6e0-6ac5-4bbb-8cb6-52eeba6a0078" />
           

### AVERAGE PROFIT ACROSS REGIONS
    Is the mean profit earned from sales across multiple geographic regions, this metric helps businesses understand overall profitability while
    accounting for regional, variations in sales, discount, costs and sales channels. It is useful for comparing performance across regions and 
    identifying trends.
        Query: select * from masterdata1;
               select Region,round(avg(PROFIT),1) as 'Average profit across Region' from masterdata1
               group by Region;

<img width="1366" height="768" alt="Screenshot (121)" src="https://github.com/user-attachments/assets/5ac9a27d-333d-4cb9-8062-432ef02d9948" />
           

### THE TOP TEN CUSTOMERS IN TERMS OF REVENUE GENERATION
    Are those contributing the highest total sales price across all regions which can then be analyzed for profitability.
        Query: select * from masterdata1;
               select* from Customer;

               select Customer_Names,round(sum(PROFIT),1) as `Top Ten Customers By Revenue` from customerdata
               group by Customer_Names
               order by `Top Ten Customers By Revenue` desc
               limit 10;

<img width="1366" height="768" alt="Screenshot (122)" src="https://github.com/user-attachments/assets/38face1b-9e8a-48a1-8bc0-c2c78aa0a9a6" />
    

### GEOGRAPHICAL DISTRIBUTION OF CUSTOMERS
    This refers to the spread of customers across different geogrpaphic regions based on their purchasing activity. This analysis helps businessses
    understand regional demand, optimize marketing assess channel effectiveness.
    
        Query: select Region,count(Customer_Names) as `Geographical Distribution of Customer` from customerdata
               group by Region;

<img width="1366" height="768" alt="Screenshot (123)" src="https://github.com/user-attachments/assets/71bdc756-d16f-4261-9311-21b40db6def0" />
         

### SALES TEAM MEMBERS DRIVING THE MOST REVENUE
    It involves calculating the total sales price generated by each team member. This query identifies top performing team members driving the most 
    revenue.

        Query: select Sales_Team,round(sum(PROFIT),1) as `Sales Teams Driving the most Revenue` from `salesteam data`
               group by Sales_Team
               order by `Sales Teams Driving the most Revenue` desc
               limit 7;

<img width="1366" height="768" alt="Screenshot (124)" src="https://github.com/user-attachments/assets/e1865775-e213-4201-a24a-24ba99dc4bd9" />
         

### KEY INSIGHTS
    1. Underperforming products may need repricing or marketing.
    2. High performing regions may justify increased investment; low performing regions may need market research campaigns.
    3. Channel with high revenue but low average order values may benefit from upselling strategies.
    4. Top performers can share best practices; under performers may need trainning or reassignment.

### CONCLUSION
    This project uncovers critical patterns such as revenue trends, top performing products, customer segments, geographic performance, channel effectiveness 
    and profitability. Key approaches include aggregating data with GROUP BY, using Joins to combine tables(e.g sales, products, customers), optimizing queries 
    with indexes for performance.
    





