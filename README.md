
-- Q What are the first 10 books in the Fiction genre?
SELECT * from books 
where genre like 'Fiction' limit 10


--Q Which books were published after the year 1950?
SELECT * from books 
 where books.published_year > 1950

-- Q Who are the last 5 customers (by name) from Canada?
 select * from customers 
 where country  like 'Canada' 
 order by customers."name" DESC limit 5

-- Q Which orders were placed during the month of November 2023?
SELECT * FROM orders
WHERE order_date >= '2023-11-01'
  AND order_date <  '2023-12-01';

--Q What is the total number of books available in stock?
select sum(stock) as Total_Stock from books 


-- Q Which book is the most expensive?
SELECT * from books order by price desc limit 1

--Q Which orders include more than one book?
SELECT * from orders where quantity>1

-- Q Which orders have a total amount greater than $20, sorted by customer ID in descending order?
select * from orders where total_amount > '20.00' order by orders.customer_id desc

-- Q What are the different genres available in the books collection?
SELECT   distinct genre from books


-- Q Which books are currently out of stock?
SELECT * from books where stock=0  order by stock asc

--  What is the total revenue generated from all orders?
SELECT cast(sum(orders.total_amount) as int ) as total_revenue from orders



-- Advance Questions

--  How many books are there in each genre?
SELECT genre, COUNT(*) AS book_count
FROM books
GROUP BY genre;

-- How many books were sold in each genre?
SELECT b.genre, sum(o.quantity) as Total_Books_Sold
from orders o
join books b on o.book_id = o.book_id
GROUP by b.genre


-- What is the average price of books in the Fantasy genre (rounded to the nearest whole number)?
SELECT TRUNC(AVG(price)) AS Avg_Price_of_Fantasy
FROM books
WHERE genre = 'Fantasy';


select c.name, SUM(o.quantity) AS total_order_quantity from customers  c
join orders o on o.customer_id = c.customer_id
GROUP by c."name"
having sum(o.quantity) > 2
order by total_order_quantity desc
limit 5

-- Which customers have ordered more than 2 books, 
-- and what is the total quantity of books they have ordered?
SELECt o.book_id,b.title, count(o.order_id) as Order_Count
from orders o
join books b on o.book_id=b.book_id
group by o.book_id,b.title
order by Order_Count desc LIMIT 1

-- Which are the 3 least expensive books in the Fantasy genre?
SELECT * from books where books.genre='Fantasy' order by price asc limit 3


-- Which customers from different cities have spent more than $20 on books,
-- ordered by the highest book price?
SELECT c.city, c.name,b.price from customers c
join orders o on o.customer_id = c.customer_id
join books b on b.book_id = o.book_id
GROUP by c.city,c."name",b.price
having sum(b.price) > 20.00
order by b.price DESC limit 10


-- Which 5 books have sold the most copies overall?
SELECT 
  b.book_id,
  b.title, 
  b.author, 
  SUM(o.quantity) AS total_sold
FROM books b
JOIN orders o ON o.book_id = b.book_id
GROUP BY b.book_id, b.title, b.author
ORDER BY total_sold DESC
LIMIT 5;

-- Which customers have spent more than $30 on orders, 
-- and what is their total spend, ordered by the highest total spend?
SELECT c.customer_id, c.name,c.phone, TRUNC(sum(o.total_amount)) as Total_spend_by_Person
from customers c 
JOIN orders o on o.customer_id = c.customer_id
where o.total_amount > 30
GROUP by c.customer_id, c.name,c.phone
order by Total_spend_by_Person desc 












