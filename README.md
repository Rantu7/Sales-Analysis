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

