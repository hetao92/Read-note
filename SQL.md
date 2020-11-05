### SELECT

检索

```mysql
SELECT ID 
FROM Products

SELECT DISTINCT xx	-- 检索不同的值
FROM Products

SELECT TOP 5 prod_name	-- 检索前 n 行数据
FROM Products;

SELECT prod_name -- MySQL 的语法
FROM Products
LIMIT m OFFSET n; -- 返回从第 n 行开始的 m 行数据 offset 初始为 0，也可以简写为 LIMIT n, m
```



### ORDER BY

排序

在指定一条 ORDER BY 子句时，应该保证它是 SELECT 语句中最后一 条子句。如果它不是最后的子句，将会出现错误消息。

```mysql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price, prod_name; -- 先以 price 排序，当 price 相同时再按 name 排序
ORDER BY xxx DESC 降序 -- 多个列时 DESC 关键字只应用到它前面的列明，默认是 ASC 升序
```





### WHERE

过滤