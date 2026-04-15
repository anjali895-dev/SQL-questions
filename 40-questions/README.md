## Q.1 Label each order as 'High Value' if TotalOrderAmount > 500, else 'Normal'. Show OrderID, TotalOrderAmount, and the label.
```sql
SELECT
	OrderID,
	TotalOrderAmount,
CASE
	WHEN TotalOrderAmount >= 500 THEN 'High Value'
	ELSE 'Normal'
END Value
FROM Fact_Orders;
```

## Q.2 Show each product with a 'Price Band' column: 'Budget' if UnitPrice < 50, 'Mid' if between 50–80, 'Premium' if above 80. Use Fact_OrderItems.
```sql
SELECT
	ProductID,
	UnitPrice,
CASE
	WHEN UnitPrice <= 50 THEN 'Budget'
	WHEN UnitPrice BETWEEN 50 AND 80 THEN 'Mid'
	ELSE 'Premium'
END PriceBand
FROM Fact_OrderItems;
```

## Q.3 From Dim_Customers, show a 'Status' column: 'Active' if IsActive = 1, else 'Inactive'.
```sql
SELECT
	CustomerName,
CASE
	WHEN IsActive = 1 THEN 'Active'
	ELSE 'Inactive'
END Status
FROM Dim_Customers;
```

## Q.4 From Fact_Payments, label PaymentStatus as 'OK' for 'Success', 'Pending' as 'Waiting', and everything else as 'Issue'.
```sql
SELECT
	OrderID,
CASE 
	WHEN PaymentStatus = 'Success' THEN 'OK'
	WHEN PaymentStatus = 'Pending' THEN 'Waiting'
	ELSE 'Issue'
END PaymentStatus
FROM Fact_Payments;
```
## Q.5 Count the number of orders in each status category: 'Completed' (Delivered), 'In Progress' (Shipped/Processing), and 'Cancelled'. Use Fact_Orders.
```sql
SELECT
	SUM(CASE WHEN OrderStatus = 'Delivered' THEN 1 ELSE 0 END) AS Completed,
	SUM(CASE WHEN OrderStatus = 'Pending' THEN 1 ELSE 0 END) AS InProcessing,
	SUM(CASE WHEN OrderStatus = 'Cancelled' THEN 1 ELSE 0 END) AS Cancelled
FROM Fact_Orders;
```

## Q.6 From Fact_OrderItems, calculate total revenue split by Price Band (Budget / Mid / Premium) using SUM with CASE WHEN.
```sql
SELECT 
	SUM(CASE WHEN UnitPrice < 50 THEN TotalAmount ELSE 0 END) AS BudgetRevenue,
	SUM(CASE WHEN UnitPrice BETWEEN 50 AND 80 THEN TotalAmount ELSE 0 END) AS MidRevenue,
    SUM(CASE WHEN UnitPrice > 80 THEN TotalAmount ELSE 0 END) AS PremiumRevenue
FROM Fact_OrderItems;
```

## Q.7 Join Fact_Orders with Dim_Customers. Add a 'DiscountTier' column: 'No Discount' if DiscountAmount = 0, 'Low' if 1–60, 'High' if above 60.
```sql
SELECT
	c.CustomerName,
    o.OrderID,
    o.DiscountAmount,
CASE
    WHEN o.DiscountAmount = 0 THEN 'No Discount'
    WHEN o.DiscountAmount BETWEEN 1 AND 60 THEN 'Low'
    ELSE 'High'
END AS DiscountTier
FROM Fact_Orders o
JOIN Dim_Customers c ON o.CustomerID = c.CustomerID;
```

## Q.8 From Fact_InventoryMovement, show each movement with a 'Direction' label: '+' for MovementType = 'IN', '-' for 'OUT'. Also show signed Quantity (positive or negative).
```sql
SELECT
	MovementID,
    ProductID,
    MovementType,
CASE WHEN MovementType = 'IN' THEN '+' ELSE '-' END AS Direction,
CASE WHEN MovementType = 'IN' THEN Quantity ELSE - Quantity END AS SignedQty
FROM Fact_InventoryMovement;
```

