# Danny's Diner
Welcome to the first Case Study in a series of 8 of them. </br>
The DataSet and the questions have all been gathered from [8 Week SQL Challenge](https://8weeksqlchallenge.com/case-study-1/)


[ER Diagram](https://dbdiagram.io/d/62a2d15c92b33b4f51393775)

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
        
        
        -----------------------------OR-----------------------------


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
