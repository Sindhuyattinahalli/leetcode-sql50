# SQL 50 - LeetCode

Solutions for SQL 50 Study Plan on LeetCode

---

## [1757 - Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/)

```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y'
AND recyclable = 'Y';
```
## [1661 - Average Time For Process Per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/)
```sql
SELECT 
    a1.machine_id,
    ROUND(AVG(a2.timestamp - a1.timestamp), 3) AS processing_time
FROM Activity a1
JOIN Activity a2
ON a1.machine_id = a2.machine_id
AND a1.process_id = a2.process_id
WHERE a1.activity_type = 'start'
AND a2.activity_type = 'end'
GROUP BY a1.machine_id;
```

---

## [584 - Find Customer Referee](https://leetcode.com/problems/find-customer-referee/)

```sql
SELECT name
FROM Customer
WHERE referee_id != 2 OR referee_id IS NULL;
```
 ## [595 - Big Countries](https://leetcode.com/problems/big-countries/)

 ```sql
SELECT NAME,POPULATION,AREA FROM  WORLD
WHERE AREA >= 3000000
OR POPULATION >= 25000000;
```

## [1148 -  Article Views I](https://leetcode.com/problems/article-views-i/)

```sql
SELECT DISTINCT AUTHOR_ID AS ID FROM VIEWS
WHERE AUTHOR_ID = VIEWER_ID
ORDER BY AUTHOR_ID ASC; 
```

## [1683 - Invalid Tweets](https://leetcode.com/problems/invalid-tweets/)
```sql
SELECT TWEET_ID FROM TWEETS
WHERE LENGTH(CONTENT)>15;
```

## [1378 - Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/)
```sql
select unique_id ,name
from employees  emp
left join employeeUNI uni
on uni.id = emp.id;
```
## [1068 - Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/)
```sql
SELECT P.PRODUCT_NAME,
       S.YEAR,
       S.PRICE
       FROM PRODUCT P
       JOIN SALES S
       ON P.PRODUCT_ID = S.PRODUCT_ID;
```
## [197 - Rising Temperature](https://leetcode.com/problems/rising-temperature/)
```sql
SELECT W1.ID
FROM WEATHER W1,WEATHER W2
WHERE W1.RECORDDATE = W2.RECORDDATE+1
AND W1.TEMPERATURE > W2.TEMPERATURE;
```
## [620 - Not Boring Movies](https://leetcode.com/problems/not-boring-movies/)
```sql
SELECT * FROM CINEMA
WHERE MOD(ID,2) = 1  AND
DESCRIPTION !=  'boring'
ORDER BY RATING DESC;
```
## [577 - Employee Bonus](https://leetcode.com/problems/employee-bonus/)
```sql
SELECT E.NAME , B.BONUS
FROM EMPLOYEE E LEFT JOIN BONUS B
ON E.EMPID = B.EMPID 
WHERE B.BONUS<1000 OR  B.BONUS IS NULL;
```
## [1729 - Find Followers Count](https://leetcode.com/problems/find-followers-count/)
```sql
SELECT USER_ID,
COUNT(FOLLOWER_ID) AS FOLLOWERS_COUNT
FROM FOLLOWERS
GROUP BY USER_ID
ORDER BY USER_ID ASC;
```
## [1581 - Customer who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/)
```sql
SELECT v.customer_id,
       COUNT(v.visit_id) AS count_no_trans
FROM Visits v
LEFT JOIN Transactions t
ON v.visit_id = t.visit_id
WHERE t.transaction_id IS NULL
GROUP BY v.customer_id;
```

## [570-Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/)
```sql
SELECT name
FROM Employee
WHERE id IN (
    SELECT managerId
    FROM Employee
    GROUP BY managerId
    HAVING COUNT(managerId) >= 5
);
```