## Q.9 Per CustomerSegment, show count of Active vs Inactive customers side by side as columns. Join Dim_Customers.
```sql
SELECT
	CustomerSegment,
	SUM(CASE WHEN IsActive = 1 THEN 1 ELSE 0 END) AS ActiveCount,
	SUM(CASE WHEN IsActive = 0 THEN 1 ELSE 0 END) AS InactiveCount
FROM Dim_Customers
GROUP BY CustomerSegment
ORDER BY CustomerSegment;
```

## Q.10 For each product, show total quantity sold, and split it into Weekend vs Weekday sales. Join Fact_OrderItems, Fact_Orders, Dim_Date, and Dim_Products.
```sql
SELECT 
	p.ProductName,
	SUM(oi.Quantity) AS TotalQty,
    SUM(CASE WHEN d.IsWeekend = 1 THEN oi.Quantity ELSE 0 END) AS WeekendQty,
    SUM(CASE WHEN d.IsWeekend = 0 THEN oi.Quantity ELSE 0 END) AS WeekdayQty
FROM Fact_OrderItems oi
JOIN Fact_Orders o ON oi.OrderID = o.OrderID
JOIN Dim_Products p ON oi.ProductID = p.ProductID
JOIN Dim_Date d ON o.OrderDate = d.DateKey
GROUP BY p.ProductName
ORDER BY TotalQty DESC;
```

## Q.11 Rank all orders by TotalOrderAmount from highest to lowest using RANK(). Show OrderID and rank.
```sql
SELECT
	OrderID,
	TotalOrderAmount,
	RANK() OVER(ORDER BY TotalOrderAmount DESC) AS RevenueRank
FROM Fact_Orders;
```

## Q.12 Add a running total of PaidAmount ordered by PaymentDate in Fact_Payments using SUM() as a window function.
```sql
SELECT
	PaymentID,
	PaymentDate,
	PaidAmount,
	SUM(PaidAmount) OVER(ORDER BY PaidAmount DESC) AS TotalPaidAmount
FROM Fact_Payments
ORDER BY PaymentDate;
```

## Q.13 For each row in Fact_OrderItems, show the average UnitPrice across all rows as a column alongside each row's own UnitPrice.
```sql
SELECT
	OrderId,
	ProductID,
	UnitPrice,
	CAST (AVG(UnitPrice) OVER()AS Decimal(10,2))AS AvgUnitPrice
FROM Fact_OrderItems;
```

## Q.14 Rank customers within each CustomerSegment by their SignupDate (earliest = rank 1). Use Dim_Customers.
```sql
SELECT 
	CustomerName,
	CustomerSegment,
	SignupDate,
	RANK() OVER(PARTITION BY CustomerSegment ORDER BY SignupDate ASC)AS SignupRank
FROM Dim_Customers;
```

## Q.15 For each order in Fact_Orders, show the previous order's TotalOrderAmount for the same CustomerID using LAG().
```sql
SELECT
	OrderId,
	CustomerID,
	OrderStatus,
	TotalOrderAmount,
	LAG(TotalOrderAmount) OVER(PARTITION BY CustomerID ORDER BY OrderDate) AS PrevOrderAmount
FROM Fact_Orders;
```

## Q.16 Show a running count of orders placed per CustomerID over time using COUNT() as a window function.
```sql
SELECT
	OrderID,
	CustomerID,
	OrderDate,
	COUNT(OrderID) OVER(PARTITION BY CustomerID ORDER BY OrderDate) AS OrderNumber
FROM Fact_Orders
ORDER BY CustomerID, OrderDate;
```

## Q.17 For each product in Fact_OrderItems, show each row's TotalAmount and the maximum TotalAmount for that ProductID as a separate column.
```sql
SELECT
	OrderItemID,
	ProductID,
	TotalAmount,
	MAX(TotalAmount) OVER (PARTITION BY ProductID) AS MaxAmountForProduct
FROM Fact_OrderItems;
```

## Q.18 Using DENSE_RANK(), rank products by total quantity sold. Join Fact_OrderItems with Dim_Products.
```sql
SELECT
	p.ProductName,
	SUM(oi.Quantity) AS TotalQty,
	DENSE_RANK() OVER (ORDER BY SUM(oi.Quantity) DESC) AS QtyRank
FROM Fact_OrderItems oi
JOIN Dim_Products p ON oi.ProductID = p.ProductID
GROUP BY p.ProductName
ORDER BY QtyRank;
```

