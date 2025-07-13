# SQL 50 - LeetCode

Solutions for [SQL 50 Study Plan](https://leetcode.com/studyplan/top-sql-50/) on LeetCode (PostgreSQL)

---

## Select

[1757. Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/description/)

```sql
-- Write your PostgreSQL query statement below
select product_id from products 
where low_fats = 'Y' and recyclable = 'Y';
```

---

[584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select name from Customer
where referee_id != 2 or referee_id is null;
```

---
[595. Big Countries](https://leetcode.com/problems/big-countries/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select name, 
population, 
area from world
    where area >= 3000000 or population >= 25000000;
```

---
[1148. Article Views I](https://leetcode.com/problems/article-views-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select distinct author_id as id from views
    where author_id = viewer_id
    order by author_id asc;
```

---

[1683. Invalid Tweets](https://leetcode.com/problems/invalid-tweets/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select tweet_id from tweets
    where length(content) > 15;
```

---

## Basic Joins

[1378. Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select unique_id, 
name from employees 
left join employeeUNI on 
    employees.id = employeeUNI.id;
```

---

[1068. Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select product_name, 
year, 
price from sales 
join product on 
sales.product_id = product.product_id
```

---

[1581. Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select 
customer_id, 
count(*) as 
count_no_trans from visits
left join transactions on 
visits.visit_id = transactions.visit_id 
where transaction_id is null 
group by customer_id;
```

---
[197. Rising Temperature](https://leetcode.com/problems/rising-temperature/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select w1.id 
from weather w1
join weather w2
on w1.recordDate = w2.recordDate + interval '1 day'
where w2.temperature < w1.temperature;
```

---

[1661. Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT
  s.machine_id, ROUND(AVG(e.timestamp- s.timestamp)::numeric, 3) AS processing_time
FROM Activity AS s
JOIN Activity AS e
  ON s.machine_id = e.machine_id
 AND s.process_id = e.process_id
 where s.activity_type = 'start'
 AND e.activity_type = 'end'
GROUP BY s.machine_id;
```

---

[577. Employee Bonus](https://leetcode.com/problems/employee-bonus/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select e.name, b.bonus 
from employee e 
left join bonus b 
on e.empId = b.empId
where b.bonus < 1000 or b.bonus is null;
```

---

[1280. Students and Examinations](https://leetcode.com/problems/students-and-examinations/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    students.student_id,
    student_name, 
    subjects.subject_name,
    COUNT(examinations.subject_name) AS attended_exams
FROM 
    students 
CROSS JOIN 
    subjects 
LEFT JOIN 
    examinations 
    ON students.student_id = examinations.student_id 
    AND examinations.subject_name = subjects.subject_name
GROUP BY 
    students.student_id,
    student_name, 
    subjects.subject_name
ORDER BY 
    students.student_id,
    subjects.subject_name;

```

---
[570. Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
-- Write your PostgreSQL query statement below
-- create view manager_id as
/* select employee.name from employee where employee.id IN
    (select managerId from employee 
    Group by managerId
    having count(employee.managerId)>=5
    ); */
   

select employee.name from employee join
    (select managerId from employee 
    Group by managerId
    having count(employee.managerId)>=5
    ) as manager_id on employee.id = manager_id.managerId;
```

---
[1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
-- Write your PostgreSQL query statement below
SELECT 
    user_id, 
    ROUND(
        CASE 
            WHEN COUNT(c.action) = 0 THEN 0
            ELSE 
                SUM(CASE WHEN c.action = 'confirmed' THEN 1 ELSE 0 END)::DECIMAL 
                / COUNT(c.action) 
        END, 
        2
    ) AS confirmation_rate
FROM 
    signups s
LEFT JOIN 
    confirmations c 
    USING (user_id)
GROUP BY 
    s.user_id;

```

## Basic Aggregation functions

[620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select * from cinema
where (id % 2) != 0 and
description != 'boring'
order by rating desc
```

---

[1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
-- Write your PostgreSQL query statement below
-- select product_id, sum(units*price)/sum(units)::decimal as average_price from prices
-- select product_id, sum(units*price)::decimal as average_price from prices
SELECT 
    p.product_id, 
    COALESCE(
        ROUND(SUM(u.units * p.price) / SUM(u.units)::DECIMAL, 2), 
        0
    ) AS average_price
FROM 
    prices p
LEFT JOIN 
    unitsSold u 
    ON p.product_id = u.product_id 
    AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY 
    p.product_id;

 ```

[1075. Project Employees I](https://leetcode.com/problems/project-employees-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
select 
  project_id, 
  round(
    avg(employee.experience_years), 
    2
  ) as average_years 
from 
  project 
  join employee on project.employee_id = employee.employee_id 
group by 
  project_id;
```
