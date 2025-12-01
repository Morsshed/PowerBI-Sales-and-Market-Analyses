# 1. Power-BI: E-Commerce-Sales-Analysis
This project includes Sales Analysis &amp; Forecasting, Territory Analysis, Customer Analysis and Product Analysis
[Interactive Power BI Dashboard](https://app.powerbi.com/view?r=eyJrIjoiMWRmNWI0YjItM2I1NC00MzZkLTlmZjctNDZiMzU2YzYzMTE0IiwidCI6IjFhOTM4M2ZmLTVlMDEtNDkzYy04MTJmLTg0ODAzZTliMGI3YiJ9)



# Analysis Details
 ## i. Business Case
 
🚩 Problem Statement:

The E-Commerce Sales Analytics Dashboard aims to address several key business challenges related to sales performance, customer behavior, product profitability, and regional growth. Despite generating strong revenue and profit, the organization faces gaps in customer retention, uneven regional performance, and product return issues that impact overall business efficiency. To support data-driven decision-making, the project focuses on identifying underlying issues and converting raw transactional data into meaningful insights.

The primary business problems addressed in this project include:

✔️ Declining revenue per customer at the start of each month, indicating potential behavioral or promotional timing gaps.

✔️ High return rates in specific product categories, especially Shorts, which negatively impact profit margins and customer satisfaction.

✔️ Regional sales imbalance, with North America outperforming other regions, highlighting untapped market potential in Europe, Asia-Pacific, and South America.

✔️ Dependence on a small group of high-performing products, increasing risk if demand shifts.

✔️ Suboptimal customer retention, where many customers fall into “Potential Loyalists” or “At-Risk” segments instead of moving into loyal or champion categories.

✔️ Insufficient demographic targeting, as the business lacks actionable insights into how age, gender, and marital status influence purchase behavior.

✔️ Inefficient forecasting and inventory allocation, causing fluctuations in demand planning.

This project provides a structured analytical approach to uncover these issues and enables the business to make informed decisions regarding pricing, product strategy, customer engagement, and market expansion.

 ## ii. Snapshots
 ![Snapshot of Sales Analytics Dashboard](https://github.com/Morsshed/1.-Power-BI-E-Commerce-Sales-Analysis/blob/main/Snapshot%20of%20Sales%20Analytics%20Dashboard.jpeg?raw=true)

 ## iii. Dashboard Features

                             Dynamic Features:
                                              1. Tooltips : Matrix in Line Chart
                                              2. Drill Through : Table Chart with Gauge Charts
                                              3. Drill Down : Stacked Bar Charts
                                              4. Filed Parameter :                   
                             Analytical Features:
                                              1. KPI Cards : Total Orders, Total Revenue, Net Profit, Profit Margin and Rate of Return
                                              2. Stacked Bard Chart: Number of Orders by Category and Sub Category
                                              3. Line Charts : Trend and Forecasting, Net Profit Vs Adjusted Profit
                                              4. Table chart : Top 10 Products by Profit, Sales by Continents/Country/Regions
                                              5. Gauge Charts : Tagrgets VS Profit/Order/Revenue
                                              6. Map : Country by Total Revenue
                                              7. Pie Charts : Customers' Demographic Analyses
                                              8. Matrix : Customer Segmentation
                                              
 ## iv. Insights and Recommendation

                 Summary Insights
                        1. The business has achieved 25K total orders within the selected period.
                        2. Total revenue is 25M, showing strong overall sales performance.
                        3. Net profit is 10M, resulting in an excellent 42% profit margin.
                        4. The return rate is 2.2%, indicating efficient operations and product quality.
                        5. The forecasting chart shows a clear upward trend, meaning revenue is expected to grow steadily.
                        6. “Tires and Tubes” is the most ordered product sub-category.
                        7. “Shorts” is the sub-category with the highest return rate.
                        8. “Bib-Shorts” is the top-performing product in terms of revenue and net profit.
                        9. The top 10 products by profit show that cycling gear dominates the highest revenue list.
                       10. There are 18,148 total customers, and 17,416 have made purchases, giving a strong customer purchase rate.
                       11. The average revenue per customer is approximately $1,430.
                       12. Gender distribution is nearly balanced, with slightly more male customers.
                       13. Most customers fall within the 25–34 age group, indicating a young target audience.
                       14. The customer segmentation (RFM) shows a large portion in the “Potential Loyalists” and “Champions” categories.
                       15. Revenue per customer has a declining trend at the start of the month but stabilizes later.
                       16. Single customers represent the majority of buyers, which may influence product preferences.
                       17. North America generates the highest total revenue and profit, with 42.5% profit margin.
                       18. Europe, although significant in sales volume, shows slightly lower return rates compared to NA.
                       19. Revenue is widely diversified across continents, reducing business risk.
                       20. The United States leads all countries in both revenue and total orders, followed by the U.K. and Germany.

                Recommendations for Business Growth
                       1. Focus Marketing on High-Performing Segments. It is recommended to target promotions to “Champions” and “Potential Loyalists” to maximize repeat purchases.
                       2. Improve Product Quality for High-Return Categories. It is necessary to investigate Shorts category (highest return rate) for sizing, material, or expectation mismatch issues.
                       3. Expand Inventory & Promotions for High-Demand Products. The company can increase advertising and stock for Tires and Tubes, Bib-Shorts, and other top-profit items.
                       4. Grow Underperforming Regions With High Potential. Europe and Asia-Pacific show significant order volume—launch region-specific campaigns and pricing adjustments.
                       5. Enhance Customer Lifetime Value. The company can introduce loyalty programs for young customers (25–34 age group), who represent the highest engagement segment.
 ## v. Data Source
 [Adventure Works Dataset (Kaggle)](https://www.kaggle.com/datasets/atulmittal199174/adventure-works-dataset)
# A - Analysis Techniques:
# A1 - Data Preparation (ETL & Load)
# A2 - Data Modelling (Relationship)

                                         Fact Tables:
                                               1. Fact Sales
                                               2. Fact Returns
                                         Dimension Tables:
                                               1. Dim Customer
                                               2. Dim Date
                                               3. Dim Product
                                               4. Dim Category
                                               5. Dim SubCategory
                                               6. Dim Territory

  ## A2.1 - Model

![Data Modelling](https://github.com/Morsshed/1.-Power-BI-E-Commerce-Sales-Analysis/blob/main/Data%20Modelling.png?raw=true) 
  
  ## A2.2 - Cardinality

![Table Cardinality](https://github.com/Morsshed/1.-Power-BI-E-Commerce-Sales-Analysis/blob/main/Table%20Cardinality.png?raw=true) 

  ## A2.3 - Filter Direction
  
![Common Filter Direction](https://github.com/Morsshed/1.-Power-BI-E-Commerce-Sales-Analysis/blob/main/Common%20Filter%20Direction.png?raw=true)

# A3 - DAX

 ## A3.1 - Calculated Tables

   ### A3.1.1 Lookup Date Table
   
       let
          Source = {Number.From(List.Min(FactSales[OrderDate]))..Number.From(List.Max(FactSales[OrderDate]))},
          #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
          #"Changed Type" = Table.TransformColumnTypes(#"Converted to Table",{{"Column1", type date}}),
          #"Renamed Columns" = Table.RenameColumns(#"Changed Type",{{"Column1", "Date"}}),
          #"Inserted Year" = Table.AddColumn(#"Renamed Columns", "Year", each Date.Year([Date]), Int64.Type),
          #"Inserted Quarter" = Table.AddColumn(#"Inserted Year", "Quarter", each Date.QuarterOfYear([Date]), Int64.Type),
          #"Inserted Month" = Table.AddColumn(#"Inserted Quarter", "Month", each Date.Month([Date]), Int64.Type),
          #"Inserted Month Name" = Table.AddColumn(#"Inserted Month", "Month Name", each Date.MonthName([Date]), type text),
          #"Inserted Start of Month" = Table.AddColumn(#"Inserted Month Name", "Start of Month", each Date.StartOfMonth([Date]), type date),
          #"Inserted Week of Year" = Table.AddColumn(#"Inserted Start of Month", "Week of Year", each Date.WeekOfYear([Date]), Int64.Type),
          #"Inserted Week of Month" = Table.AddColumn(#"Inserted Week of Year", "Week of Month", each Date.WeekOfMonth([Date]), Int64.Type),
          #"Inserted Day Name" = Table.AddColumn(#"Inserted Week of Month", "Day Name", each Date.DayOfWeekName([Date]), type text),
          #"Added Conditional Column" = Table.AddColumn(#"Inserted Day Name", "Day Type", each if [Day Name] = "Saturday" then "Weekend" else if [Day Name] = "Sunday" then "Weekend" else "Weekday"),
          #"Changed Type1" = Table.TransformColumnTypes(#"Added Conditional Column",{{"Day Type", type text}}),
          #"Extracted First Characters" = Table.TransformColumns(#"Changed Type1", {{"Month Name", each Text.Start(_, 3), type text}}),
          #"Added Prefix" = Table.TransformColumns(#"Extracted First Characters", {{"Quarter", each "Q" & Text.From(_, "en-US"), type text}}),
          #"Inserted Merged Column" = Table.AddColumn(#"Added Prefix", "Year-Quarter", each Text.Combine({Text.From([Year], "en-US"), [Quarter]}, "-"), type text),
          #"Reordered Columns" = Table.ReorderColumns(#"Inserted Merged Column",{"Date", "Year", "Quarter", "Year-Quarter", "Month", "Month Name", "Start of Month", "Week of Year", "Week of Month", "Day Name", "Day Type"}),
          #"Inserted Last Characters" = Table.AddColumn(#"Reordered Columns", "Last Characters", each Text.End(Text.From([Year], "en-US"), 2), type text),
          #"Renamed Columns1" = Table.RenameColumns(#"Inserted Last Characters",{{"Last Characters", "Short Year"}}),
          #"Inserted Merged Column1" = Table.AddColumn(#"Renamed Columns1", "Mon-Year", each Text.Combine({[Month Name], [Short Year]}, "-"), type text),
          #"Reordered Columns1" = Table.ReorderColumns(#"Inserted Merged Column1",{"Date", "Start of Month", "Year", "Quarter", "Year-Quarter", "Month", "Month Name", "Week of Year", "Week of Month", "Day Name", "Day Type", "Short Year", "Mon-Year"}),
          #"Renamed Columns2" = Table.RenameColumns(#"Reordered Columns1",{{"Start of Month", "Start of the Month"}}),
          #"Inserted Day of Week" = Table.AddColumn(#"Renamed Columns2", "Day of Week", each Date.DayOfWeek([Date]), Int64.Type),
          #"Removed Columns" = Table.RemoveColumns(#"Inserted Day of Week",{"Day of Week"})
      in
          #"Removed Columns"
 
 ## A3.2 - Calculated Columns

                                  Age Group = 
                                    SWITCH(
                                        TRUE(),
                                        DimCustomer[Age] <= 16, "0-16y",
                                        DimCustomer[Age] <= 30, "17-30y",
                                        DimCustomer[Age] <= 60, "31-60y",
                                        "61y+"
                                    )

                                  Priority Customers = 
                                     IF(
                                       DimCustomer[AnnualIncome] >= 100000 && DimCustomer[Is Parent?] = "Yes",
                                         "Priority", "Standard"
                                     ) 

                                  Price Group = 
                                      SWITCH(
                                          TRUE(),
                                          DimProduct[ProductPrice] >= 500, "Premium",
                                          DimProduct[ProductPrice] >= 150, "Mid-Range",
                                          "Low"
                                      )

                                  Retail Price = RELATED(DimProduct[ProductPrice])
                                  Sale Value = FactSales[OrderQuantity] * FactSales[Retail Price]

 ## A3.3 - Calculated Measures (KPI Measures)
 
  #### Sales & Profits
  
                                   Total Sales = SUM(FactSales[Sale Value])
                                   Total Revenue = SUMX(FactSales,FactSales[OrderQuantity]*RELATED(DimProduct[ProductPrice]))
                                   Total Cost = SUMX(FactSales,FactSales[OrderQuantity]*RELATED(DimProduct[ProductCost]))
                                   Net Profit = [Total Revenue]-[Total Cost]
                                   Profit Margin = DIVIDE([Net Profit], [Total Revenue], BLANK())
                                   Profit Target = [PM Profit]*1.1
                                   Profit Target Gap = [Net Profit]-[Profit Target]
                                   Revenue Target = [PM Revenue]*1.1
                                   Revenue Target Gap = [Total Revenue]-[Revenue Target]

   #### Orders
   
                                    Number of Orders = DISTINCTCOUNT(FactSales[OrderNumber])
                                    AOV = DIVIDE([Total Revenue], [# of Orders], BLANK())
                                    AVG Basket Size = DIVIDE([Quantity Ordered], [# of Orders], BLANK())
                                    Order Target = [PM Order]*1.1
                                    Order Target Gap = [# of Orders]- [Order Target]
                                    Price Per Item = DIVIDE([Total Revenue],[Quantity Ordered], BLANK())
                                    Quantity Ordered = sum(FactSales[OrderQuantity])

   #### Customers 

                                    Number of Customers who Purchased = DISTINCTCOUNT(FactSales[CustomerKey])
                                    Revenue per Customer = DIVIDE([Total Revenue], [# of Customers who Purchased], BLANK())
                                    Total Customers = COUNTROWS(DimCustomer)
  
   #### Returns 

                                    Quantity Returned = SUM(FactReturns[ReturnQuantity])
                                    Return Rate = DIVIDE([Quantity Returned],[Quantity Ordered], BLANK())
                                    Total Returns = COUNTROWS(FactReturns)

   #### Adjusted Pricer (5%)

                                    Adjusted Price = [AVG Retail Price]* (1+'Price Adjustment (%)'[Price Adjustment (%) Value])
                                    Adjusted Profit = [Adjusted Revenue]-[Total Cost]
                                    Adjusted Revenue = SUMX(FactSales, FactSales[OrderQuantity]*[Adjusted Price])
                                    AVG Retail Price = AVERAGE(DimProduct[ProductPrice])

   #### Time Intelligence 

                                   Order YTD = CALCULATE([# of Orders], DATESYTD(DimDate[Date]))
                                   PM Profit = CALCULATE([Net Profit], DATEADD(DimDate[Date],-1,MONTH))
                                   PM Quantity Returned = CALCULATE([Quantity Returned], DATEADD(DimDate[Date],-1,MONTH))
                                   PM Revenue = CALCULATE([Total Revenue], DATEADD(DimDate[Date],-1,MONTH) )
                                   PM Order = CALCULATE([# of Orders], DATEADD(DimDate[Date],-1,MONTH))

# B - Analyses and Interactivities:
  ## B1 - KPI and Trend Analysis
  ![KPI and Trend Analysis](https://github.com/Morsshed/1.-Power-BI-E-Commerce-Sales-Analysis/blob/main/KPI%20and%20Trend%20Analysis.png?raw=true) 
  ## B2 - Territory Analysis
  ![Territory Analysis](https://github.com/Morsshed/1.-Power-BI-E-Commerce-Sales-Analysis/blob/main/Territory%20Analysis.png?raw=true)
  ## B3 - Customer Analysis
  ![Customer Analysis](https://github.com/Morsshed/1.-Power-BI-E-Commerce-Sales-Analysis/blob/main/Customer%20Analysis.png?raw=true)
  ## B4 - Product Analysis
  ![Product Analysis](https://github.com/Morsshed/1.-Power-BI-E-Commerce-Sales-Analysis/blob/main/Product%20Analysis.png?raw=true)




