## Q.19 For each customer, find their order that was their single biggest purchase. Use ROW_NUMBER() partitioned by CustomerID to get rank 1.
```sql
SELECT CustomerID, OrderID, OrderDate, TotalOrderAmount
FROM (
  SELECT
    CustomerID, OrderID, OrderDate, TotalOrderAmount,
    ROW_NUMBER() OVER (PARTITION BY CustomerID ORDER BY TotalOrderAmount DESC) AS rn
  FROM Fact_Orders
) ranked
WHERE rn = 1
ORDER BY TotalOrderAmount DESC;
```

## Q.20 Calculate month-over-month revenue growth using LAG(). Show Year, Month, Revenue, previous month revenue, and growth %.
```sql
SELECT
	Year, Month, Revenue,
	LAG(Revenue) OVER (ORDER BY Year, Month) AS PrevRevenue,
	ROUND(
    100.0 * (Revenue - LAG(Revenue) OVER (ORDER BY Year, Month))
    / NULLIF(LAG(Revenue) OVER (ORDER BY Year, Month), 0), 2
	) AS GrowthPct
FROM (
  SELECT
    YEAR(OrderDate) AS Year,
    MONTH(OrderDate) AS Month,
    SUM(TotalOrderAmount) AS Revenue
  FROM Fact_Orders
  GROUP BY YEAR(OrderDate), MONTH(OrderDate)
) monthly
ORDER BY Year, Month;
```

## Q.21 Write a CTE called 'ActiveCustomers' that selects only active customers from Dim_Customers, then query it to count how many exist.
```sql
WITH ActiveCustomers AS (
	SELECT *
	FROM Dim_Customers
	WHERE IsActive = 1
)
SELECT COUNT(*) AS ActiveCount
FROM ActiveCustomers;
```

## Q.22 Create a CTE 'HighValueOrders' for orders above 50 in Fact_Orders. Then list the CustomerIDs who made these orders.
```sql
WITH HighValueOrders AS (
	SELECT *
	FROM Fact_Orders
	WHERE TotalOrderAmount >50
)
SELECT DISTINCT CustomerID
FROM HighValueOrders
ORDER BY CustomerID;
```

## Q.23 Use a CTE to first get all 'Electronics' products from Dim_Products, then count how many sub-categories exist within Electronics.
```sql
WITH ElectronicsProducts AS (
	SELECT *
	FROM Dim_Products
	WHERE Category = 'Electronics'
)
SELECT COUNT(DISTINCT SubCategory) AS SubCategoryCount
FROM ElectronicsProducts;
```

## Q.24 Use a CTE to calculate total spend per CustomerID from Fact_Orders. Then join the CTE with Dim_Customers to show CustomerName and TotalSpend.
```sql
WITH CustomerSpend AS (
  SELECT CustomerID, SUM(TotalOrderAmount) AS TotalSpend
  FROM Fact_Orders
  GROUP BY CustomerID
)
SELECT c.CustomerName, cs.TotalSpend
FROM CustomerSpend cs
JOIN Dim_Customers c ON cs.CustomerID = c.CustomerID
ORDER BY cs.TotalSpend DESC;
```

## Q.25 Create a CTE 'OrderSummary' that computes total orders and total revenue per LocationID. Then join with Dim_Location to show Zone-level summary.
```sql
WITH OrderSummary AS (
  SELECT
    LocationID,
    COUNT(OrderID) AS TotalOrders,
    SUM(TotalOrderAmount) AS TotalRevenue
  FROM Fact_Orders
  GROUP BY LocationID
)
SELECT l.Zone, SUM(os.TotalOrders) AS Orders, SUM(os.TotalRevenue) AS Revenue
FROM OrderSummary os
JOIN Dim_Location l ON os.LocationID = l.LocationID
GROUP BY l.Zone
ORDER BY Revenue DESC;
```

## Q.26 Use two CTEs: first 'ProductRevenue' (total revenue per ProductID from Fact_OrderItems), then 'TopProducts' (top 5 by revenue). Show ProductName using Dim_Products.
```sql
WITH ProductRevenue AS (
  SELECT ProductID, SUM(TotalAmount) AS Revenue
  FROM Fact_OrderItems
  GROUP BY ProductID
),
TopProducts AS (
  SELECT TOP 5 
  ProductID, Revenue
  FROM ProductRevenue
  ORDER BY Revenue DESC
)
SELECT p.ProductName, tp.Revenue
FROM TopProducts tp
JOIN Dim_Products p ON tp.ProductID = p.ProductID
ORDER BY tp.Revenue DESC;
```

