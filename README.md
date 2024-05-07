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

