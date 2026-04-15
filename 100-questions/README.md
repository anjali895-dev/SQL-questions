# 100 SQL Questions

```sql
USE FK_Groceries_Advanced;
```

```sql
--================================================================
--Q.1 List all customer names and their cities from Dim_Customers.
--================================================================
/*SELECT 
	CustomerName, City
FROM Dim_Customers;*/
```

--================================================================
--Q.2 Retrieve all columns from Dim_Products.
--================================================================
/*SELECT *
FROM Dim_Products;*/

--================================================================
--Q.3 Show ProductName and Category from Dim_Products.
--================================================================
/*SELECT 
	ProductName, Category
FROM Dim_Products;*/

--================================================================
--Q.4 Display all unique CustomerSegment values from Dim_Customers.
--================================================================
/*SELECT
	DISTINCT CustomerSegment
FROM Dim_Customers;*/

--===================================================================
--Q.5 Show OrderID, OrderDate, and TotalOrderAmount from Fact_Orders.
--===================================================================
/*SELECT 
	OrderId, OrderDate, TotalOrderAmount
FROM Fact_Orders;*/

--================================================================
--Q.6 List all PaymentMethod values from Fact_Payments.
--================================================================
/*SELECT 
	PaymentMethod
FROM Fact_Payments;*/

--================================================================
--Q.7 Show all columns from Fact_DeliveryLogs.
--================================================================
/*SELECT *
FROM Fact_DeliveryLogs;*/

--================================================================
--Q.8 Retrieve ProductID and Quantity from Fact_OrderItems.
--================================================================
/*SELECT 
	ProductID, Quantity
FROM Fact_OrderItems;*/

--================================================================
--Q.9 Show all distinct EventType values from Fact_CartEvents.
--================================================================
/*SELECT 
	DISTINCT EventType
FROM Fact_CartEvents;*/

--================================================================
--Q.10 Display all Zone and Pincode values from Dim_Location.
--================================================================
/*SELECT 
	Zone, Pincode
FROM Dim_Location;*/

--================================================================
--Q.11 List all active customers (IsActive = 1) from Dim_Customers.
--================================================================
/*SELECT *
FROM Dim_Customers
WHERE IsActive = 1;*/

--====================================================================
--Q.12 Find all orders with OrderStatus = 'Delivered' from Fact_Orders.
--====================================================================
/*SELECT *
FROM Fact_Orders
WHERE OrderStatus = 'Delivered';*/

--====================================================================
--Q.13 Show products in the 'Electronics' category from Dim_Products.
--====================================================================
/*SELECT *
FROM Dim_Products
WHERE Category = 'Electronics';*/

--====================================================================
--Q.14 List payments made via 'UPI' from Fact_Payments.
--====================================================================
/*SELECT *
FROM Fact_Payments
WHERE PaymentMethod = 'UPI';*/

--==============================================================================
--Q.15 Find all orders with TotalOrderAmount greater than 1000 from Fact_Orders.
--==============================================================================
/*SELECT*
FROM Fact_Orders
WHERE TotalOrderAmount > 1000;*/

--====================================================================
--Q.16 Show customers from the city 'Bengaluru' in Dim_Customers.
--====================================================================
/*SELECT *
FROM Dim_Customers
WHERE City = 'Bangalore';*/

--=======================================================================
--Q.17 Find inventory movements of type 'IN' from Fact_InventoryMovement.
--=======================================================================
/*SELECT *
FROM Fact_InventoryMovement
WHERE MovementType = 'IN';*/

--====================================================================
--Q.18 List orders where DiscountAmount is greater than 0.
--====================================================================
/*SELECT *
FROM Fact_Orders
WHERE DiscountAmount = 0;*/

--====================================================================
--Q.19 List all customers sorted by CustomerName in ascending order.
--====================================================================
/*SELECT *
FROM Dim_Customers
ORDER BY CustomerName ASC;*/

--====================================================================
--Q.20 Show orders sorted by TotalOrderAmount descending.
--====================================================================
/*SELECT *
FROM Fact_Orders
ORDER BY TotalOrderAmount DESC;*/

--====================================================================
--Q.21 List products sorted by Category then by ProductName.
--====================================================================
/*SELECT *
FROM Dim_Products
ORDER BY Category, ProductName;*/

--====================================================================
--Q.22 Show the top 5 most recent orders from Fact_Orders.
--====================================================================
/*SELECT TOP 5 *
FROM Fact_Orders
ORDER BY OrderDate DESC;*/

--========================================================================
--Q.23 Retrieve the first 10 customers who signed up (earliest SignupDate).
--========================================================================
/*SELECT TOP 10 *
FROM Dim_Customers
ORDER BY SignupDate ASC;*/