## Q.27 Write a CTE 'CartedNotOrdered' that finds CustomerIDs who added items to cart (Fact_CartEvents, EventType='AddToCart') but have no record in Fact_Orders.
```sql
WITH CartedNotOrdered AS (
	SELECT DISTINCT CustomerID
	FROM Fact_CartEvents
	WHERE EventType = 'AddToCart'
)
SELECT cc.CustomerID
FROM CartedNotOrdered cc
LEFT JOIN Fact_Orders fo ON cc.CustomerID = fo.CustomerID
WHERE fo.OrderID IS NULL;
```

## Q.28 Use a CTE to find the average order value per CustomerSegment. Then classify segments as 'Above Average' or 'Below Average' compared to the overall average.
```sql
WITH SegmentAvg AS (
  SELECT
    c.CustomerSegment,
    AVG(o.TotalOrderAmount) AS AvgOrderVal
  FROM Fact_Orders o
  JOIN Dim_Customers c ON o.CustomerID = c.CustomerID
  GROUP BY c.CustomerSegment
),
OverallAvg AS (
  SELECT AVG(TotalOrderAmount) AS GlobalAvg FROM Fact_Orders
)
SELECT
  sa.CustomerSegment,
  ROUND(sa.AvgOrderVal, 2) AS SegmentAvg,
  CASE
    WHEN sa.AvgOrderVal > oa.GlobalAvg THEN 'Above Average'
    ELSE 'Below Average'
  END AS Comparison
FROM SegmentAvg sa
CROSS JOIN OverallAvg oa
ORDER BY sa.AvgOrderVal DESC;
```

## Q.29 Use a CTE to compute monthly revenue. Then use a second CTE to calculate the 3-month moving average of revenue.
```sql
WITH MonthlyRevenue AS (
  SELECT
    YEAR(OrderDate) AS Year,
    MONTH(OrderDate) AS Month,
    SUM(TotalOrderAmount) AS Revenue
  FROM Fact_Orders
  GROUP BY YEAR(OrderDate), MONTH(OrderDate)
),
MovingAvg AS (
  SELECT
    Year, Month, Revenue,
    AVG(Revenue) OVER (
      ORDER BY Year, Month
      ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS MovingAvg3M
  FROM MonthlyRevenue
)
SELECT * FROM MovingAvg ORDER BY Year, Month;
```

## Q.30 Using a CTE, identify customers who placed orders in both Q1 and Q3 of the same year. Show CustomerID and the year.
```sql
WITH Q1Orders AS (
  SELECT DISTINCT CustomerID, YEAR(OrderDate) AS Year
  FROM Fact_Orders
  WHERE MONTH(OrderDate) IN (1,2,3)
),
Q3Orders AS (
  SELECT DISTINCT CustomerID, YEAR(OrderDate) AS Year
  FROM Fact_Orders
  WHERE MONTH(OrderDate) IN (7,8,9)
)
SELECT q1.CustomerID, q1.Year
FROM Q1Orders q1
INNER JOIN Q3Orders q3 ON q1.CustomerID = q3.CustomerID AND q1.Year = q3.Year
ORDER BY q1.Year, q1.CustomerID;
```

## Q.31 Find all orders where TotalOrderAmount is greater than the average TotalOrderAmount across all orders.
```sql
SELECT *
FROM Fact_Orders
WHERE TotalOrderAmount > (
	SELECT AVG(TotalOrderAmount)
	FROM Fact_Orders
)
ORDER BY TotalOrderAmount DESC;
```

## Q.32 Find customers who have placed at least one order. Use a subquery with IN.
```sql
SELECT *
FROM Dim_Customers
WHERE CustomerID IN (
	SELECT DISTINCT CustomerID
	FROM Fact_Orders
)
ORDER BY CustomerName;
```

