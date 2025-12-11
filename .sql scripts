set SQL_SAFE_UPDATES=0;

CREATE DATABASE MY_SQL_PROJECT;

# selecting a database
use MY_SQL_PROJECT;

select * from `sales order`;
select * from store_sales;
describe `sales order`;

alter table `sales order`
modify column Procured_Date date,
modify column Order_Date date,
modify column Ship_Date date,
modify column Delivery_Date date;

select coalesce(str_to_date(Procured_Date,'%d/%m/%Y'),str_to_date(Procured_Date,'%Y/%m/%d')) from `sales order`;
select coalesce(str_to_date(Order_Date,'%d/%m/%Y'),str_to_date(Order_Date,'%Y/%m/%d')) from `sales order`;
select coalesce(str_to_date(Ship_Date,'%d/%m/%Y'),str_to_date(Ship_Date,'%Y/%m/%d')) from `sales order`;
select coalesce(str_to_date(Delivery_Date,'%d/%m/%Y'),str_to_date(Delivery_Date,'%Y/%m/%d')) from `sales order`;

update `sales order`
set Procured_Date=coalesce(str_to_date(Procured_Date,'%Y-%m-%d'),str_to_date(Procured_Date,'%Y-%m-%d'));

update `sales order`
set Order_Date=coalesce(str_to_date(Order_Date,'%d/%m/%Y'),str_to_date(Order_Date,'%Y-%m-%d'));

update `sales order`
set Ship_Date=coalesce(str_to_date(Ship_Date,'%d/%m/%Y'),str_to_date(Ship_Date,'%Y-%m-%d'));

update `sales order`
set Delivery_Date=coalesce(str_to_date(Delivery_Date,'%d/%m/%Y'),str_to_date(Delivery_Date,'%Y-%m-%d'));

# ADDING A NEW COLUMN TSP
alter table `sales order`
add column TSP double;

# ADDING A NEW COLUMN TCP AND PROFIT
alter table `sales order`
add column TCP double,
add column PROFIT double;

alter table `sales order`
add column TDA double;

# UPDATING THE COLUMNS TSP, TCP AND PROFIT

update `sales order`
set TSP=Unit_Price*Order_Quantity;
update `sales order`
set TCP=Unit_Cost*Order_Quantity;
update `sales order`
set PROFIT=(TSP-TDA)-TCP;
update `sales order`
set TDA=Discount_Applied*Order_Quantity;

# CALCULATE THE TOTAL PROFIT(TSP-DISCOUNT)-TCP

select * from `sales order`;

select sum(PROFIT) as Total_Profit from `sales order`;

# TOTAL QUANTITY SOLD

select sum(Order_Quantity) as Total_Quantity_Sold from `sales order`;

# COUNT OF CUSTOMERS

select * from `sales order`;

select count(customer_ID) as Count_Of_Customers from `sales order`;

# TOTAL PROFIT BY REGION

with masterdata as(
select store_sales.Store_ID,store_sales.City_Name,store_sales.County,store_sales.State,
store_sales.Latitude,store_sales.Longitude,store_sales.Population,store_sales.Household_Income,
store_sales.Median_Income,Region.Region from store_sales
join Region
on store_sales.State=Region.State
)
select * from masterdata;
 
 Create table masterdata_new as
 with masterdata as(
 select store_sales.Store_ID,store_sales.City_Name,store_sales.County,store_sales.State,
store_sales.Latitude,store_sales.Longitude,store_sales.Population,store_sales.Household_Income,
store_sales.Median_Income,Region.Region from store_sales
join Region
on store_sales.State=Region.State
)
select * from masterdata;
describe masterdata_new;

 
with masterdata1 as(
select masterdata_new.Store_ID,masterdata_new.County,masterdata_new.State,masterdata_new.Region,`sales order`.Order_Number,`sales order`.Sales_Channel,
`sales order`.Customer_ID,`sales order`.Product_ID,`sales order`.Order_Quantity,`sales order`.Discount_Applied,`sales order`.Unit_Price,
`sales order`.Unit_Cost from masterdata_new
join `Sales Order`
on masterdata_new.Store_ID=`sales order`.Store_ID
)
select * from masterdata1;

drop table masterdata1;
Create table masterdata1 as
 with masterdata1 as(
 select masterdata_new.Store_ID,masterdata_new.County,masterdata_new.State,masterdata_new.Region,`sales order`.Order_Number,`sales order`.Sales_Channel,
`sales order`.Customer_ID,`sales order`.Product_ID,`sales order`.Order_Quantity,`sales order`.Discount_Applied,`sales order`.Unit_Price,
`sales order`.Unit_Cost from masterdata_new
join `Sales Order`
on masterdata_new.Store_ID=`sales order`.Store_ID
)
select * from masterdata1;

