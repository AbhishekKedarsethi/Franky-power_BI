# FRANKY -Dashboard

##### Dashboard Link : https://app.powerbi.com/groups/me/reports/eacffd43-150d-400b-a49b-695ff415eb3e/eefad415b497bb3ad650?experience=power-bi

## Problem Statement

This dashboard provides invaluable insights into customer behavior, enabling the owner to monitor orders, revenue, returns, and profit effectively. By leveraging this tool, the owner can access detailed customer information and identify the geographical areas generating the most orders. For instance, if the majority of orders are coming from urban areas, targeted marketing campaigns can be developed to further penetrate these markets.

Additionally, the dashboard categorizes products into various product brands and product names, highlighting areas for improvement. For example, if a particular product shows higher return rates, the owner can investigate potential quality issues or mismatches in customer expectations.

Ultimately, this dashboard empowers the owner to refine their services and ensure a superior customer experience. By continuously analyzing data, the owner can stay ahead of trends, anticipate customer needs, and make informed decisions that drive business growth.

#### We have divided the total project into 4 parts 
- Loading and Shaping data
- Creating the data modeling
- Adding DAX Measures
- Building Reports


#### Loading and Shaping data (Steps followed)

- Step 1 : First we load data into Power BI Desktop and the datasets are csv files.

- Step 2 : Upon uploading the data "Promotion of Header" - (It recognizes the header and changes the column name to that specific header) and "Change of the data type" - (It changes the data type of the given datasets) automatically. 

- Step 3 : After that we will check all the headers and the change of the data type of the datasets are accurate. If they are not accurate then we should change it manually by selecting a particular column and in home ribbon there is an option for change the data type.

#### Note: id(unique numbers) should be in whole numbers and account number, product_sku, postal_code should be in text because we do not perform calculation on those and by keeping in text data type helps in prevent the cahnge of the data.

- Step 4 : It was observed that in some of the columns errors & empty values were present so we can manually remove them and for some values we have replaced values by using replace value in transform ribbon.

- Step 5 : After checking and cleaning the data. We have merged 2 columns "First Name", "Last Name" and named it as "Full Name". A new column "birth_year" is extracted from "birthdate" column.

- Step 6 : We have created conditional column "Has_children" if column "total_children" = 0 then "No" else "Yes". And in product table we created a new column as "Discount price" by multipying "product price" column to 0.9. 

- Step 7 : We have added a calculated column named "full_address", by merging "store_city", "store_state", and "store_country", separated by a comma and space and added a calculated column named "area_code", by extracting the characters before the dash ("-") in the "store_phone" field.

- Step 8 : We have loaded region file, return file and transaction folder which contains transactions of 1997 and 1998. And loaded a new table "Calendar" and we created the following columns

- Start of Week (starting Sunday)

- Name of Day

- Start of Month

- Name of Month

- Quarter of Year

- Year 

By this we have completed the first part of the project.(ie Cleaning and Shaping data)

#### Creating the data Modeling

- Step 1 : First we go to the data model view connect the tables from one table to another by using primary keys and Foreign keys.

- Step 2 : We have connected transactions table with customer table, calendar table, Products table, Stores table.

- Step 3 : We have also connected Returns table with Store table, Products table, Calendar table. The store table is connected to Regions table by "Snowflake schema".

- Step 4 : We have to check all the connected filters are one-to-many (ie one-way filters) and all relationships follow one-to-many cardinality, with primary keys (1) on the lookup side and foreign keys (*) on the data side.

- Step 5 : We have hidden all the foreign keys which are present in the tables so that when we create report we don't get confuse and pick the wrong column.

- Step 6 : We have updated the format such as currency, Postal code, country, city, address so that it will be easy to make report and will have some new ways to look into our data such as maps and dates. By this we have completed "Creating the Data model"

#### Adding DAX Measures

-Step 1 : in the data view we have created the calculated columens such as Weekend, End of month, Current age, Priority, Short_Country, House number, Price_Tire, Year_Since_Remodel by using DAX like if, left, switch, sum, count, etc.