--====================================================================
--Q.24 Show 10 order items starting from the 11th record (use OFFSET).
--====================================================================
/*SELECT *
FROM Fact_Orders
ORDER BY OrderDate
OFFSET 10 ROWS
FETCH NEXT 10 ROWS ONLY;*/

--========================================================================
--Q.25 Get the top 3 highest paid orders by PaidAmount from Fact_Payments.
--========================================================================
/*SELECT TOP 3*
FROM Fact_Payments
ORDER BY PaidAmount DESC;*/

--=========================================================================================
--Q.26 Find customers who signed up after '2023-01-01' and belong to the 'Premium' segment.
--=========================================================================================
/*SELECT *
FROM Dim_Customers
WHERE SignupDate > '2023-01-01'
AND CustomerSegment = 'Premium';*/

--=========================================================================================
--Q.27 List orders placed in January 2024 from Fact_Orders.
--=========================================================================================
/*SELECT *
FROM Fact_Orders
WHERE OrderDate BETWEEN '2024-01-01' AND '2024-01-31';*/

--=========================================================================================
--Q.28 Show products whose ProductName contains the word 'Pro'.
--=========================================================================================
/*SELECT *
FROM Dim_Products
WHERE ProductName LIKE '%Pro%';*/

--=========================================================================================
--Q.29 Find all cart events where EventType is either 'AddToCart' or 'Purchase'.
--=========================================================================================
/*SELECT *
FROM Fact_CartEvents
WHERE EventType IN ('AddToCart' , 'Purchase');*/

--=========================================================================================
--Q.30 Find orders where DeliveryFee is NULL from Fact_Orders.
--=========================================================================================
/*SELECT *
FROM Fact_Orders
WHERE DeliveryFee IS NULL;*/

--=========================================================================================
--Q.31 Count the number of customers in each CustomerSegment.
--=========================================================================================
/*SELECT 
	COUNT(*) AS CustomerNumbers,
	CustomerSegment
FROM Dim_Customers
GROUP BY CustomerSegment;*/

--=========================================================================================
--Q.32 Find the total TotalOrderAmount for each OrderStatus in Fact_Orders.
--=========================================================================================
/*SELECT 
	OrderStatus,
	SUM(TotalOrderAmount) AS Total
FROM Fact_Orders
GROUP BY OrderStatus;*/

--=========================================================================================
--Q.33 Count how many products exist in each Category from Dim_Products.
--=========================================================================================
/*SELECT 
	Category,
	COUNT(ProductName) AS Products
FROM Dim_Products
GROUP BY Category;*/

--=========================================================================================
--Q.34 Find the average UnitPrice per ProductID in Fact_OrderItems.
--=========================================================================================
/*SELECT 
	ProductID,
	CAST(AVG(UnitPrice) AS DECIMAL(10,2)) AS AvgPrice
FROM Fact_OrderItems
GROUP BY ProductID;*/

--=========================================================================================
--Q.35 Count the number of payments per PaymentMethod in Fact_Payments.
--=========================================================================================
/*SELECT 
	PaymentMethod,
	COUNT(*) AS PaymentsNumber
FROM Fact_Payments
GROUP By PaymentMethod;*/

--=========================================================================================
--Q.36 Find total Quantity moved per ProductID in Fact_InventoryMovement.
--=========================================================================================
/*SELECT 
	ProductID,
	SUM(Quantity) AS TotalQuantity
FROM Fact_InventoryMovement
GROUP BY ProductID;*/

--=========================================================================================
--Q.37 Count cart events by EventType from Fact_CartEvents.
--=========================================================================================
/*SELECT 
	EventType,
	COUNT(EventTime) AS Events
FROM Fact_CartEvents
GROUP BY EventType;*/

--=========================================================================================
--Q.38 Find the maximum TotalAmount per OrderID in Fact_OrderItems.
--=========================================================================================
/*SELECT 
	OrderID,
	MAX(TotalAmount) AS TotalAmount
FROM Fact_OrderItems
GROUP BY OrderID;*/

--=========================================================================================
--Q.39 Show CustomerSegments that have more than 2 customers.
--=========================================================================================
/*SELECT 
	CustomerSegment,
	COUNT(CustomerName) AS Customers
FROM Dim_Customers
GROUP BY CustomerSegment
HAVING COUNT(CustomerName) > 2;*/

