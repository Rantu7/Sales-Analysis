# <p align="center">Pizza Sales Data Analysis</p>
## Objective
The objective of the "Pizza Sales Data Analysis" project is to gain valuable insights by answering critical business questions posed by the owner. By leveraging SQL programming in MySQL, key performance indicators such as total revenue, total orders, sales per day etc. along with more complex questions were answered . To enhance understanding and facilitate decision-making, the results were then visualized using relevant graphs and charts in Power BI. Through this approach, we tried to provide actionable insights that would assist the owner to make calculated strategic decisions and drive business growth.

## About the Data
The data set consisted of total 4 .csv files which can be found here. Brief description about the columns in the data files are given in the table below, where the first row indicates the file names:
![Q1](https://i.ibb.co/bRcbKyN/datass.png)

## The Questions
## 1. What is the	total number of orders placed?
```mysql
SELECT 
    COUNT(ID) AS TOTAL_ORDERS
FROM
    ORDERS;
```
Result: 

![Q1](https://i.ibb.co/pWWxSMF/1.png)

## 2. Calculate the total revenue generated from pizza sales.
```mysql
SELECT 
    ROUND(SUM(PIZZAS.PRICE * ORDER_DETAILS.QUANTITY),2) AS REVENUE
FROM
    PIZZAS
JOIN
    ORDER_DETAILS ON PIZZAS.PIZZA_ID = ORDER_DETAILS.PIZZA_ID;
```
Result: 

![Q2](https://i.ibb.co/MpZjtv9/2.png)

## 3. Identify the highest-priced pizza.
```mysql
SELECT 
    PIZZA_TYPES.NAME, PIZZAS.PRICE
FROM
    PIZZA_TYPES
        JOIN
    PIZZAS ON PIZZA_TYPES.PIZZA_TYPE_ID = PIZZAS.PIZZA_TYPE_ID
ORDER BY PRICE DESC
LIMIT 1;
```
Result:

![Q3](https://i.ibb.co/JR80rf1/3.png)

## 4. Identify the most common pizza size ordered.
```mysql
SELECT 
    PIZZAS.SIZE, COUNT(ORDER_DETAILS.QUANTITY) TOTAL_ORDERS
FROM
    ORDER_DETAILS
        JOIN
    PIZZAS ON ORDER_DETAILS.PIZZA_ID = PIZZAS.PIZZA_ID
GROUP BY SIZE;
```
Result:

![Q4](https://i.ibb.co/tLGZGKS/4.png)

Visualization:

![Q4](https://i.ibb.co/VpdWWyD/4bi.png)

## 5.	List the top 5 most ordered pizza types along with their quantities.
```mysql
SELECT 
    PIZZA_TYPES.NAME,
    SUM(ORDER_DETAILS.QUANTITY) AS TOTAL_QUANTITY
FROM
    PIZZA_TYPES
        JOIN
    PIZZAS ON PIZZA_TYPES.PIZZA_TYPE_ID = PIZZAS.PIZZA_TYPE_ID
        JOIN
    ORDER_DETAILS ON ORDER_DETAILS.PIZZA_ID = PIZZAS.PIZZA_ID
GROUP BY PIZZA_TYPES.NAME
ORDER BY 2 DESC
LIMIT 5;
```
Result:

![Q5](https://i.ibb.co/k4mZt52/5.png)


## 6. Find the total quantity of each pizza category ordered.
```mysql
SELECT 
    PIZZA_TYPES.CATEGORY,
    SUM(ORDER_DETAILS.QUANTITY) AS TOTAL_QUANTITY
FROM
    PIZZA_TYPES
        JOIN
    PIZZAS ON PIZZA_TYPES.PIZZA_TYPE_ID = PIZZAS.PIZZA_TYPE_ID
        JOIN
    ORDER_DETAILS ON ORDER_DETAILS.PIZZA_ID = PIZZAS.PIZZA_ID
GROUP BY 1;
```
Result:

![Q6](https://i.ibb.co/YLGNfcf/6.png)

Visualization:

![Q6](https://i.ibb.co/6s4R3zc/6bi.png)

## 7. Determine the distribution of orders by hour of the day.
```mysql
SELECT 
    HOUR(ORDER_TIME), COUNT(ID) AS ORDER_COUNT
FROM
    ORDERS
GROUP BY 1;
```
Result:

![Q7](https://i.ibb.co/V3TkPmN/7.png)

Visualization:

![Q7](https://i.ibb.co/pJ8Tf4T/7b.png)

## 8.	What is the average amount of pizzas ordered per day?
```mysql
SELECT 
    ROUND(AVG(QUANTITY),2) AS AVG_ORDERS_PER_DAY
FROM
    (SELECT 
        ORDERS.ORDER_DATE, SUM(ORDER_DETAILS.QUANTITY) AS QUANTITY
    FROM
        ORDERS
    JOIN ORDER_DETAILS ON ORDERS.ID = ORDER_DETAILS.ID
    GROUP BY 1) AS SUBQ;
```
Result:

![Q8](https://i.ibb.co/DKrFxBr/8.png)

## 9.	Determine the top 5 most ordered pizza types based on revenue.
```mysql
SELECT 
    PIZZA_TYPES.NAME,
    ROUND(SUM(PIZZAS.PRICE * ORDER_DETAILS.QUANTITY),2) AS REVENUE
FROM
    PIZZAS
        JOIN
    ORDER_DETAILS ON PIZZAS.PIZZA_ID = ORDER_DETAILS.PIZZA_ID
        JOIN
    PIZZA_TYPES ON PIZZAS.PIZZA_TYPE_ID = PIZZA_TYPES.PIZZA_TYPE_ID
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```
Result:

![Q9](https://i.ibb.co/c8mgHxX/9.png)

Visualization:

![Q9](https://i.ibb.co/cbcDWLP/5bi.png)

## 10.	Calculate the percentage contribution of each pizza type to total revenue.
```mysql
SELECT 
    PIZZA_TYPES.Name, ROUND(SUM(PIZZAS.PRICE * ORDER_DETAILS.QUANTITY),2) AS Revenue,
	(SUM(PIZZAS.PRICE * ORDER_DETAILS.QUANTITY)/
		(SELECT SUM(PIZZAS.PRICE * ORDER_DETAILS.QUANTITY)
		 FROM PIZZAS
		 JOIN
    ORDER_DETAILS ON PIZZAS.PIZZA_ID = ORDER_DETAILS.PIZZA_ID
    JOIN PIZZA_TYPES ON PIZZAS.PIZZA_TYPE_ID = PIZZA_TYPES.PIZZA_TYPE_ID))*100 AS Revenue_percentage
FROM
    PIZZAS
JOIN
    ORDER_DETAILS ON PIZZAS.PIZZA_ID = ORDER_DETAILS.PIZZA_ID
    JOIN PIZZA_TYPES ON PIZZAS.PIZZA_TYPE_ID = PIZZA_TYPES.PIZZA_TYPE_ID
    GROUP BY 1 ORDER BY 2 DESC;
```
Result:

![Q10](https://i.ibb.co/W25B3m4/10.png)

## 11.	Find the cumulative revenue generated over time.
```mysql
SELECT ORDER_DATE,
ROUND(SUM(REVENUE) OVER (ORDER BY ORDER_DATE),2) AS CUMULATIVE_REVENUE 
FROM
	(SELECT ORDERS.ORDER_DATE, SUM(PIZZAS.PRICE * ORDER_DETAILS.QUANTITY) AS REVENUE 
    FROM ORDERS
    JOIN ORDER_DETAILS ON ORDERS.ID = ORDER_DETAILS.ID
    JOIN PIZZAS ON PIZZAS.PIZZA_ID = ORDER_DETAILS.PIZZA_ID
    GROUP BY 1) AS SUBQUERY;
```
Result:

![Q11](https://i.ibb.co/L0FyBZt/11.png)

Visualization:

![Q11](https://i.ibb.co/CV6jzDq/11b.png) by month

## 12.	Find the top 3 most ordered pizza types based on revenue for each pizza category.
```mysql
 SELECT NAME, CATEGORY, REVENUE FROM
	(SELECT NAME, CATEGORY, REVENUE, 
	RANK() OVER (PARTITION BY CATEGORY ORDER BY REVENUE DESC) AS RANKS 
	FROM
		(SELECT PIZZA_TYPES.NAME, PIZZA_TYPES.CATEGORY, SUM(PIZZAS.PRICE * ORDER_DETAILS.QUANTITY) AS REVENUE 
		FROM PIZZA_TYPES JOIN PIZZAS ON PIZZA_TYPES.PIZZA_TYPE_ID = PIZZAS.PIZZA_TYPE_ID
		JOIN ORDER_DETAILS ON PIZZAS.PIZZA_ID = ORDER_DETAILS.PIZZA_ID
		GROUP BY 1,2) AS SUBQ) 
	AS SUBQ2
WHERE RANKS <=3;
```
Result:

![Q12](https://i.ibb.co/3pZ1x2P/12.png)

## 13.	Rank each pizza category based on the total revenue.
```mysql
SELECT CATEGORY, TOTAL_REVENUE,
       RANK() OVER (ORDER BY TOTAL_REVENUE DESC) AS CATEGORY_RANK
FROM (
    SELECT PIZZA_TYPES.CATEGORY, ROUND(SUM(PIZZAS.PRICE *ORDER_DETAILS.QUANTITY),2) AS TOTAL_REVENUE 
    FROM PIZZA_TYPES JOIN PIZZAS ON  PIZZA_TYPES.PIZZA_TYPE_ID = PIZZAS.PIZZA_TYPE_ID
    JOIN  ORDER_DETAILS ON PIZZAS.PIZZA_ID = ORDER_DETAILS.PIZZA_ID
    GROUP BY 1
) AS CATEGORY_REVENUE;
```
Result:

![Q13](https://i.ibb.co/LRbwmXs/13.png)

## 14.	Calculate revenue generated for each month and find the months that made the highest revenue.
```mysql
SELECT 
    MONTH(order_date) AS month,
    ROUND(SUM(price * quantity), 2) AS revenue
FROM
    orders
        JOIN
    order_details ON orders.id = order_details.id
        JOIN
    pizzas ON order_details.pizza_id = pizzas.pizza_id
GROUP BY MONTH(order_date)
ORDER BY 2 DESC;
```
Result:

![Q14](https://i.ibb.co/q1y9W1C/14.png)

Visualization:

![Q14](https://i.ibb.co/RQCyPvS/14b.png)