Weekends : 

		Weekends = 
		SWITCH( ' calendar ' [Day Name], "Sunday", I' Yes" , "Saturday", "Yes" , "No")

Customer Priority :

		Customer priority = 
		IF (
			Customers [ homeowner] = "Y" &&
			Customers[member card] = "Golden",
			"High" , 
			"Standard" 
		)
House Number : 

		House Number = 
		LEFT( 
			Customers [customer address] 
			SEARCH("   ", Customers[customer address]) 
		)




- Step 2 : In the report view we have added the following measures such as Quantity sold, Quantity returned by using sum DAX functions and total transactions, total returns by using count DAX functions.

- Step 3 : We have also calculated Return rate by using divide DAX function and name a new function "Weekend Transactions" to calculate the transactions on weekends and  calculated the percentage(%) value of weekend transactions also.

Weekend Transactions :

		Weekend transactions = 
		CALCULATE( 
			[Total Transaction] 
			'Calendar ' [Weekends]  = "Yes " ) 

- Step 4 : We created new measures to calculate total revenue based on transaction quantity and product retail price & total cost based on transaction quantity and product cost by using sumx DAX function
Total Revenue :

		Total Revenue  = 
		SUMX( 
			'Transactions 1997&8' , 
			'Transactions 1997&8' [quantity] 
			*
			RELATED(Products[product retail price]) 
		)


- Step 5 : We have created new measures such as profit margin, unique products, total profit, etc by using basic DAX functions.

- Step 6 : A new measure named YTD Revenue to calculate year-to-date total revenue and another measure 60-days Revenue to calculate the running revenue total over a 60-days period.

YTC Revenue :

		YTD Revenue  =
		CALCULATE( 
			[Total Revenue] ,
			DATESYTD( ' calendar [date]) 
		)

60-days Revenue :

		60-days Revenue  = 
		CALCULATE( 
			[Total Revenue],
			DATESINPERIOD( 
				'Calendar' [date], 
				MAX( 'Calendar ' [date]), 
				-60, 
				DAY)
			)
		

- Step 7 : We have created Last month transactions, last month revenue, last month profit, last month returns and revenue traget. By this we have completed Adding DAX functions.

#### Building Reports

- Step 1 : In the report tab first we have inserted the logo of the company that we have created manually. And we have inserted a table with product brand on their rows and total transaction, total profit, profit margin, return rate as values.

Product Table:

<img width="244" alt="table" src="https://github.com/user-attachments/assets/75f45d2a-2a0f-414b-931a-eb5ce171efb6">



- Step 2 : We have used KPI to show the monthly updates on revenue, return, profit, transactions. And we have edited those by using format options.

KPI :

<img width="445" alt="KPIs" src="https://github.com/user-attachments/assets/a98509a9-dead-4786-a8f1-f9f4481600da">

- Step 3 : We have add a graph to show the total transactions occure in time. In the graph we can observer transactions on yearly basis, quaterly, monthly, weekly and daily also.

Graph : 

<img width="317" alt="Graph" src="https://github.com/user-attachments/assets/0994e191-adba-49c5-bcde-a9ab1ad153ab">



- Step 4 : We have created maps on another page to show the revenue in respective countries.

- Step 5 : We have also created a bar-chart to show the total revenue by occupation and we have also inseted a slicer which sorts by membership card.

Dashboard :

<img width="610" alt="dashboard" src="https://github.com/user-attachments/assets/cc330d4c-ae50-403e-b43f-dc88ebc45b68">

- Step 6 : We have instered a bar-graph for city and profit in a new page and a donut chart which was sorted by transactions and customer priority.

- Step 7 : We have also inserted treemap for countries and we can drill down to cities also.

Treemap :

<img width="333" alt="treemap" src="https://github.com/user-attachments/assets/5e5ff099-9b47-4c70-a239-06de17f3f37e">

- Step 8 : We have inserted 2 cards for customer with most transactions and product with most transactions.

Cards :

<img width="143" alt="Cards" src="https://github.com/user-attachments/assets/bb1106db-d2fe-425d-b59e-40e0797707a9">


We have named the new page as "Additional Details" :

<img width="605" alt="Additional" src="https://github.com/user-attachments/assets/ec47a55d-7961-4d60-8ea0-b6af834b84d9">

By using this dashboard we can obtain many insights and answers complicated questions by using those insights.

1) In october 1997 to january 1998 there was a huge growth in Transactions. By finding out what are the changes took place we can implement those changes to reach higher trainsactions in the other months also.

2) The bronze membership card owners generated a revenue of 67k which is 7% higher then the last month revenue.

3) The customer with most of the transactions is "Dawn Laner" and the product with most of the transactions is "Moms Roasted Chicken".

4) Most of the transactions are from USA with 1,80,823 and next is mexico with 72,860 and canada with 16,091 transactions.