--=========================================================================================
--Q.40 List ProductIDs where total ordered quantity (from Fact_OrderItems) exceeds 4.
--=========================================================================================
/*SELECT 
	ProductID,
	SUM(Quantity) AS TotalOrder
FROM Fact_OrderItems
GROUP BY ProductID
HAVING SUM(Quantity) >= 4*/

--=========================================================================================
--Q.41 Find PaymentMethods where total PaidAmount is more than 2500.
--=========================================================================================
/*SELECT 
	PaymentMethod,
	SUM(PaidAmount) AS TotalPaidAmount
FROM Fact_Payments
GROUP BY PaymentMethod
HAVING SUM(PaidAmount) >2500;*/

--=========================================================================================
--Q.42 Show OrderIDs in Fact_OrderItems that have more than 2 line items.
--=========================================================================================
/*SELECT 
	OrderID,
	COUNT(*) AS LineItems
FROM Fact_OrderItems
GROUP BY OrderID
HAVING COUNT(*) > 1;*/

--=============================================================================================
--Q.43 Join Fact_Orders with Dim_Customers to show CustomerName, OrderID, and TotalOrderAmount.
--=============================================================================================
/*SELECT 
	c.CustomerName, o.OrderID, o.TotalOrderAmount
FROM Fact_Orders o
JOIN Dim_Customers c ON o.CustomerID = c.CustomerID;*/

--=============================================================================================
--Q.44 Join Fact_OrderItems with Dim_Products to show ProductName and Quantity ordered.
--=============================================================================================
/*SELECT 
	p.ProductName, oi.Quantity
FROM Fact_OrderItems oi
JOIN Dim_Products p ON oi.ProductID = p.ProductID;*/

--=============================================================================================
--Q.45 Join Fact_Orders with Dim_Location to show OrderID and Zone.
--=============================================================================================
/*SELECT 
	o.OrderID, l.Zone
FROM Fact_Orders o
JOIN Dim_Location l ON o.LocationID = l.LocationID;*/

--=============================================================================================
--Q.46 Join Fact_Payments with Fact_Orders to show OrderDate and PaidAmount.
--=============================================================================================
/*SELECT 
	o.OrderID, pa.PaidAmount
FROM Fact_Payments pa
JOIN Fact_Orders o ON pa.OrderID = o.OrderID;*/

--=============================================================================================
--Q.47 Join Fact_DeliveryLogs with Fact_Orders to show CustomerID and DeliveryPartner.
--=============================================================================================
/*SELECT 
	d.DeliveryPartner, o.CustomerID
FROM Fact_DeliveryLogs d
JOIN Fact_Orders o ON d.OrderID = o.OrderID;*/

--=============================================================================================
--Q.48 Join Fact_CartEvents with Dim_Products to show ProductName and EventType.
--=============================================================================================
/*SELECT 
	P.ProductName, c.EventType
FROM Fact_CartEvents c
JOIN Dim_Products p ON c.ProductID = p.ProductID;*/

--=============================================================================================
--Q.49 Join Fact_InventoryMovement with Dim_Products to show ProductName and MovementType.
--=============================================================================================
/*SELECT 
	p.ProductName, i.MovementType
FROM Fact_InventoryMovement i
JOIN Dim_Products p ON i.ProductID = p.ProductID;*/

--=============================================================================================
--Q.50 Show all orders with customer name using a LEFT JOIN (include orders without matching customers).
--=============================================================================================
/*SELECT 
	o.OrderID, c.CustomerName, o.TotalOrderAmount
FROM Dim_Customers c
LEFT JOIN Fact_Orders o ON c.CustomerID = o.CustomerID*/

--=============================================================================================
--Q.51 List the top 10 orders by TotalOrderAmount descending, showing CustomerID and OrderDate.
--=============================================================================================
/*SELECT TOP 10
	CustomerID,
	OrderDate,
	TotalOrderAmount
FROM Fact_Orders 
ORDER BY TotalOrderAmount DESC;*/

--=============================================================================================
--Q.52 Show products sorted by Category ASC and UnitPrice DESC (using Fact_OrderItems joined with Dim_Products).
--=============================================================================================
/*SELECT
	p.Category, oi.UnitPrice
FROM Fact_OrderItems oi
JOIN Dim_Products p ON oi.ProductID = p.ProductID
ORDER BY p.Category ASC, oi.UnitPrice DESC;*/

--=============================================================================================
--Q.53 Find total revenue (TotalOrderAmount) per month using Fact_Orders. Use OrderDate.
--=============================================================================================
/*SELECT
  MONTH(OrderDate) AS Month,
  YEAR(OrderDate) AS Year,
  SUM(TotalOrderAmount) AS MonthlyRevenue
FROM Fact_Orders
GROUP BY YEAR(OrderDate), MONTH(OrderDate)
ORDER BY Year, Month;*/

