# leetcode-sql50

Solutions for SQL 50 Study Plan on LeetCode

---

## [1757 - Recyclable andLowFatProducts(https://leetcode.com/problems/recyclable-and-low-fat-products/)

```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y'
AND recyclable = 'Y';
