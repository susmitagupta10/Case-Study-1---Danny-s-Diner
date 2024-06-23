# Case-Study-1---Danny-s-Diner link- https://8weeksqlchallenge.com/case-study-1/
![Screenshot 2024-06-23 104807](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/7feb7daa-3588-4e7f-8f5a-af4b006ae2c6)
![Screenshot 2024-06-23 104310](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/13edd1ed-cb6b-476a-9c66-8a865fd01003)
![Screenshot 2024-06-23 104353](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/ee5f2af6-1fae-44d0-b31b-201a20cf83c1)
![Screenshot 2024-06-23 104410](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/265de980-0d13-4f5e-8420-19e2c8af065e)


#CREATE SCHEMA#

CREATE SCHEMA dannys_diner;

use dannys_diner;


CREATE TABLE sales (
  
  customer_id VARCHAR(1),
  
  order_date DATE,
 
  product_id INTEGER
);

INSERT INTO sales
  
  (customer_id, order_date, product_id)

VALUES
  
  ('A', '2021-01-01', '1'),
 
  ('A', '2021-01-01', '2'),
  
  ('A', '2021-01-07', '2'),
  
  ('A', '2021-01-10', '3'),
  
  ('A', '2021-01-11', '3'),
  
  ('A', '2021-01-11', '3'),
  
  ('B', '2021-01-01', '2'),
  
  ('B', '2021-01-02', '2'),
  
  ('B', '2021-01-04', '1'),
  
  ('B', '2021-01-11', '1'),
  
  ('B', '2021-01-16', '3'),
  
  ('B', '2021-02-01', '3'),
  
  ('C', '2021-01-01', '3'),
  
  ('C', '2021-01-01', '3'),
  
  ('C', '2021-01-07', '3');
 

CREATE TABLE menu (
 
  product_id INTEGER,
  
  product_name VARCHAR(5),
  
  price INTEGER
);

INSERT INTO menu
  
  (product_id, product_name, price)

VALUES

  ('1', 'sushi', '10'),
  
  ('2', 'curry', '15'),
  
  ('3', 'ramen', '12');
  

CREATE TABLE members (
  
  customer_id VARCHAR(1),
  
  join_date DATE
);

INSERT INTO members
  
  (customer_id, join_date)

VALUES

  ('A', '2021-01-07'),
  
  ('B', '2021-01-09');

#1. What is the total amount each customer spent at the restaurant?

select s.customer_id, sum(m.price) as "Total spend amount"

from sales s

join menu m on m.product_id=s.product_id

group by customer_id

![q1Screenshot 2024-06-23 105227](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/a239d949-d597-45f0-8780-d57d457b231d)


#2.How many days has each customer visited the restaurant?.

select customer_id,  count(distinct (order_date)) as "Day_visited"

from sales s

group by customer_id

![Screenshot 2024-06-23 105324](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/9e516e34-5750-40bf-b026-ed4a7b5b385f)

#3 What was the first item from the menu purchased by each customer?

with first_item as (select s.customer_id, m.product_name,s.order_date, dense_rank () over (partition by s.customer_id order by s.order_date) as "rank_by_date"

from sales s

join menu m on m.product_id=s.product_id) 

select 

  customer_id, product_name

from first_item

where rank_by_date = 1

group by customer_id, product_name

![q3Screenshot 2024-06-23 105418](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/098e204a-0f6a-4bee-9589-af120cab4bfc)

#4 What is the most purchased item on the menu and how many times was it purchased by all customers?

select m.product_name, count(s.order_date) as total_order

from  sales s

join menu m on m.product_id=s.product_id

group by m.product_name

order by total_order desc

limit 1

![Screenshot 2024-06-23 110135](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/cc8ab94d-61a1-4944-ab3a-712e8a928d1c)

#5 Which item was the most popular for each customer?

