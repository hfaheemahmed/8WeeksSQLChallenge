# 8WeeksSQLChallenge

#1. How many pizzas were ordered?
```sql
select count(order_id) PizzasOrdered from customer_orders;
```
#2. How many unique customer orders were made?
```sql
select count(distinct order_id) UniqueOrders from customer_orders;
```
#3. How many successful orders were delivered by each runner?
```sql
select runner_id,count(1) SuccessfulOrders from runner_orders where cancellation is null
group by runner_id
```
#4. How many of each type of pizza was delivered?
```sql
select pizza_name,count(c.order_id) PizzasDelivered from pizza_names p join customer_orders c on p.pizza_id=c.pizza_id
join runner_orders r on c.order_id=r.order_id
where r.cancellation is null
group by pizza_name
```
#5. How many Vegetarian and Meatlovers were ordered by each customer?
```sql
select c.customer_id, sum(case when pizza_name='Meatlovers' then 1 else 0 end) MeatLovers,
sum(case when pizza_name='Vegetarian' then 1 else 0 end) Vegetarian
from pizza_names p join customer_orders c on p.pizza_id=c.pizza_id
join runner_orders r on c.order_id=r.order_id
group by c.customer_id
order by 1

#6. What was the maximum number of pizzas delivered in a single order?
```sql
select top 1 c.order_id,count(*) NoOfPizzasDelivered from customer_orders c join runner_orders r
on c.order_id=r.order_id
where cancellation is null 
group by c.order_id
order by 2 desc
```
#7. For each customer,how many delivered pizzas had at least 1 change and how many had no changes?
```sql
select c.customer_id,sum(case when exclusions is not null or extras is not null then 1 else 0 end) change,
					sum(case when exclusions is NULL and extras is null then 1 else 0 end) nochange
from customer_orders c join runner_orders r on c.order_id=r.order_id
where cancellation is null
group by c.customer_id
order by 1
```
#8. How many pizzas were delivered that had both exclusions and extras?
```sql
select count(pizza_id) Pizzas from customer_orders c join runner_orders r on c.order_id=r.order_id
and cancellation is null  
and exclusions is not null and extras is not null
```
#9. What was the total volume of pizzas ordered for each hour of the day?
```sql
select datepart(hour,order_time) HourofDay,FORMAT(order_time,'hh tt') HourAMPM,count(pizza_id) PizzaCount
from customer_orders
group by datepart(hour,order_time),FORMAT(order_time,'hh tt')
```
#10. What was the volume of orders for each day of the week?
```sql
select datename(WEEKDAY,order_time) Day,count(order_id) Volume from customer_orders
group by datename(WEEKDAY,order_time)
```