--=============================================================================================
--Q.54 Count the number of orders per LocationID in Fact_Orders.
--=============================================================================================
/*SELECT
	LocationID,
	COUNT(*) AS OrderCount
FROM Fact_Orders
GROUP BY LocationID
ORDER BY OrderCount DESC;*/

--=============================================================================================
--Q.55 Show all payments where PaymentStatus = 'Failed' and PaidAmount > 0.
--=============================================================================================
/*SELECT *
FROM Fact_Payments
WHERE PaymentStatus = 'Failed' and PaidAmount > 0;*/

--=============================================================================================
--Q.56 For each customer, show their total number of orders and total spend. Join Dim_Customers with Fact_Orders.
--=============================================================================================
/*SELECT
	c.CustomerName,
	COUNT(o.OrderID) AS TotalOrders,
	SUM(o.TotalOrderAmount) AS TotalSpend
FROM Dim_Customers c
Join Fact_Orders o ON c.CustomerID = o.CustomerID
GROUP BY CustomerName 
ORDER BY TotalSpend DESC;*/

--=============================================================================================
--Q.57 Show the top 5 products by total revenue. Join Dim_Products with Fact_OrderItems.
--=============================================================================================
/*SELECT TOP 5
	p.ProductName,
	SUM(oi.TotalAmount) AS TotalRevenue
FROM Dim_Products p
JOIN Fact_OrderItems oi ON p.ProductID = oi.ProductID
GROUP BY ProductName
ORDER BY TotalRevenue DESC;*/

--=============================================================================================
--Q.58 List customers who have never placed an order (use LEFT JOIN and check for NULL).
--=============================================================================================
/*SELECT
	c.CustomerName
FROM Dim_Customers c
LEFT JOIN Fact_Orders o ON c.CustomerID = o.CustomerID
WHERE o.OrderID IS NULL*/

--=============================================================================================
--Q.59 Show each order with customer name, zone, and delivery partner. Requires 3 table joins.
--=============================================================================================
/*SELECT
	c.CustomerName,
	l.Zone,
	d.DeliveryPartner,
	o.OrderID
FROM Fact_Orders o
JOIN Dim_Customers c ON o.CustomerID = c.CustomerID
JOIN Dim_Location l ON o.LocationID = l.LocationID
JOIN Fact_DeliveryLogs d ON o.OrderID = d.OrderID;*/

--=============================================================================================
--Q.60 Join Fact_Orders, Fact_OrderItems, and Dim_Products to show each order's product names and quantities.
--=============================================================================================
/*SELECT
	o.OrderID,
	p.ProductName,
	oi.Quantity,
	oi.Unitprice
FROM Fact_Orders o
JOIN Fact_Orderitems oi ON o.OrderID = oi.OrderID
JOIN Dim_Products p ON p.ProductID = oi.ProductID;*/

--=============================================================================================
--Q.61 Find customers whose total spend across all orders exceeds 500.
--=============================================================================================
/*SELECT
	c.CustomerName,
	SUM(o.TotalOrderAmount) AS TotalSpend
FROM Fact_Orders o
JOIN Dim_Customers c ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerName
HAVING SUM(o.TotalOrderAmount) > 500;*/

--=============================================================================================
--Q.62 Find product categories where average UnitPrice in Fact_OrderItems is above 20.
--=============================================================================================
/*SELECT
	p.Category,
	CAST(AVG(oi.UnitPrice) AS decimal(10,2)) AS AvgPrice
FROM Fact_OrderItems oi
JOIN Dim_Products p ON oi.ProductID = p.ProductID
GROUP BY p.Category
HAVING AVG(oi.UnitPrice) > 20;*/

--=============================================================================================
--Q.63 Show zones that have more than 2 orders placed. Join Fact_Orders with Dim_Location.
--=============================================================================================
/*SELECT
	l.Zone,
	COUNT(o.OrderID) AS OrderCount
FROM Fact_Orders o
JOIN Dim_Location l ON l.LocationID = O.LocationID
GROUP BY l.Zone
HAVING COUNT(o.OrderStatus) > 2*/

--=============================================================================================
--Q.64 Find total discount given per CustomerSegment. Join Fact_Orders with Dim_Customers.
--=============================================================================================
/*SELECT 
	c.CustomerSegment,
	SUM(o.DiscountAmount) AS TotalDiscount
FROM Fact_Orders o
JOIN Dim_Customers c ON o.CustomerID = c.CustomerID
GROUP BY C.CustomerSegment
ORDER BY TotalDiscount DESC;*/

