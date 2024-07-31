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

```bash
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

```bash
SELECT 
    table_schema AS `Database`,
    table_name AS `Table`,
    ROUND(((data_length + index_length) / 1024 / 1024), 2) `Size in MB`
FROM
    information_schema.TABLES
ORDER BY (data_length + index_length) DESC;

```

### Single Database

If you’re interested in the size of tables within a specific database, modify the query to filter by the desired database name: