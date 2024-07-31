GUIDE: https://www.sqltutorial.org/

# Viewing and Selecting Databases and Tables

```sql
USE DATABASE;
```

```sql
SHOW TABLES;
```

```sql
select * from employees;
```

---
## Size Per Database

Understanding the size of your databases can provide valuable insights into resource utilization and help you optimize performance. Use the following query to retrieve the size of each database on your MySQL server

```sql
SELECT 
    table_schema,
    COUNT(*) TABLES,
    CONCAT(ROUND(SUM(table_rows) / 1000000, 2), 'M') rows,
    CONCAT(ROUND(SUM(data_length) / (1024 * 1024 * 1024), 2), 'G') DATA,
    CONCAT(ROUND(SUM(index_length) / (1024 * 1024 * 1024), 2), 'G') idx,
    CONCAT(ROUND(SUM(data_length + index_length) / (1024 * 1024 * 1024), 2), 'G') total_size,
    ROUND(SUM(index_length) / SUM(data_length), 2) idxfrac
FROM
    information_schema.TABLES
GROUP BY table_schema;
```

---
## Size Per Table

### All Databases

To examine the size of tables across all databases, use the following query:

```sql
SELECT 
    table_schema AS `Database`,
    table_name AS `Table`,
    ROUND(((data_length + index_length) / 1024 / 1024), 2) `Size in MB`
FROM
    information_schema.TABLES
ORDER BY (data_length + index_length) DESC;

```

This query retrieves the size of each table in megabytes and sorts the results in descending order by total size.

### Single Database

If you’re interested in the size of tables within a specific database, modify the query to filter by the desired database name:

```sql
SELECT 
    table_schema AS `Database`,
    table_name AS `Table`,
    ROUND(((data_length + index_length) / 1024 / 1024), 2) `Size in MB`
FROM
    information_schema.TABLES
WHERE
    table_schema = "your_database_name_here"
ORDER BY (data_length + index_length) DESC;
```

Replace `"your_database_name_here"` with the name of the database you want to inspect. This query provides insights into the size of tables within a single database, facilitating targeted optimization efforts.


### Optimized Pagination Using Variables

```sql
SET @row_number = 0;
SELECT *
FROM (
    SELECT *, (@row_number:=@row_number + 1) AS num
    FROM table_name
    ORDER BY column1
) AS ranked
WHERE num BETWEEN 51 AND 100;
```

This query demonstrates an optimized way to handle pagination in MySQL, particularly useful for very large datasets. By assigning row numbers to each row and filtering based on these numbers, you can efficiently retrieve a specific page of results without the performance hit of using LIMIT with a high offset.

### Recursive CTE for Hierarchical Data 

```sql
WITH RECURSIVE CategoryPath AS (
    SELECT categoryId, name, 1 AS depth
    FROM categories
    WHERE parentCategoryId IS NULL
    UNION ALL
    SELECT c.categoryId, CONCAT(cp.name, ' > ', c.name), depth+1
    FROM CategoryPath AS cp
    JOIN categories AS c ON cp.categoryId = c.parentCategoryId
)
SELECT * FROM CategoryPath;
```

This query uses a recursive Common Table Expression (CTE) to handle hierarchical data, which can be useful for categories. It starts with the root categories (where parentCategoryId IS NULL) and recursively joins to child categories, building a path and computing the depth of each category in the hierarchy.

These MySQL tips and tricks offer practical solutions for monitoring database size and optimizing performance. Incorporate these queries into your workflow to streamline database management and ensure optimal resource utilization. Happy querying!