--=============================================================================================
--Q.65 Show total inventory IN and OUT per ProductID from Fact_InventoryMovement.
--=============================================================================================
/*SELECT
	ProductID,
	MovementType,
	SUM(Quantity) AS TotalInventory
FROM Fact_InventoryMovement
WHERE MovementType != 'Return'
GROUP BY ProductID, MovementType
ORDER BY ProductID;*/

--=============================================================================================
--Q.66 Find average order value per State by joining Fact_Orders with Dim_Customers.
--=============================================================================================
/*SELECT 
	c.State,
	CAST(AVG(o.TotalOrderAmount) AS decimal(10,2)) AS AvgOrderValue
FROM Fact_Orders o
JOIN Dim_Customers c ON o.CustomerID = c.CustomerID
GROUP BY c.State
ORDER BY AvgOrderValue DESC;*/

--=============================================================================================
--Q.67 For each product, show the count of 'AddToCart' events vs 'Purchase' events from Fact_CartEvents. Join with Dim_Products.
--=============================================================================================
/*SELECT
	p.ProductName,
	SUM(CASE WHEN ce.EventType = 'Add' THEN 1 ELSE 0 END) AS AddToCartCount,
	SUM(CASE WHEN ce.EventType = 'Checkout' THEN 1 ELSE 0 END) AS PurchaseCount
FROM Fact_CartEvents ce
JOIN Dim_Products p ON ce.ProductID = p.ProductID
GROUP BY p.ProductName;*/

--=============================================================================================
--Q.68 Find orders placed in Q4 (October–December) of 2024 with status 'Delivered'.
--=============================================================================================
/*SELECT
	*
FROM Fact_Orders
WHERE OrderDate BETWEEN '2024-01-01' AND '2024-01-31'
	AND OrderStatus = 'Delivered'*/

--=============================================================================================
--Q.69 Show customers who placed orders in more than one different zone. Join Fact_Orders with Dim_Location and Dim_Customers.
--=============================================================================================
/*SELECT
	c.CustomerName,
	COUNT(DISTINCT l.Zone) AS ZoneCount
FROM Fact_Orders o
JOIN Dim_Customers c ON o.CustomerID = c.CustomerID
JOIN Dim_Location l ON o.LocationID = L.LocationID
GROUP BY c.CustomerName
HAVING COUNT(DISTINCT l.Zone) > 1;*/

--=============================================================================================
--Q.70 List orders along with their payment status. Show OrderID, OrderStatus, and PaymentStatus.
--=============================================================================================
/*SELECT
	o.OrderID,
	o.OrderStatus,
	p.PaymentStatus
FROM Fact_Orders o
JOIN Fact_Payments p ON o.OrderID = p.OrderID;*/

--=============================================================================================
--Q.71 Find the month with the highest total revenue from Fact_Orders.
--=============================================================================================
/*SELECT TOP 1
	Month(OrderDate) AS Month,
	Year(OrderDate) AS Year,
	SUM(TotalOrderAmount) AS TotalRevenue
FROM Fact_Orders
GROUP BY YEAR(OrderDate), MONTH(OrderDate)
ORDER BY TotalRevenue DESC;*/

--=============================================================================================
--Q.72 Show the full order details including product names, customer name, and payment method for OrderID = 1001.
--=============================================================================================
/*SELECT
	c.CustomerName,
	p.ProductName,
	oi.Quantity,
	oi.UnitPrice,
	pay.PaymentMethod
FROM Fact_Orders o
JOIN Dim_Customers c ON o.CustomerID = c.CutomerId
JOIN Fact_OrderItems oi ON o.OrderID = oi.OrderID
JOIN Dim_Products p ON oi.ProductID = p.ProductID
JOIN Fact_Payments pay ON o.OrderID = pay.OrderID
WHERE o.OrderID = 1001;*/

--=============================================================================================
--Q.73 Find SubCategories where total quantity sold (Fact_OrderItems) is less than 20.
--=============================================================================================
/*SELECT
	p.SubCategory,
	SUM(oi.Quantity) AS TotalSold
FROM Fact_OrderItems oi
JOIN Dim_Products p ON oi.ProductID = p.ProductID
GROUP BY p.SubCategory
HAVING SUM(oi.Quantity) < 20;*/

--=============================================================================================
--Q.74 List products that appear in Fact_CartEvents but have never been ordered in Fact_OrderItems.
--=============================================================================================
/*SELECT
	DISTINCT c.ProductID
FROM Fact_CartEvents c
JOIN Fact_OrderItems oi ON c.ProductID = oi.ProductID
WHERE oi.ProductID IS NULL;*/

