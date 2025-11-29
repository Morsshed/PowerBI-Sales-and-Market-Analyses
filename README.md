# 1. Power-BI: E-Commerce-Sales-Analysis
This project includes Sales Analysis &amp; Forecasting, Territory Analysis, Customer Analysis and Product Analysis

# Analysis Details
 ## i. Business Case
 ## ii. Snapshots
 ## iii. Dashboard Features
 ## iv. Insights and Recommendation
 ## v. Data Source
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
  ## B3 - Customer Analysis
  ![Customer Analysis](https://github.com/Morsshed/1.-Power-BI-E-Commerce-Sales-Analysis/blob/main/Customer%20Analysis.png?raw=true)
  ## B4 - Product Analysis



















