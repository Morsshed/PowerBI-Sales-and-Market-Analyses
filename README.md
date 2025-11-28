# 1. Power-BI: E-Commerce-Sales-Analysis
This project includes Sales Analysis &amp; Forecasting, Territory Analysis, Customer Analysis and Product Analysis

# Analysis Details
# A - Analysis Techniques:
# A1 - Data Preparation
# A2 - Data Modelling
# A3 - DAX
 # A3.1 - Calculated Tables
 # A3.2 - Calculated Columns
 # A3.3 - Calculated Measures (KPI Measures)
 
  ### Sales & Profits
  
                                   Total Sales = SUM(FactSales[Sale Value])
                                   Total Revenue = SUMX(FactSales,FactSales[OrderQuantity]*RELATED(DimProduct[ProductPrice]))
                                   Total Cost = SUMX(FactSales,FactSales[OrderQuantity]*RELATED(DimProduct[ProductCost]))
                                   Net Profit = [Total Revenue]-[Total Cost]
                                   Profit Margin = DIVIDE([Net Profit], [Total Revenue], BLANK())
                                   Profit Target = [PM Profit]*1.1
                                   Profit Target Gap = [Net Profit]-[Profit Target]
                                   Revenue Target = [PM Revenue]*1.1
                                   Revenue Target Gap = [Total Revenue]-[Revenue Target]

   ### Orders
   
                                    Number of Orders = DISTINCTCOUNT(FactSales[OrderNumber])
                                    AOV = DIVIDE([Total Revenue], [# of Orders], BLANK())
                                    AVG Basket Size = DIVIDE([Quantity Ordered], [# of Orders], BLANK())
                                    Order Target = [PM Order]*1.1
                                    Order Target Gap = [# of Orders]- [Order Target]
                                    PM Order = CALCULATE([# of Orders], DATEADD(DimDate[Date],-1,MONTH))
                                    Price Per Item = DIVIDE([Total Revenue],[Quantity Ordered], BLANK())
                                    Quantity Ordered = sum(FactSales[OrderQuantity])
  