--=============================================================================================
--Q.75 Show total revenue per Category per Month. Join Fact_OrderItems with Dim_Products and Fact_Orders.
--=============================================================================================
/*SELECT
  p.Category,
  YEAR(o.OrderDate) AS Year,
  MONTH(o.OrderDate) AS Month,
  SUM(oi.TotalAmount) AS Revenue
FROM Fact_OrderItems oi
JOIN Fact_Orders o ON oi.OrderID = o.OrderID
JOIN Dim_Products p ON oi.ProductID = p.ProductID
GROUP BY p.Category, YEAR(o.OrderDate), MONTH(o.OrderDate)
ORDER BY p.Category, Year, Month;*/

--=============================================================================================
--Q.76 Show DeliveryPartners who have handled more than 2 orders.
--=============================================================================================
/*SELECT
	d.DeliveryPartner,
	COUNT(DISTINCT d.OrderID) AS OrdersHandled
FROM Fact_DeliveryLogs d
GROUP BY d.DeliveryPartner
HAVING COUNT(DISTINCT d.OrderID) > 2;*/

--=============================================================================================
--Q.77 Show each customer's last order date and amount. Join Dim_Customers with Fact_Orders.
--=============================================================================================
/*SELECT
	c.CustomerName,
	MAX(o.OrderDate) AS LastOrderDate,
	SUM(o.TotalOrderAmount) AS TotalSpend
FROM Dim_Customers c
JOIN Fact_Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerName
ORDER BY LastOrderDate DESC;*/

--=============================================================================================
--Q.78 Find customers who signed up in the year 2022 and are from the 'South' zone. Join Dim_Customers with Dim_Location.
--=============================================================================================
/*SELECT 
	c.CustomerName, 
	c.SignupDate, 
	l.Zone
FROM Dim_Customers c
JOIN Fact_Orders o ON c.CustomerID = o.CustomerID
JOIN Dim_Location l ON o.LocationID = l.LocationID
WHERE YEAR(c.SignupDate) = 2022
  AND l.Zone = 'South';*/

--=============================================================================================
--Q.79 Find average delivery fee per Zone by joining Fact_Orders with Dim_Location.
--=============================================================================================
/*SELECT
	l.Zone,
	CAST (AVG (o.DeliveryFee) AS Decimal(10, 2)) AS AvgDeliveryFee
FROM Fact_Orders o
JOIN Dim_Location l ON o.LocationID = l.LocationID
GROUP BY l.Zone
ORDER BY AvgDeliveryFee DESC;*/	

--=============================================================================================
--Q.80 Find the top 3 customers by number of distinct products ordered. Join Dim_Customers, Fact_Orders, Fact_OrderItems.
--=============================================================================================
/*SELECT TOP 3
	c.CustomerName,
	COUNT(DISTINCT oi.ProductID) AS UniqueProducts
FROM Dim_Customers c
JOIN Fact_Orders o ON c.CustomerID = o.CustomerID
JOIN Fact_OrderItems oi ON o.OrderID = oi.OrderID
GROUP BY c.CustomerName
ORDER BY UniqueProducts DESC;*/

--=============================================================================================
--Q.81 Show customers who have placed more than 5 orders with OrderStatus = 'Cancelled'. 
--=============================================================================================
/*SELECT
	c.CustomerName,
	COUNT(o.OrderID) AS CancelledOrders
FROM Fact_Orders o
JOIN Dim_Customers c ON o.CustomerID = c.CustomerID
WHERE o.OrderStatus = 'Cancelled'
GROUP BY c.CustomerName
HAVING COUNT(o.OrderID) > 5;*/

--=============================================================================================
--Q.82 Show the net inventory (IN minus OUT) per product. Join Fact_InventoryMovement with Dim_Products. 
--=============================================================================================
/*SELECT
	p.ProductName,
	SUM(CASE WHEN im.MovementType = 'IN' THEN im.Quantity ELSE -im.Quantity END) AS NetInventory
FROM Fact_InventoryMovement im
JOIN Dim_Products p ON im.ProductID = p.ProductID
GROUP BY p.ProductName
ORDER BY NetInventory;*/

--=============================================================================================
--Q.83 List all 'Failed' payments where the corresponding order has status 'Delivered'. 
--=============================================================================================
/*SELECT
	p.*
FROM Fact_Payments p
JOIN Fact_Orders o ON p.OrderID = o.OrderID
WHERE p.PaymentStatus = 'Failed'
	AND o.OrderStatus = 'Delivered';*/

