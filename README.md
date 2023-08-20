# SQL-PROJECT-2-Pizza-Runner
![Pizza runner](https://8weeksqlchallenge.com/images/case-study-designs/2.png)

This project was carried out using Postgresql to solve the pizza runner business problem and to get insights. The pizza runner database consists of 6 tables:customer_orders, runner_orders, pizza_names, runners, pizza_recipes, pizza_toppings.

Let's start querying the database to find insights on how to improve sales. Note, the database has been cleaned to prepare the database for analysis.

1. How many pizzas were ordered?
   
        SELECT COUNT(order_id) AS total_pizza_ordered
        FROM pizza_runner.customer_orders;
![image](https://github.com/lolatunji/SQL-PROJECT-2-Pizza-Runner/assets/108012164/041ab7ed-6c62-4dd8-99f6-5b94b9cd74b0)

2. How many unique customer orders were made?
 
         SELECT COUNT(DISTINCT (order_id)) AS number_of_unique_order
         FROM pizza_runner.customer_orders;
   ![image](https://github.com/lolatunji/SQL-PROJECT-2-Pizza-Runner/assets/108012164/0e4cf3ca-dec6-43f4-bf47-82e000094b08)

3. How many successful orders were delivered by each runner?

          SELECT runner_id, COUNT(*) AS successful_delivery 
          FROM pizza_runner.runner_orders
          WHERE cancellation  IS NULL
          GROUP BY runner_id
          ORDER BY runner_id;
   ![image](https://github.com/lolatunji/SQL-PROJECT-2-Pizza-Runner/assets/108012164/7b9eadfc-b1d1-47ba-bae3-e0df8df98546)

4. How many of each type of pizza was delivered?
   
                   SELECT pizza_id, COUNT(*) AS total_pizza_delivered 
                  FROM  pizza_runner.customer_orders
                  JOIN pizza_runner.runner_orders
                  USING (order_id)
                  WHERE cancellation IS NULL
                  GROUP BY pizza_id;
   ![image](https://github.com/lolatunji/SQL-PROJECT-2-Pizza-Runner/assets/108012164/93b44a01-acac-48c4-bda0-d39a5e6f8a82)

5. How many Vegetarian and Meatlovers were ordered by each customer?
   
               SELECT DISTINCT customer_id,
              COUNT(CASE WHEN pizza_name = 'Vegetarian' THEN 1 END)AS numbers_vegetarian,
              COUNT(CASE WHEN pizza_name = 'Meatlovers' THEN 1 END) AS numbers_meatlovers
              FROM pizza_runner.customer_orders
              JOIN pizza_runner.pizza_names
              USING(pizza_id)
              GROUP BY customer_id
              ORDER BY customer_id;
   ![image](https://github.com/lolatunji/SQL-PROJECT-2-Pizza-Runner/assets/108012164/0f8dd571-e686-4840-bb1c-636031c4ed0a)

6. What was the maximum number of pizzas delivered in a single order?
   
                 WITH CTE AS
	              (SELECT DISTINCT order_id, COUNT(pizza_id) AS numberof_pizza_ordered
	              FROM pizza_runner.customer_orders
	              JOIN pizza_runner.runner_orders
	              USING (order_id)
	              WHERE distance != 'null'
	              GROUP BY order_id
	              ORDER BY order_id)
	              SELECT MAX(numberof_pizza_ordered)
	              FROM CTE;
   ![image](https://github.com/lolatunji/SQL-PROJECT-2-Pizza-Runner/assets/108012164/65ee71e4-efc7-433a-825e-dc67842a79fe)

7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
   
                  SELECT customer_id,
                  COUNT(CASE WHEN exclusions IS NOT NULL OR extras IS NOT NULL THEN 1 END) AS delivery_pizza_change,
                   COUNT(CASE WHEN exclusions IS NULL AND extras IS NULL THEN 1 END) AS delivery_pizza_nochange
                    FROM pizza_runner.customer_orders
                    JOIN pizza_runner.runner_orders
                    USING(order_id)
                    WHERE cancellation IS NULL
                    GROUP BY customer_id;
   ![image](https://github.com/lolatunji/SQL-PROJECT-2-Pizza-Runner/assets/108012164/5bd578d0-b903-4f90-a3fc-ff8af90f0ce8)

8. How many pizzas were delivered that had both exclusions and extras?
   
                  SELECT COUNT(*)
                  FROM pizza_runner.customer_orders
                  JOIN pizza_runner.runner_orders
                  USING(order_id)
                  WHERE cancellation IS NULL AND extras IS NOT NULL 
                 AND exclusions IS NOT NULL;
   ![image](https://github.com/lolatunji/SQL-PROJECT-2-Pizza-Runner/assets/108012164/31aaea0b-5cc3-4388-b32b-88cce8fcf9ba)

9. What was the total volume of pizzas ordered for each hour of the day?
    
                    SELECT COUNT(pizza_id) AS volume_of_order, EXTRACT(HOUR FROM order_time) AS hour_ofthe_day
                    FROM pizza_runner.customer_orders
                    GROUP BY hour_ofthe_day
                    ORDER BY hour_ofthe_day;
  ![image](https://github.com/lolatunji/SQL-PROJECT-2-Pizza-Runner/assets/108012164/11d6fe35-014a-4a7c-8adc-2a8e27bb8baa)

10. What was the volume of orders for each day of the week?
    
                    SELECT COUNT(order_id) AS order_volume, TO_CHAR( order_time, 'Day') AS day_ofthe_week
                    FROM pizza_runner.customer_orders
                    GROUP BY 2
                    ORDER BY 1;
    ![image](https://github.com/lolatunji/SQL-PROJECT-2-Pizza-Runner/assets/108012164/7f31d3e9-2353-4ee2-8239-b90eb9d66df9)


# INSIGHTS AND RECOMMENDATIONS:

1. Customers prefer Meatlovers pizza to Vegetarian pizza with a total of 9 Meatlovers delivered. To increase the sales of Vegetarian pizza the marketing team should put effort to market the product.

2. The highest single order delivered is three (3). To increase the number of single orders, a discount should be offered to customers 5 single orders and upward.

3. Pizza orders increase at the late hour of the day. Runners should be available, especially at late hours to deliver pizza faster.
   
 


   