WITH popular_item as (

select s.customer_id, m.product_name, count(s.order_date) as "total_order", dense_rank() over (partition by s.customer_id order by count(s.order_date) desc) as ranking

from  sales s

join menu m on m.product_id=s.product_id

group by s.customer_id,m.product_name
)

select customer_id, product_name,total_order

from popular_item

where ranking=1

![Screenshot 2024-06-23 110219](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/a1e7aab7-d61c-4ee9-8c08-5e31a1eb1d21)

#6. Which item was purchased first by the customer after they became a member?

WITH member_order as (select m.product_name, s.customer_id, s.order_date, dense_rank() over(partition by s.customer_id order by s.order_date) as ranking

from  sales s

join menu m on m.product_id=s.product_id

join members me on me.customer_id=s.customer_id

where order_date >= me.join_date
)

select customer_id, product_name, order_date

from member_order

where ranking = 1
![Screenshot 2024-06-23 110454](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/6d6d0a2f-76b5-409e-9634-2408b906343b)

#7 Which item was purchased just before the customer became a member?

with member_order as (select s.customer_id,m.product_name, s.order_date, dense_rank() over(partition by s.customer_id order by s.order_date) as ranking

from  sales s

join menu m on m.product_id=s.product_id

join members me on me.customer_id=s.customer_id

where order_date < me.join_date
)

select customer_id, product_name, order_date

from member_order

where ranking = 1
![Screenshot 2024-06-23 110617](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/af7f746a-2bc1-4085-80b8-b78d1533b635)


#8 What is the total items and amount spent for each member before they became a member?

Select  s.customer_id,sum(m.price) as total_price, count(m.product_name)

from sales s

join menu m using (product_id)

join members me using (customer_id)

where s.order_date < me.join_date

group by s.customer_id

![Screenshot 2024-06-23 110617](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/46420fa1-f804-4228-8d01-66d05b270c6a)

#9 If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?

select S.customer_id, SUM(

case when  m.product_name= "sushi" then m.price*20

else m.price*10

end) AS points

FROM sales s

join menu m using (product_id)

group by s.customer_id
![Screenshot 2024-06-23 110759](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/4e9d8d1b-58b2-49a2-aeff-c8f61b83b6d2)

#10 In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - 
how many points do customer A and B have at the end of January?

select S.customer_id, sum(

case when product_id= 1 then  m.price*20
	
  when product_id != 1  and s.order_date<me.join_date then  m.price*10
    
    when  (s.order_date between me.join_date and me.join_date + 6 )  then  m.price*20

else m.price*10

end) AS points

FROM sales s

join menu m using (product_id)

join members me using (customer_id)

where s.order_date<='2021-01-31'

group by s.customer_id

![Screenshot 2024-06-23 110917](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/8dacda80-b970-4365-a77b-caa60f9901b8)

# Bonus Questions 1#
# Join All The Things -> customer_id,order_date,product_name,price,member#


select s.customer_id,s.order_date,m.product_name,m.price, 

(case when s.order_date>me.join_date then "Y"

else "N"

end) as member

from sales s

join menu m on m.product_id=s.product_id

left join members me on me.customer_id=s.customer_id

![Screenshot 2024-06-23 111135](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/11eb4001-06ad-4156-bcd9-658beae47cf8)


# Bonus Questions 2#
Danny also requires further information about the ranking of customer products, but he purposely does not need the ranking for non-member purchases 
so he expects null ranking values for the records when customers are not yet part of the loyalty program

with detail_table as(

select s.customer_id,s.order_date,m.product_name,m.price, 

(case when s.order_date>=me.join_date then "Y"

else "N"

end) as member_

from sales s
join menu m on m.product_id=s.product_id

left join members me on me.customer_id=s.customer_id)

select *,(case when member_= "y" then dense_rank() over(partition by customer_id, member_ order by order_date)

else NULL

END )AS RANKING

from detail_table

![Screenshot 2024-06-23 111405](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/99c7a218-7673-4414-920b-9baeffd47ab2)