--=============================================================================================
--Q.84 Count the number of unique customers per Zone who placed at least one order. 
--=============================================================================================
/*SELECT 
	l.Zone,
	COUNT(DISTINCT o.CustomerID) AS UniqueCustomers
FROM Fact_Orders o
JOIN Dim_Location l ON o.LocationID = l.LocationID
GROUP BY l.Zone
ORDER BY UniqueCustomers DESC;*/

--=============================================================================================
--Q.85 Show Zones where total revenue exceeds 200. Join Fact_Orders with Dim_Location. 
--=============================================================================================
/*SELECT
	l.Zone,
	SUM(o.TotalOrderAmount) AS TotalRevenue
FROM Fact_Orders o
JOIN Dim_Location l ON o.LocationID = l.LocationID
GROUP BY l.Zone
HAVING SUM(o.TotalOrderAmount) > 200;*/

--=============================================================================================
--Q.86 Show total quantity of each product sold per Quarter. Use Dim_Date joined via OrderDate. 
--=============================================================================================
/*SELECT
	p.ProductName,
	d.Quarter,
	SUM(oi.Quantity) AS TotalQty
FROM Fact_OrderItems oi
JOIN Fact_Orders o ON oi.OrderID = o.OrderID
JOIN Dim_Products p ON oi.ProductID = p.ProductID
JOIN Dim_Date d ON o.OrderDate = d.DateKey
GROUP BY p.ProductName, d.Quarter
ORDER BY p.ProductName, d.Quarter;*/

--=============================================================================================
--Q.87 Find the payment method with the highest total PaidAmount per CustomerSegment.
--=============================================================================================
/*SELECT
	c.CustomerSegment,
	p.PaymentMethod,
	SUM(p.PaidAmount) AS TotalPaid
FROM Fact_Payments p
JOIN Fact_Orders o ON p.OrderID = o.OrderID
JOIN Dim_Customers c ON o.CustomerID = c.CustomerID
GROUP BY c.CustomerSegment, p.PaymentMethod
ORDER BY c.CustomerSegment, TotalPaid DESC;*/

--=============================================================================================
--Q.88 Show cart events that happened on weekends. Join Fact_CartEvents with Dim_Date.
--=============================================================================================
/*SELECT
	ce.*
FROM Fact_CartEvents ce
JOIN Dim_Date d ON ce.EventTime = d.DateKey
WHERE d.IsWeekend = 1;*/

--=============================================================================================
--Q.89 List products that are in the cart but never purchased. Use Fact_CartEvents grouping by EventType.
--=============================================================================================	
/*SELECT
	DISTINCT p.ProductName
FROM Fact_CartEvents ce
JOIN Dim_Products p ON ce.ProductID = p.ProductID
WHERE ce.EventType = 'AddToCart'
  AND ce.ProductID NOT IN (
    SELECT ProductID FROM Fact_CartEvents WHERE EventType = 'Purchase'
  );*/

--=============================================================================================
--Q.90 Find Pincodes where the number of delivered orders is less than 5.
--=============================================================================================
/*SELECT
	l.Pincode,
	COUNT(o.OrderID) AS DeliveredCount
FROM Fact_Orders o
JOIN Dim_Location l ON o.LocationID = l.LocationID
WHERE o.OrderStatus = 'Delivered'
GROUP BY l.Pincode
HAVING COUNT(o.OrderID) < 5;*/

--=============================================================================================
--Q.91 Show orders placed on weekdays vs weekends (count each). Join Fact_Orders with Dim_Date.
--=============================================================================================
/*SELECT
	d.IsWeekend,
	COUNT(o.OrderID) AS OrderCount
FROM Fact_Orders o
JOIN Dim_Date d ON o.OrderDate = d.DateKey
GROUP BY d.IsWeekend;*/

--=============================================================================================
--Q.92 Calculate the cart-to-purchase conversion rate per product (purchases / total cart events). Use Fact_CartEvents joined with Dim_Products.
--=============================================================================================
/*SELECT
	p.ProductName,
	COUNT(*) AS TotalCartEvents,
	SUM(CASE WHEN ce.EventType = 'Purchase' THEN 1 ELSE 0 END) AS Purchases,
	ROUND(100.0 * SUM(CASE WHEN ce.EventType = 'Purchase' THEN 1 ELSE 0 END) / COUNT(*), 2) AS ConversionRate
FROM Fact_CartEvents ce
JOIN Dim_Products p ON ce.ProductID = p.ProductID
GROUP BY p.ProductName
ORDER BY ConversionRate DESC;*/

