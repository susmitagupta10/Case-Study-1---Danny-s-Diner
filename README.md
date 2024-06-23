# Case-Study-1---Danny-s-Diner link- https://8weeksqlchallenge.com/case-study-1/
![Screenshot 2024-06-23 104807](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/7feb7daa-3588-4e7f-8f5a-af4b006ae2c6)
![Screenshot 2024-06-23 104310](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/13edd1ed-cb6b-476a-9c66-8a865fd01003)
![Screenshot 2024-06-23 104353](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/ee5f2af6-1fae-44d0-b31b-201a20cf83c1)
![Screenshot 2024-06-23 104410](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/265de980-0d13-4f5e-8420-19e2c8af065e)

#1. What is the total amount each customer spent at the restaurant?

use dannys_diner;
select s.customer_id, sum(m.price) as "Total spend amount"
from sales s
join menu m on m.product_id=s.product_id
group by customer_id

![q1Screenshot 2024-06-23 105227](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/a239d949-d597-45f0-8780-d57d457b231d)

#2.How many days has each customer visited the restaurant?.

use dannys_diner;
select customer_id,  count(distinct (order_date)) as "Day_visited"
from sales s
group by customer_id

![Screenshot 2024-06-23 105324](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/9e516e34-5750-40bf-b026-ed4a7b5b385f)

#3 What was the first item from the menu purchased by each customer?
use dannys_diner;
with first_item as (select s.customer_id, m.product_name,s.order_date, dense_rank () over (partition by s.customer_id order by s.order_date) as "rank_by_date"
from sales s
join menu m on m.product_id=s.product_id) 
select customer_id, product_name
from first_item
where rank_by_date = 1
group by customer_id, product_name

![q3Screenshot 2024-06-23 105418](https://github.com/susmitagupta10/Case-Study-1---Danny-s-Diner/assets/166834605/098e204a-0f6a-4bee-9589-af120cab4bfc)









