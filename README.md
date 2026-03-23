# AdventureWorksReport

<img width="1269" height="715" alt="image" src="https://github.com/user-attachments/assets/2e459227-22bf-4286-a117-9f3d4cebdd81" />
<br>
<br>
In this Power BI project, I developed a comprehensive report based on the AdventureWorks dataset, a fictional company specializing in bicycle sales and related products. The focus of the project was to build a structured data model, implement advanced calculations, and deliver meaningful business insights through interactive visualizations.

## Data Loading and Transformation

At the beginning of the project, I imported multiple datasets into Power BI and performed initial transformations using the Power Query Editor. Each table was reviewed to ensure correct data types, consistent formatting, and overall data quality before building the model.

The data primarily consisted of fact and dimension tables, including sales, customers, products, and territories. During the transformation process, I removed unnecessary columns, handled missing values where needed, and ensured that key fields were properly structured for establishing relationships.

In the product-related tables, I verified category hierarchies and ensured that product attributes such as subcategory and category were correctly aligned. In the customer data, I validated geographic and demographic fields to support segmentation and regional analysis.

The calendar table was prepared to support time-based analysis, with properly formatted date fields that could be used for time intelligence calculations.

Additionally, I ensured that all numerical fields such as sales amount, cost, and quantity were assigned appropriate data types and formatting. This preparation step was essential for enabling accurate calculations and efficient data modeling in later stages of the project.

## Creating the Data Model

In the Relationships view, I developed the data model by connecting the Sales Data table as the primary fact table to several lookup tables, including Customer Lookup, Product Lookup, Product Subcategories Lookup, Product Categories Lookup, Calendar Lookup, and Territory Lookup, using primary and foreign keys. This structure enabled a clear separation between transactional data and descriptive dimensions.

I also incorporated the Returns Data table as a secondary fact table and linked it to the Product Lookup, Calendar Lookup, and Territory Lookup tables. Both fact tables were connected through shared lookup tables, without any direct relationship between them, ensuring a clean and scalable design.

The Calendar Lookup table played a key role in supporting time-based analysis by providing a consistent date reference across both Sales and Returns data. All relationships were configured as one-to-many, with a single-direction filter flow from the lookup tables toward the fact tables to maintain optimal performance and accurate filtering behavior.

The final model followed best practices by relying on shared dimensions and avoiding unnecessary complexity, resulting in a well-structured foundation for advanced reporting and analysis.


<img width="1213" height="610" alt="image" src="https://github.com/user-attachments/assets/62b64c91-eded-4919-a1d9-0f78b496fb47" />

## Dashboards 

In this AdventureWorks Power BI project, I created multiple interactive dashboards to provide comprehensive business insights across sales, products, and customers.

The Executive Dashboard serves as a high-level overview, displaying key performance indicators such as total sales, total profit, and order quantities. It allows management to quickly assess business performance and track trends over time.

The Map Dashboard provides a geographic view of sales and returns by region and territory, enabling analysis of regional performance and identification of high- and low-performing areas. It uses color-coded map visuals to make regional patterns immediately visible.

<img width="1264" height="706" alt="image" src="https://github.com/user-attachments/assets/c34b482e-9ea7-485b-9ab7-1ccae499368e" />

<br>
<br>

The Product Detail Dashboard focuses on product-level analysis, showing metrics such as Profit Trending and Returns trending. It enables product managers to drill down into individual products or categories for targeted insights.

<img width="1257" height="715" alt="image" src="https://github.com/user-attachments/assets/af1372e0-cfee-455a-ae48-6d9f332c057c" />


<br>
<br>

The Customer Detail Dashboard provides insights into customer behavior and segmentation. It includes metrics, income level, occupation, weekly customers, top customers by revenue, allowing detailed analysis of customer demographics and purchasing patterns.

<img width="1236" height="711" alt="image" src="https://github.com/user-attachments/assets/8f359762-1348-4856-88c1-7d1e7f5658a1" />


## Calculated Columns

In the Power BI Data view, I enhanced the data model by creating calculated columns across multiple tables to improve analytical capabilities and enable better segmentation. In the Calendar Lookup table, I added a Day of Week column using the WEEKDAY function, a Month Short column that returns the first three uppercase letters of the month name, and a Weekend column that classifies days as “Weekend” or “Weekday” based on the day number.

In the Customer Lookup table, I created a Birth Year column by extracting the year from the birth date, as well as a Customer Full Name column by concatenating prefix, first name, and last name. I also added a Customer Priority column that flags customers as “Priority” if they are parents with annual income above 100,000, otherwise “Standard.” Additionally, I defined an Education Category column to group education levels into broader categories, an Income Level column to segment customers based on income thresholds, and an Is Parent column derived from the total number of children.

In the Product Lookup table, I introduced a Price Point column to categorize products as “High,” “Mid-Range,” or “Low” based on price, along with an SKU Category column that extracts the prefix of the product SKU using the SEARCH function.

Finally, in the Sales Data table, I added a Quantity Type column to distinguish between single-item and multiple-item orders based on order quantity. These calculated columns enhance the model by enabling more flexible filtering, segmentation, and deeper analytical insights within reports.

```Day of Week = WEEKDAY('Calendar Lookup'[Date], 2)``` <br>

```Month Short = UPPER(LEFT('Calendar Lookup'[Month Name], 3))``` <br>

```Weekend = IF('Calendar Lookup'[Day of Week] IN {6,7}, "Weekend", "Weekday")``` <br>