## Q.33 Find products that have never been ordered. Use a subquery with NOT IN.
```sql
SELECT 
	ProductName,
	Category
FROM Dim_Products
WHERE ProductID NOT IN (
	SELECT DISTINCT ProductID
	FROM Fact_OrderItems
)
ORDER BY ProductName;
```

## Q.34 Show the most expensive single order item (highest UnitPrice) from Fact_OrderItems using a subquery.
```sql
SELECT *
FROM Fact_OrderItems
WHERE UnitPrice = (
	SELECT MAX(UnitPrice)
	FROM Fact_OrderItems
);
```

## Q.35 List customers whose total spend is above the average total spend per customer. Use a subquery in the HAVING clause.
```sql
SELECT 
	CustomerID,
	SUM(TotalOrderAmount) AS TotalSpend
FROM Fact_Orders
GROUP BY CustomerID
HAVING SUM(TotalOrderAmount) > (
	SELECT AVG(CustomerTotal)
	FROM(
		SELECT
			CustomerID, 
			SUM(TotalOrderAmount) AS CustomerTotal
		FROM Fact_Orders
		GROUP BY CustomerID
	) avg_sub
)
ORDER BY TotalSpend DESC;
```

## Q.36 For each order, show whether the TotalOrderAmount is above or below that customer's own average order amount. Use a correlated subquery.
```sql
SELECT
  OrderID,
  CustomerID,
  TotalOrderAmount,
  (
    SELECT CAST(AVG(TotalOrderAmount)AS Decimal(10,2))
    FROM Fact_Orders inner_o
    WHERE inner_o.CustomerID = outer_o.CustomerID
  ) AS CustomerAvg,
  CASE
    WHEN TotalOrderAmount > (
      SELECT AVG(TotalOrderAmount)
      FROM Fact_Orders inner_o
      WHERE inner_o.CustomerID = outer_o.CustomerID
    ) THEN 'Above'
    ELSE 'Below'
  END AS VsAvg
FROM Fact_Orders outer_o
ORDER BY CustomerID, OrderDate;
```

## Q.37 Show product names and their total revenue, but only for products whose total revenue exceeds the average product revenue. Use a subquery in WHERE.
```sql
SELECT 
	ProductID, 
	TotalRevenue
FROM (
  SELECT ProductID, SUM(TotalAmount) AS TotalRevenue
  FROM Fact_OrderItems
  GROUP BY ProductID
) product_rev
WHERE TotalRevenue > (
  SELECT AVG(TotalRevenue)
  FROM (
    SELECT ProductID, SUM(TotalAmount) AS TotalRevenue
    FROM Fact_OrderItems
    GROUP BY ProductID
  ) avg_sub
)
ORDER BY TotalRevenue DESC;
```

## Q.38 Find the second highest TotalOrderAmount from Fact_Orders using a subquery.
```sql
SELECT
	MAX(TotalOrderAmount) AS SecondHighest
FROM Fact_Orders
WHERE TotalOrderAmount < (
	SELECT MAX(TotalOrderAmount)
	FROM Fact_Orders
);
```

## Q.39 Using EXISTS, find customers who have made at least one payment with PaymentStatus = 'Failed'.
```sql
SELECT 
	c.CustomerName, 
	c.CustomerSegment
FROM Dim_Customers c
WHERE EXISTS (
  SELECT 1
  FROM Fact_Orders o
  JOIN Fact_Payments p ON o.OrderID = p.OrderID
  WHERE o.CustomerID = c.CustomerID
    AND p.PaymentStatus = 'Failed'
)
ORDER BY c.CustomerName;
```

## Q.40 For each Zone, show the top-earning product. Use a subquery to rank products per zone, then filter for rank 1.
```sql
SELECT
	Zone,
	ProductID, 
	ZoneRevenue
FROM (
  SELECT
    l.Zone,
    oi.ProductID,
    SUM(oi.TotalAmount) AS ZoneRevenue,
    ROW_NUMBER() OVER (
      PARTITION BY l.Zone
      ORDER BY SUM(oi.TotalAmount) DESC
    ) AS rn
  FROM Fact_OrderItems oi
  JOIN Fact_Orders o ON oi.OrderID = o.OrderID
  JOIN Dim_Location l ON o.LocationID = l.LocationID
  GROUP BY l.Zone, oi.ProductID
) ranked
WHERE rn = 1
ORDER BY Zone;
```
