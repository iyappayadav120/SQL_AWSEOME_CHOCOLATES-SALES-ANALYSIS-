select*from sales;

select*from geo;

select*from products;

select*from people;  


-- 1 )top 10 products
select  pr.Product,sum(s.amount) as total_sales
from sales s join products pr
on pr.PID=s.PID
group by  pr.Product
order by total_sales desc
limit 10;

-- 2 )customers per geographical region
select count(s.Customers)  Customers,g.Geo
from sales s join geo g
on s.GeoID=g.GeoID 
GROUP by g.Geo
order by Customers desc;

-- 3 )total sales amount and customer per region
select sum(s.amount) total_sales,count(s.Customers),g.Geo
from sales s join geo g 
on s.GeoID = g.GeoID
group by g.	Geo; 

-- 4 )total sales and customers for each product 
select sum(s.amount) total_sales,count(s.Customers) Customers,pr.Product
from sales s join products pr
on s.PID=pr.PID
group by pr.Product
order by Customers desc;

-- 5 )total sales and customers for each product category
select sum(s.amount) total_sales,count(s.Customers) Customers,pr.Category
from sales s join products pr
on s.PID=pr.PID
group by pr.Category
order by Customers desc;

-- 6 )salesperson highest sales in each region
select p.salesperson, sum(s.amount)as total_sales,g.Geo
from sales s join people p on
s.SPID=p.SPID join
geo g on s.GeoID=g.GeoID
group by p.salesperson,g.Geo
order by total_sales desc;

-- 7 )salesperson with highest sales overall
SELECT p.salesperson, 
SUM(s.amount) AS total_sales, 
COUNT(DISTINCT s.Customers) AS total_customers
FROM sales s
JOIN people p ON s.SPID = p.SPID
GROUP BY p.salesperson
order by total_sales desc;



-- 8 )salesperson made the highest sales in region
WITH SalesPerRegion AS (
    SELECT p.salesperson, 
           g.Region,
           SUM(s.amount) AS total_sales,
           ROW_NUMBER() OVER (PARTITION BY g.Region ORDER BY SUM(s.amount) DESC) AS sales_rank
    FROM sales s 
    JOIN people p ON s.SPID = p.SPID 
    JOIN geo g ON s.GeoID = g.GeoID 
    GROUP BY p.salesperson, g.Region
)
SELECT salesperson, Region, total_sales
FROM SalesPerRegion
WHERE sales_rank = 1;



-- 9 )salesperson made the highest sales in each geo
WITH SalesPerGeo AS (
    SELECT p.salesperson, 
           g.Geo,
           SUM(s.amount) AS total_sales,
           ROW_NUMBER() OVER (PARTITION BY g.Geo ORDER BY SUM(s.amount) DESC) AS sales_rank
    FROM sales s 
    JOIN people p ON s.SPID = p.SPID 
    JOIN geo g ON s.GeoID = g.GeoID 
    GROUP BY p.salesperson, g.Geo
)
SELECT salesperson, Geo, total_sales
FROM SalesPerGeo
WHERE sales_rank = 1;

-- 10 )the total sales amount and the number of customers for each month.
select sum(Amount) total_sales,count(Customers) Customers,month(saledate) monthh,year(saledate) yearr
from sales
group by monthh,yearr
order by Customers desc;

-- 11 )the busiest month in terms of sales and the number of customers.
select sum(Amount) total_sales,count(Customers) Customers,month(saledate) monthh,year(saledate) yearr
from sales
group by monthh,yearr
order by Customers desc
limit 1;

-- 12 )sales trends over time for a specific product.
select sum(s.amount) total_sales,pr.Product,s.saledate
from sales s join products pr
on s.PID=pr.PID
where pr.PID='P06'
group by Product,SaleDate
order by total_sales desc;

--  13)which product had performed good in india
select pr.product,sum(amount) total_sales,g.geo
from sales s join products pr
on s.PID=pr.PID
join geo g on
s.GeoID=g.GeoID
WHERE g.GeoID='G1'
group by pr.product,g.Geo
order by total_sales desc;


-- 14) top-selling product in each geo
select geo,product,total_sales
from
(select*,row_number() over(partition by geo order by total_sales desc ) product_rank
from
(select pr.product,sum(amount) total_sales,g.geo
from sales s join products pr
on s.PID=pr.PID
join geo g
on s.GeoID=g.GeoID
group by pr.product,g.geo) top_sellingin_geo)ranked_data
where product_rank=1;