--=============================================================================================
--Q.93 Show products where inventory quantity received (MovementType = 'IN') is less than quantity dispatched ('OUT'). 
--=============================================================================================
/*SELECT
	ProductID,
	SUM(CASE WHEN MovementType = 'IN' THEN Quantity ELSE 0 END) AS TotalIN,
	SUM(CASE WHEN MovementType = 'OUT' THEN Quantity ELSE 0 END) AS TotalOut
FROM Fact_InventoryMovement
GROUP BY ProductID
HAVING SUM(CASE WHEN MovementType = 'IN' THEN Quantity ELSE 0 END)
	< SUM(CASE WHEN MovementType = 'OUT' THEN Quantity ELSE 0 END);*/

--=============================================================================================
--Q.94 Show each customer's most recently ordered product. Join Dim_Customers, Fact_Orders, Fact_OrderItems, Dim_Products.
--=============================================================================================
/*SELECT
	c.CustomerName,
	p.ProductName,
	o.OrderDate
FROM Dim_Customers c
JOIN Fact_Orders o ON c.CustomerID = o.CustomerID
JOIN Fact_OrderItems oi ON o.OrderID = oi.OrderID
JOIN Dim_Products p ON oi.ProductID = p.ProductID
WHERE o.OrderDate = (
  SELECT 
	MAX(o2.OrderDate)
  FROM Fact_Orders o2
  WHERE o2.CustomerID = c.CustomerID
)
ORDER BY c.CustomerName;*/

--=============================================================================================
--Q.95 Find the top 3 zones by average order value. Join Fact_Orders with Dim_Location.
--=============================================================================================
/*SELECT TOP 3
	l.Zone,
	CAST(AVG(o.TotalOrderAmount) AS Decimal(10,2)) AS AvgOrderValue
FROM Fact_Orders o
JOIN Dim_Location l ON o.LocationID = l.LocationID
GROUP BY l.Zone
ORDER BY AvgOrderValue;*/

--=============================================================================================
--Q.96 Find customer segments where the average discount given is more than 10% of average order value.
--=============================================================================================
/*SELECT
	c.CustomerSegment,
	CAST(AVG(o.DiscountAmount)AS Decimal(10,2)) AS AvgDiscount,
	CAST(AVG(o.TotalOrderAmount)AS Decimal(10,2)) AS AvgOrderValue
FROM Dim_Customers c
JOIN Fact_Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerSegment
HAVING AVG(o.DiscountAmount) > 0.10%  AVG(o.TotalOrderAmount);*/

--=============================================================================================
--Q.97 Show all orders where payment was successful but delivery status is still 'Pending'. Join Fact_Payments and Fact_DeliveryLogs.
--=============================================================================================
/*SELECT
	d.Status,
	p.PaymentStatus
FROM Fact_Payments p
JOIN Fact_DeliveryLogs d ON p.OrderID = d.OrderID
WHERE p.PaymentStatus = 'Success'
	AND d.Status = 'Out for Delivery';*/

--=============================================================================================
--Q.98 Find the top 5 customers by number of unique product categories purchased.
--=============================================================================================
/*SELECT TOP 5
	c.CustomerName,
	COUNT(DISTINCT p.Category) AS UniqueCategories
FROM Dim_Customers c
JOIN Fact_Orders o ON o.CustomerID = c.CustomerID
JOIN Fact_OrderItems oi ON oi.OrderID = o.OrderID
JOIN Dim_Products p ON p.ProductID = oi.ProductID
GROUP BY c.CustomerName
ORDER BY UniqueCategories DESC;*/

--=============================================================================================
--Q.99 Find months where total cancelled order value exceeds 500.
--=============================================================================================
/*SELECT
	YEAR(OrderDate) AS Year,
	MONTH(OrderDate) AS Month,
	SUM(TotalOrderAmount) AS CancelledValue
FROM Fact_Orders
WHERE OrderStatus = 'Cancelled'
GROUP BY Year(OrderDate), Month(OrderDate)
HAVING SUM(TotalOrderAmount) > 500
ORDER BY Year, Month;*/

--=============================================================================================
--Q.100 Build a full customer order summary: customer name, segment, total orders, total spend, total discount, and average order value. Use all relevant tables.
--=============================================================================================
/*SELECT
	c.CustomerName,
	c.CustomerSegment,
	COUNT(o.OrderID) AS TotalOrders,
	SUM(o.TotalOrderAmount) AS TotalSpend,
	SUM(o.DiscountAmount) AS TotalDiscount,
	CAST(AVG(o.TotalOrderAmount)AS Decimal(10,2)) AS AvgOrderValue
FROM Dim_Customers c
JOIN Fact_Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerName, c.CustomerSegment
ORDER BY TotalSpend DESC;*/