# ADDING AN EXTRA COLUMNS

alter table masterdata1
add column TSP double,
add column TCP double,
add column PROFIT double,
add column TDA double;

# UPDATING MASTERDATA1

update masterdata1
set TSP=Unit_Price*Order_Quantity;
update masterdata1
set TCP=Unit_Cost*Order_Quantity;
update masterdata1
set PROFIT=(TSP-TDA)-TCP;
update masterdata1
set TDA=Discount_Applied*Order_Quantity;


select Region,round(sum(PROFIT),1) as `Total Profit by Region` from masterdata1
group by Region
order by `Total Profit by Region` desc
limit 1;

# PRODUCT THAT CONTRIBUTE THE MOST TO PROFIT IN EACH REGION

select * from masterdata1;

select Product_ID,Region,round(sum(PROFIT),1) as `Top Product by Region` from masterdata1
group by Product_ID,Region
order by `Top Product by Region` desc
limit 1;

# HOW DIFFERENT SALES CHANNELS AFFECTS STORE PROFIT

select * from masterdata1;

select Sales_Channel,round(sum(PROFIT),1) as 'Profit by Sales_Channels' from masterdata1
group by Sales_Channel
order by 'Profit by Sales_Channels' desc;

# AVERAGE PROFIT ACROSS REGIONS

select * from masterdata1;

select Region,round(avg(PROFIT),1) as 'Average profit across Region' from masterdata1
group by Region;

# THE TOP TEN CUSTOMERS IN TERMS OF REVENUE GENERATION

select * from masterdata1;
select* from Customer;

with customerdata as(
select masterdata1.Store_ID,masterdata1.Customer_ID,Customer.Customer_Names,masterdata1.County,masterdata1.State,masterdata1.Region,masterdata1.Order_Number,masterdata1.Sales_Channel,
masterdata1.Product_ID,masterdata1.Order_Quantity,masterdata1.TDA,masterdata1.TSP,masterdata1.PROFIT,
masterdata1.TCP from masterdata1
join Customer
on masterdata1.Customer_ID=customer.Customer_ID
)
select * from customerdata;

Create Table Customerdata as
with customerdata as(
select masterdata1.Store_ID,masterdata1.Customer_ID,Customer.Customer_Names,masterdata1.County,masterdata1.State,masterdata1.Region,masterdata1.Order_Number,masterdata1.Sales_Channel,
masterdata1.Product_ID,masterdata1.Order_Quantity,masterdata1.TDA,masterdata1.TSP,masterdata1.PROFIT,
masterdata1.TCP from masterdata1
join Customer
on masterdata1.Customer_ID=customer.Customer_ID
)
select * from customerdata;

select Customer_Names,round(sum(PROFIT),1) as `Top Ten Customers By Revenue` from customerdata
group by Customer_Names
order by `Top Ten Customers By Revenue` desc
limit 10;

# GEOGRAPHICAL DISTRIBUTION OF CUSTOMERS

select Region,count(Customer_Names) as `Geographical Distribution of Customer` from customerdata
group by Region;

# SALES TEAM MEMBERS DRIVING THE MOST REVENUE

with `salesteam data`as(
select masterdata1.Customer_ID,`sales team`.Sales_Team_ID,`sales team`.Sales_Team,masterdata1.Region,
masterdata1.Product_ID,masterdata1.Order_Quantity,masterdata1.TDA,masterdata1.TSP,masterdata1.PROFIT,
masterdata1.TCP from masterdata1
join `sales team`
on masterdata1.Region=`sales team`.Region
)
select * from `salesteam data`;

Create table `Salesteam data` as
with `salesteam data`as(
select masterdata1.Customer_ID,`sales team`.Sales_Team_ID,`sales team`.Sales_Team,masterdata1.Region,
masterdata1.Product_ID,masterdata1.Order_Quantity,masterdata1.TDA,masterdata1.TSP,masterdata1.PROFIT,
masterdata1.TCP from masterdata1
join `sales team`
on masterdata1.Region=`sales team`.Region
)
select * from `salesteam data`;

select Sales_Team,round(sum(PROFIT),1) as `Sales Teams Driving the most Revenue` from `salesteam data`
group by Sales_Team
order by `Sales Teams Driving the most Revenue` desc
limit 7;





 


