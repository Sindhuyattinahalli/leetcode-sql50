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














