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

## [1934 -Confirmation Rate](https://leetcode.com/problems/confirmation-rate/)
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
## [550-Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/)
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


## [2356-Number of Unique Subjects Taught By Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/)
```sql
SELECT
    teacher_id,
    COUNT(DISTINCT subject_id) AS cnt
FROM Teacher
GROUP BY teacher_id;
```

## [1141-User Activity for the Past 30 Days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/)
```sql
SELECT
    TO_CHAR(activity_date, 'YYYY-MM-DD') AS day,
    COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date BETWEEN DATE '2019-06-28' AND DATE '2019-07-27'
GROUP BY activity_date
ORDER BY activity_date;

```

## [1070- Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii/)
```sql
/* Write your PL/SQL query statement below */
SELECT
    product_id,
    year AS first_year,
    quantity,
    price
FROM Sales s
WHERE year = (
    SELECT MIN(year)
    FROM Sales
    WHERE product_id = s.product_id
);
```
## [596-Classes With at Least 5 Students](https://leetcode.com/problems/classes-with-at-least-5-students/)
```sql
SELECT
    class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5;

```

## [619-Biggest Single Number](https://leetcode.com/problems/biggest-single-number/)
```sql
SELECT
    MAX(num) AS num
FROM MyNumbers
WHERE num IN (
    SELECT num
    FROM MyNumbers
    GROUP BY num
    HAVING COUNT(*) = 1
);
```

## [1045- Customers Who Bought All Products](https://leetcode.com/problems/customers-who-bought-all-products/)
```sql
SELECT
    customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (
    SELECT COUNT(*)
    FROM Product
);
```

## [1731-The Number Of Employees Which Report to Each Employee](https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee/)
```sql
ELECT
    e1.employee_id,
    e1.name,
    COUNT(e2.employee_id) AS reports_count,
    ROUND(AVG(e2.age), 0) AS average_age
FROM Employees e1
JOIN Employees e2
ON e1.employee_id = e2.reports_to
GROUP BY e1.employee_id, e1.name
ORDER BY e1.employee_id;

```

## [ 1789 - Primary Department For Each Employee](https://leetcode.com/problems/primary-department-for-each-employee/)
```sql
SELECT employee_id, department_id
FROM Employee
WHERE primary_flag = 'Y'

UNION

SELECT employee_id, MAX(department_id) AS department_id
FROM Employee
GROUP BY employee_id
HAVING COUNT(*) = 1;

```

## [619-Triangle Judgement](https://leetcode.com/problems/triangle-judgement/)
```sql
SELECT
    x,
    y,
    z,
    CASE
        WHEN x + y > z
         AND y + z > x
         AND x + z > y
        THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM Triangle;

```
## [180-Consecutive Numbers](https://leetcode.com/problems/consecutive-numbers/)
```sql
SELECT DISTINCT l1.num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2
ON l1.id = l2.id - 1
JOIN Logs l3
ON l2.id = l3.id - 1
WHERE l1.num = l2.num
AND l2.num = l3.num;
```
## [1164-Product Price at a Given Date](https://leetcode.com/problems/product-price-at-a-given-date/)
```sql
SELECT p.product_id,
       NVL(t.new_price, 10) AS price
FROM (SELECT DISTINCT product_id FROM Products) p
LEFT JOIN (
    SELECT product_id, new_price
    FROM (
        SELECT product_id,
               new_price,
               change_date,
               ROW_NUMBER() OVER (
                   PARTITION BY product_id
                   ORDER BY change_date DESC
               ) AS rn
        FROM Products
        WHERE change_date <= DATE '2019-08-16'
    )
    WHERE rn = 1
) t
ON p.product_id = t.product_id;
```

## [1204- Last Person to Fit In The Bus](https://leetcode.com/problems/last-person-to-fit-in-the-bus/)
```sql
SELECT person_name
FROM (
    SELECT person_name,
           turn,
           SUM(weight) OVER (ORDER BY turn) AS total_weight
    FROM Queue
)
WHERE total_weight <= 1000
AND turn = (
    SELECT MAX(turn)
    FROM (
        SELECT turn,
               SUM(weight) OVER (ORDER BY turn) AS total_weight
        FROM Queue
    )
    WHERE total_weight <= 1000
);
```

## [1907-Count Salary Category](https://leetcode.com/problems/count-salary-categories/)

```sql
SELECT 'Low Salary' AS category,
       COUNT(*) AS accounts_count
FROM Accounts
WHERE income < 20000

UNION ALL

SELECT 'Average Salary' AS category,
       COUNT(*) AS accounts_count
FROM Accounts
WHERE income BETWEEN 20000 AND 50000

UNION ALL

SELECT 'High Salary' AS category,
       COUNT(*) AS accounts_count
FROM Accounts
WHERE income > 50000;
```
## [1978-Employees Whose Manager Left the Company](https://leetcode.com/problems/employees-whose-manager-left-the-company/)

