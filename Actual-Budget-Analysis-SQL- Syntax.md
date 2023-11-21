# Actual Budget Analysis SQL Syntax

~~~ SQL
-- ACTUAL METRICS
/*FY 18 Actuals*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Actuals) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_18_Actual_Budget_Data`;

/*FY 19 Actuals*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Actuals) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_19_Actual_Budget_Data`;

/*FY 20 Actuals*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Actuals) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_20_Actual_Budget_Data`;

/*FY 21 Actuals*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Actuals) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_21_Actual_Budget_Data`;


/*FY 22 Actuals*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Actuals) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_22_Actual_Budget_Data`;

/*FY 23 Actuals*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Actuals) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_23_Actual_Budget_Data`;

-- UNION ALL
-- Creating Temp Table
CREATE TEMP TABLE t1 AS
/*FY 18 Actuals*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Actuals) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_18_Actual_Budget_Data`
UNION ALL
/*FY 19 Actuals*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Actuals) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_19_Actual_Budget_Data`
UNION ALL
/*FY 20 Actuals*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Actuals) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_20_Actual_Budget_Data`
UNION ALL
/*FY 21 Actuals*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Actuals) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_21_Actual_Budget_Data`
UNION ALL
/*FY 22 Actuals*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Actuals) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_22_Actual_Budget_Data`
UNION ALL
/*FY 23 Actuals*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Actuals) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_23_Actual_Budget_Data`;

/*YOY Actuals Metrics Instruction*/ 
SELECT
  sub.FY,
  sub.Category,
  sub.Total_Spending,
  sub.YoY_Change,
  COALESCE(ROUND(sub.YoY_Change/(LAG(sub.Total_Spending,1) OVER (ORDER BY sub.FY)),2),0) AS YoY_Change_Pct 
FROM 
(SELECT 
  t1.FY,
  t1.Category,
  t1.Total_Spending,
  COALESCE(ROUND(t1.Total_Spending - LAG(t1.Total_Spending,1) OVER (ORDER BY t1.FY),2),0) AS YoY_Change,
FROM t1
WHERE Category = 'Instruction'
ORDER BY t1.FY) AS sub
ORDER BY sub.FY;


/*YOY Actuals Metrics Administrative*/ 
SELECT
  sub.FY,
  sub.Category,
  sub.Total_Spending,
  sub.YoY_Change,
  COALESCE(ROUND(sub.YoY_Change/(LAG(sub.Total_Spending,1) OVER (ORDER BY sub.FY)),2),0) AS YoY_Change_Pct 
FROM 
(SELECT 
  t1.FY,
  t1.Category,
  t1.Total_Spending,
  COALESCE(ROUND(t1.Total_Spending - LAG(t1.Total_Spending,1) OVER (ORDER BY t1.FY),2),0) AS YoY_Change,
FROM t1
WHERE Category = 'Administrative'
ORDER BY t1.FY) AS sub
ORDER BY sub.FY;


/*YoY Actuals Metrics Support Services*/ 
SELECT
  sub.FY,
  sub.Category,
  sub.Total_Spending,
  sub.YoY_Change,
  COALESCE(ROUND(sub.YoY_Change/(LAG(sub.Total_Spending,1) OVER (ORDER BY sub.FY)),2),0) AS YoY_Change_Pct 
FROM 
(SELECT 
  t1.FY,
  t1.Category,
  t1.Total_Spending,
  COALESCE(ROUND(t1.Total_Spending - LAG(t1.Total_Spending,1) OVER (ORDER BY t1.FY),2),0) AS YoY_Change,
FROM t1
WHERE Category = 'Support Services'
ORDER BY t1.FY) AS sub
ORDER BY sub.FY;
~~~
