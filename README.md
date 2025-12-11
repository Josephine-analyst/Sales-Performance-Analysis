# SALES PERFORMANCE ANALYSIS SQL PROJECT

## PROJECT DESCRIPTION
   This project involves quering a relational database containing sales-related data (e.g orders, customers,revenue and regions) to generate
   meaningful reports and metrics. The analysis will focus on key business questions such as Total Profit by Region, Top-Performing Products,
   Average Profit across regions etc.

### SOFTWARE USED
    * SQL WORKBENCH

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

### TOTAL QUANTITY SOLD
    Refers to the toatal number of units of a product sold over a specific period.

         Query:  select sum(Order_Quantity) as Total_Quantity_Sold from `sales order`;

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

### PRODUCT THAT CONTRIBUTE THE MOST TO PROFIT IN EACH REGION
    The product that contribute the most to profit in each region is the one with the highest total profit after applying the query below.
    
        Query: select * from masterdata1;
               select Product_ID,Region,round(sum(PROFIT),1) as `Top Product by Region` from masterdata1
               group by Product_ID,Region
               order by `Top Product by Region` desc
               limit 1;

### HOW DIFFERENT SALES CHANNELS AFFECTS STORE PROFIT
    Each sales channel affects total profit by influencing the total sales price, discount, total cost price and total quantity sold as these
    factors varies across channels due to differences in pricing, costs, customer reach and operational requirements.

        Query: select * from masterdata1;
               select Sales_Channel,round(sum(PROFIT),1) as 'Profit by Sales_Channels' from masterdata1
               group by Sales_Channel
               order by 'Profit by Sales_Channels' desc;

### AVERAGE PROFIT ACROSS REGIONS
    Is the mean profit earned from sales across multiple geographic regions, this metric helps businesses understand overall profitability while
    accounting for regional, variations in sales, discount, costs and sales channels. It is useful for comparing performance across regions and 
    identifying trends.
        Query: select * from masterdata1;
               select Region,round(avg(PROFIT),1) as 'Average profit across Region' from masterdata1
               group by Region;

### THE TOP TEN CUSTOMERS IN TERMS OF REVENUE GENERATION
    Are those contributing the highest total sales price across all regions which can then be analyzed for profitability.
        Query: select * from masterdata1;
               select* from Customer;

               select Customer_Names,round(sum(PROFIT),1) as `Top Ten Customers By Revenue` from customerdata
               group by Customer_Names
               order by `Top Ten Customers By Revenue` desc
               limit 10;

### GEOGRAPHICAL DISTRIBUTION OF CUSTOMERS
    This refers to the spread of customers across different geogrpaphic regions based on their purchasing activity. This analysis helps businessses
    understand regional demand, optimize marketing assess channel effectiveness.
    
        Query: select Region,count(Customer_Names) as `Geographical Distribution of Customer` from customerdata
               group by Region;

### SALES TEAM MEMBERS DRIVING THE MOST REVENUE
    It involves calculating the total sales price generated by each team member. This query identifies top performing team members driving the most 
    revenue.

        Query: select Sales_Team,round(sum(PROFIT),1) as `Sales Teams Driving the most Revenue` from `salesteam data`
               group by Sales_Team
               order by `Sales Teams Driving the most Revenue` desc
               limit 7;

### KEY INSIGHTS
    1. Underperforming products may need repricing or marketing.
    2. High performing regions may justify increased investment; low performing regions may need market research campaigns.
    3. Channel with high revenue but low average order values may benefit from upselling strategies.
    4. Top performers can share best practices; under performers may need trainning or reassignment.

### CONCLUSION
    This project uncovers critical patterns such as revenue trends, top performing products, customer segments, geographic performance, channel effectiveness 
    and profitability. Key approaches include aggregating data with GROUP BY, using Joins to combine tables(e.g sales, products, customers), optimizing queries 
    with indexes for performance.
    