```sql
SELECT employee_id
FROM Employees
WHERE salary < 30000
  AND manager_id IS NOT NULL
  AND manager_id NOT IN (
      SELECT employee_id
      FROM Employees
  )
ORDER BY employee_id;
```

## [185-Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries/)
```sql
SELECT
    d.name AS Department,
    e.name AS Employee,
    e.salary AS Salary
FROM (
    SELECT
        id,
        name,
        salary,
        departmentId,
        DENSE_RANK() OVER (
            PARTITION BY departmentId
            ORDER BY salary DESC
        ) AS rnk
    FROM Employee
) e
JOIN Department d
ON e.departmentId = d.id
WHERE e.rnk <= 3;
```

## [585-Investements in 2016](https://leetcode.com/problems/investments-in-2016/)
```sql
SELECT  ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM Insurance
WHERE tiv_2015 IN (
    SELECT tiv_2015
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
)
AND (lat, lon) IN (
    SELECT lat, lon
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) = 1
);
```

## [602-Friend Requests II:Who Has The Most Friends](https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/)
```sql
WITH friends AS (
    SELECT requester_id AS id
    FROM RequestAccepted
    UNION ALL
    SELECT accepter_id
    FROM RequestAccepted
),
cnt AS (
    SELECT
        id,
        COUNT(*) AS num,
        RANK() OVER (ORDER BY COUNT(*) DESC) AS rnk
    FROM friends
    GROUP BY id
)
SELECT
    id,
    num
FROM cnt
WHERE rnk = 1;
```
## [1667-Fix Names in a Table](https://leetcode.com/problems/fix-names-in-a-table/)
```sql
SELECT
    user_id,
    UPPER(SUBSTR(LOWER(name), 1, 1)) ||
    SUBSTR(LOWER(name), 2) AS name
FROM Users
ORDER BY user_id;
```
## [1527-Patients With a Condition](https://leetcode.com/problems/patients-with-a-condition/)

```sql
SELECT
    patient_id,
    patient_name,
    conditions
FROM Patients
WHERE REGEXP_LIKE(conditions, '(^| )DIAB1');
```
## [196-Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails/)
```sql
DELETE FROM Person
WHERE id NOT IN (
    SELECT MIN(id)
    FROM Person
    GROUP BY email
);
```


## [1341-Movie Rating](https://leetcode.com/problems/movie-rating/)
```sql
SELECT name AS results
FROM (
    SELECT u.name
    FROM MovieRating mr
    JOIN Users u
    ON mr.user_id = u.user_id
    GROUP BY u.user_id, u.name
    ORDER BY COUNT(*) DESC, u.name
)
WHERE ROWNUM = 1

UNION ALL

SELECT title AS results
FROM (
    SELECT m.title
    FROM MovieRating mr
    JOIN Movies m
    ON mr.movie_id = m.movie_id
    WHERE TO_CHAR(mr.created_at, 'YYYY-MM') = '2020-02'
    GROUP BY m.movie_id, m.title
    ORDER BY AVG(mr.rating) DESC, m.title
)
WHERE ROWNUM = 1;
```
## [1378- Replace Employee ID With The Unique Identifier](
```sql
/* Write your PL/SQL query statement below */
SELECT
    eu.unique_id,
    e.name
FROM Employees e
LEFT JOIN EmployeeUNI eu
ON e.id = eu.id;
```
## [1251-Average Selling Price](https://leetcode.com/problems/average-selling-price/)
```sql
SELECT
    p.product_id,
    ROUND(
        NVL(SUM(u.units * p.price) / SUM(u.units), 0),
        2
    ) AS average_price
FROM Prices p
LEFT JOIN UnitsSold u
ON p.product_id = u.product_id
AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY p.product_id
ORDER BY p.product_id;
```
## [1371-List the Products Ordered in a Period](https://leetcode.com/problems/list-the-products-ordered-in-a-period/)
```sql
SELECT
    p.product_name,
    SUM(o.unit) AS unit
FROM Products p
JOIN Orders o
ON p.product_id = o.product_id
WHERE o.order_date BETWEEN DATE '2020-02-01' AND DATE '2020-02-29'
GROUP BY p.product_name
HAVING SUM(o.unit) >= 100;
```
## [1517-Find Users With Valid E-Mails](https://leetcode.com/problems/find-users-with-valid-e-mails/)
```sql
SELECT
    user_id,
    name,
    mail
FROM Users
WHERE REGEXP_LIKE(
    mail,
    '^[A-Za-z][A-Za-z0-9_.-]*@leetcode\.com$'
);
```





