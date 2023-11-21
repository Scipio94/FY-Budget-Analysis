# Grant Budget Analysis SQL Syntax

~~~ SQL
-- GRANT BUDGET METRICS
/*FY 18 Grant Budget Data */
SELECT 
  DISTINCT FY,
  Category,
  SUM(Expenditures) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_18_Grant_Budget_Data`;

/*FY 19 Grant Budget Data*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Expenditures) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_19_Grant_Budget_Data`;

/*FY 20 Grant Budget Data*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Expenditures) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_20_Grant_Budget_Data`;

/*FY 21 Grant Budget Data*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Expenditures) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_21_Grant_Budget_Data`;


/*FY 22 Grant Budget Data*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Expenditures) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_22_Grant_Budget_Data`;

/*FY 23 Grant Budget Data*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Expenditures) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_23_Grant_Budget_Data`;

-- UNION ALL
-- Creating Temp Table
CREATE TEMP TABLE t1 AS
/*FY 18 Grant Budget Data*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Expenditures) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_18_Grant_Budget_Data`
UNION ALL
/*FY 19 Grant Budget Data*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Expenditures) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_19_Grant_Budget_Data`
UNION ALL
/*FY 20 Grant Budget Data*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Expenditures) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_20_Grant_Budget_Data`
UNION ALL
/*FY 21 Grant Budget Data*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Expenditures) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_21_Grant_Budget_Data`
UNION ALL
/*FY 22 Grant Budget Data*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Expenditures) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_22_Grant_Budget_Data`
UNION ALL
/*FY 23 Grant Budget Data*/
SELECT 
  DISTINCT FY,
  Category,
  SUM(Expenditures) OVER (PARTITION BY Category) AS Total_Spending
FROM `single-being-353600.FY_25_Budget_Analysis.FY_23_Grant_Budget_Data`;

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
WHERE Category = 'Instructional'
ORDER BY t1.FY) AS sub
ORDER BY sub.FY;

/*YOY Grant Budget Data Metrics Support Services*/ 
SELECT
  sub.FY,
  sub.Category,
  sub.Total_Spending,
  sub.YoY_Change,
  -- COALESCE(ROUND(sub.YoY_Change/(LAG(sub.Total_Spending,1) OVER (ORDER BY sub.FY)),2),0) AS YoY_Change_Pct 
FROM 
(SELECT 
  t1.FY,
  t1.Category,
  t1.Total_Spending,
  COALESCE(ROUND(t1.Total_Spending - LAG(t1.Total_Spending,1) OVER (ORDER BY t1.FY),2),0) AS YoY_Change,
FROM t1
WHERE Category = 'Support'
ORDER BY t1.FY) AS sub
ORDER BY sub.FY;
~~~
