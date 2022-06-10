# Danny's Diner
Welcome to the first Case Study in a series of 8 of them. </br>
The DataSet and the questions have all been gathered from [8 Week SQL Challenge](https://8weeksqlchallenge.com/case-study-1/)


[ER Diagram](https://dbdiagram.io/d/62a2d15c92b33b4f51393775)

    sqlite3 dannys_diner.db
    
    SQLite version 3.38.5 2022-05-06 15:25:27
    Enter ".help" for usage hints.
    
</br>
    
    sqlite> CREATE TABLE sales (
       ...>   "customer_id" VARCHAR(1),
       ...>   "order_date" DATE,
       ...>   "product_id" INTEGER
       ...> );


    sqlite> INSERT INTO sales
       ...>   ("customer_id", "order_date", "product_id")
       ...> VALUES
       ...>   ('A', '2021-01-01', '1'),
       ...>   ('A', '2021-01-01', '2'),
       ...>   ('A', '2021-01-07', '2'),
       ...>   ('A', '2021-01-10', '3'),
       ...>   ('A', '2021-01-11', '3'),
       ...>   ('A', '2021-01-11', '3'),
       ...>   ('B', '2021-01-01', '2'),
       ...>   ('B', '2021-01-02', '2'),
       ...>   ('B', '2021-01-04', '1'),
       ...>   ('B', '2021-01-11', '1'),
       ...>   ('B', '2021-01-16', '3'),
       ...>   ('B', '2021-02-01', '3'),
       ...>   ('C', '2021-01-01', '3'),
       ...>   ('C', '2021-01-01', '3'),
       ...>   ('C', '2021-01-07', '3');
       
</br>

    sqlite> CREATE TABLE menu (
       ...>   "product_id" INTEGER,
       ...>   "product_name" VARCHAR(5),
       ...>   "price" INTEGER
       ...> );
       
       
    sqlite> INSERT INTO menu
       ...>   ("product_id", "product_name", "price")
       ...> VALUES
       ...>   ('1', 'sushi', '10'),
       ...>   ('2', 'curry', '15'),
       ...>   ('3', 'ramen', '12');
       
</br>

    sqlite> CREATE TABLE members (
       ...>   "customer_id" VARCHAR(1),
       ...>   "join_date" DATE
       ...> );
       
       
    sqlite> INSERT INTO members
       ...>   ("customer_id", "join_date")
       ...> VALUES
       ...>   ('A', '2021-01-07'),
       ...>   ('B', '2021-01-09');


        sqlite> .headers ON
        sqlite> .mode column

1. What is the total amount each customer spent at the restaurant?
       
       SELECT customer_id, SUM(price) as Total_Amt
       FROM sales s JOIN menu m
         ON s.product_id = m.product_id
       GROUP BY customer_id;


2. How many days has each customer visited the restaurant?

         SELECT customer_id, count(DISTINCT order_date) AS No_Of_Visits
         FROM sales s JOIN menu m
           ON s.product_id = m.product_id
         GROUP BY customer_id;
           

3. What was the first item from the menu purchased by each customer?
        
        SELECT DISTINCT customer_id, product_id, product_name 
        FROM 
            (SELECT s.*, m.*, RANK() OVER (partition by customer_id ORDER BY order_date) AS rnk 
              FROM menu m JOIN sales s 
                ON m.product_id = s.product_id) 
        WHERE rnk =1;
        
        **OR**
        
         WITH rnk_table as 
         (
            SELECT s.*, m.*, RANK() OVER (partition by customer_id ORDER BY order_date) AS rnk
            FROM menu m JOIN sales s
            ON m.product_id = s.product_id
          )
         SELECT DISTINCT customer_id, product_id, product_name
         FROM rnk_table
         WHERE rnk = 1;

4. What is the most purchased item on the menu and how many times was it purchased by all customers?

        SELECT s.product_id, product_name, COUNT(customer_id) AS Most_Ordered
        FROM sales s JOIN menu m
        ON s.product_id = m.product_id
        GROUP BY s.product_id
        ORDER BY Most_Ordered
        LIMIT 1;


5. Which item was the most popular for each customer?

   


6. Which item was purchased first by the customer after they became a member?

        



7. Which item was purchased just before the customer became a member?




8. What is the total items and amount spent for each member before they became a member?

9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?

10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