## [1484-Group-Sold-Products-By-The-Date](https://leetcode.com/problems/group-sold-products-by-the-date/)
```sql
SELECT TO_CHAR(SELL_DATE,'YYYY-MM-DD') AS SELL_DATE,
       COUNT(PRODUCT) AS NUM_SOLD,
       LISTAGG(PRODUCT, ',')
       WITHIN GROUP (ORDER BY PRODUCT) AS PRODUCTS
FROM
(
    SELECT DISTINCT SELL_DATE, PRODUCT
    FROM ACTIVITIES
)
GROUP BY SELL_DATE
ORDER BY SELL_DATE;
```
## [1075-Project Employees i](https://leetcode.com/problems/project-employees-i/)
```sql
SELECT P.PROJECT_ID, ROUND(AVG(E.EXPERIENCE_YEARS),2)AS AVERAGE_YEARS
FROM PROJECT P JOIN EMPLOYEE E
ON P.EMPLOYEE_ID = E.EMPLOYEE_ID
GROUP BY PROJECT_ID
ORDER BY PROJECT_ID;
```
## [1280-Students and Examinations](https://leetcode.com/problems/students-and-examinations/)
```sql
SELECT
    s.student_id,
    s.student_name,
    sub.subject_name,
    COUNT(e.subject_name) AS attended_exams
FROM Students s
CROSS JOIN Subjects sub
LEFT JOIN Examinations e
    ON s.student_id = e.student_id
   AND sub.subject_name = e.subject_name
GROUP BY
    s.student_id,
    s.student_name,
    sub.subject_name
ORDER BY
    s.student_id,
    sub.subject_name;
```

## [Confirmation Rate](https://leetcode.com/problems/confirmation-rate/)
```sql
SELECT
    s.user_id,
    ROUND(
        NVL(
            AVG(
                CASE
                    WHEN c.action = 'confirmed' THEN 1
                    WHEN c.action = 'timeout' THEN 0
                END
            ),
            0
        ),
        2
    ) AS confirmation_rate
FROM Signups s
LEFT JOIN Confirmations c
    ON s.user_id = c.user_id
GROUP BY s.user_id;
```

## [1211-Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/)
```sql
SELECT
    query_name,
    ROUND(AVG(rating * 1.0 / position), 2) AS quality,
    ROUND(
        AVG(
            CASE
                WHEN rating < 3 THEN 100
                ELSE 0
            END
        ),
        2
    ) AS poor_query_percentage
FROM Queries
WHERE query_name IS NOT NULL
GROUP BY query_name;
```

## [1633- Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest/)
```sql
SELECT
    r.contest_id,
    ROUND(
        COUNT(DISTINCT r.user_id) * 100 /
        (SELECT COUNT(*) FROM Users),
        2
    ) AS percentage
FROM Register r
GROUP BY r.contest_id
ORDER BY percentage DESC, r.contest_id ASC;
```


## [1193-Monthly Transaction I](https://leetcode.com/problems/monthly-transactions-i/)
```sql
SELECT
    TO_CHAR(trans_date, 'YYYY-MM') AS month,
    country,
    COUNT(*) AS trans_count,
    SUM(CASE
            WHEN state = 'approved' THEN 1
            ELSE 0
        END) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(CASE
            WHEN state = 'approved' THEN amount
            ELSE 0
        END) AS approved_total_amount
FROM Transactions
GROUP BY
    TO_CHAR(trans_date, 'YYYY-MM'),
    country
ORDER BY
    month,
    country;

```

## [1174-Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/)
```sql
SELECT
    ROUND(
        SUM(
            CASE
                WHEN order_date = customer_pref_delivery_date THEN 1
                ELSE 0
            END
        ) * 100 / COUNT(*),
        2
    ) AS immediate_percentage
FROM Delivery
WHERE (customer_id, order_date) IN (
    SELECT
        customer_id,
        MIN(order_date)
    FROM Delivery
    GROUP BY customer_id
);

```
##[550-Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/)
```sql
WITH first_login AS (
    SELECT
        player_id,
        MIN(event_date) AS first_date
    FROM Activity
    GROUP BY player_id
)
SELECT
    ROUND(
        AVG(
            CASE
                WHEN EXISTS (
                    SELECT 1
                    FROM Activity a
                    WHERE a.player_id = f.player_id
                      AND a.event_date = f.first_date + 1
                )
                THEN 1
                ELSE 0
            END
        ),
        2
    ) AS fraction
FROM first_login f;
```