```Birth Year = YEAR('Customer Lookup'[BirthDate]), Customer Full Name (CC) = 'Customer Lookup'[Prefix] & " " & 'Customer Lookup'[FirstName] & " " & 'Customer Lookup'[LastName]``` <br>

```Customer Priority = IF('Customer Lookup'[Is Parent ?] = "Yes" && 'Customer Lookup'[AnnualIncome] > 100000, "Priority", "Standard")``` <br>

```Education Category = SWITCH('Customer Lookup'[EducationLevel], "High School", "High School", "Partial High School", "High School", "Bachelors", "Undergrad", "Partial College", "Undergrad", "Graduate Degree", "Graduate", "Other")``` <br>

```Income Level = IF('Customer Lookup'[AnnualIncome] >= 150000, "Very High", IF('Customer Lookup'[AnnualIncome] >= 100000, "High", IF('Customer Lookup'[AnnualIncome] >= 50000, "Average", "Low")))``` <br>

```Is Parent ? = IF('Customer Lookup'[TotalChildren] > 0, "Yes", "No")``` <br>

```Price Point = SWITCH(TRUE(), 'Product Lookup'[ProductPrice] > 500, "High", 'Product Lookup'[ProductPrice] > 100, "Mid-Range", "Low")``` <br>

```SKU Category = LEFT('Product Lookup'[ProductSKU], SEARCH("-", 'Product Lookup'[ProductSKU]) - 1)``` <br>

```Quantity Type = IF('Sales Data'[OrderQuantity] > 1, "Multiple Items", "Single Item")``` <br>

## Measures

```% of All Orders = DIVIDE([Total Orders], [All Orders])```<br>

```% of All Returns = DIVIDE([Total Returns], [All Returns])```<br>

```10 day Rolling Revenue = CALCULATE([Total Revenue], DATESINPERIOD('Calendar Lookup'[Date], MAX('Calendar Lookup'[Date]), -10, DAY))```<br>

```90-day Rolling Profit = CALCULATE([Total Profit], DATESINPERIOD('Calendar Lookup'[Date], MAX('Calendar Lookup'[Date]), -90, DAY))```<br>

```All Orders = CALCULATE([Total Orders], ALL('Sales Data'))```<br>

```All Returns = CALCULATE([Total Returns], ALL('Returns Data'))```<br>

```Average Retail Price = AVERAGE('Product Lookup'[ProductPrice])```<br>

```Average Revenue per Customer = DIVIDE([Total Revenue], [Total Customers])```<br>

```Bike Return Rate = CALCULATE([Return Rate], 'Product Categories Lookup'[CategoryName] = "Bikes")```<br>

```Bike Returns = CALCULATE([Total Returns], 'Product Categories Lookup'[CategoryName] = "Bikes")```<br>

```Bike Sales = CALCULATE([Quantity Sold], 'Product Categories Lookup'[CategoryName] = "Bikes")```<br>

```Bulk Orders = CALCULATE([Total Orders], 'Sales Data'[OrderQuantity] > 1)```<br>

```High Ticket Orders = CALCULATE([Total Orders], FILTER('Product Lookup', 'Product Lookup'[ProductPrice] > [Overall Average Price]))```<br>

```Order Target = [Previous Month Orders] * 1.1```<br>

```Order Target Gap = [Total Orders] - [Order Target]```<br>

```Overall Average Price = CALCULATE([Average Retail Price], ALL('Product Lookup'))```<br>

```Previous Month Orders = CALCULATE([Total Orders], DATEADD('Calendar Lookup'[Date], -1, MONTH))```<br>

```Previous Month Returns = CALCULATE([Total Returns], DATEADD('Calendar Lookup'[Date], -1, MONTH))```<br>

```Previous Month Revenue = CALCULATE([Total Revenue], DATEADD('Calendar Lookup'[Date], -1, MONTH))```<br>

```Prevous Month Profit = CALCULATE([Total Profit], DATEADD('Calendar Lookup'[Date], -1, MONTH))```<br>

```Profit Target = [Prevous Month Profit] * 1.1```<br>

```Profit Target Gap = [Total Profit] - [Profit Target]```<br>

```Quantity Returned = SUM('Returns Data'[ReturnQuantity])```<br>

```Quantity Sold = SUM('Sales Data'[OrderQuantity])```<br>

```Return Rate = DIVIDE([Quantity Returned], [Quantity Sold], "No sales")```<br>

```Revenue Target = [Previous Month Revenue] * 1.1```<br>

```Revenue Target Gap = [Total Revenue] - [Revenue Target]```<br>

```Total Cost = SUMX('Sales Data', 'Sales Data'[OrderQuantity] * RELATED('Product Lookup'[ProductCost]))```<br>

```Total Customers = DISTINCTCOUNT('Sales Data'[CustomerKey])```<br>

```Total Orders = DISTINCTCOUNT('Sales Data'[OrderNumber])```<br>

```Total Profit = [Total Revenue] - [Total Cost]```<br>

```Total Returns = COUNT('Returns Data'[ReturnQuantity])```<br>

```Total Revenue = SUMX('Sales Data', 'Sales Data'[OrderQuantity] * RELATED('Product Lookup'[ProductPrice]))```<br>

```Weekend Orders = CALCULATE([Total Orders], 'Calendar Lookup'[Weekend] = "Weekend")```<br>

```YTD Revenue = CALCULATE([Total Revenue], DATESYTD('Calendar Lookup'[Date]))```<br>

