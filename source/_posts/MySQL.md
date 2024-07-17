---
title: MySQL面试题
declare: true
tags: [面试题, 数据库]
categories: 面试
---
# MySQL  
<!-- more -->  

## 一张数据表`Student`中查询第4到第6行数据的代码怎么写?  

```sql
SELECT * FROM Student LIMIT 3, 3;
```

## 左连接、内连接、全连接、右连接的区别?   
在SQL中，左连接、内连接、全连接和右连接是不同类型的表连接操作，它们用于将两个或多个表中的数据组合在一起。以下是它们之间的区别：

**1. 左连接 (LEFT JOIN):**
   - 左连接返回左边表（第一个表）的所有行，以及右边表（第二个表）中与左边表匹配的行。如果右边表中没有与左边表匹配的行，则会返回NULL值。  
   - 左连接是以左边表为基础的，因此无论右边表是否有匹配的行，左边表中的所有行都会包含在结果中。  
   - 左连接的语法为：`SELECT * FROM table1 LEFT JOIN table2 ON table1.column = table2.column`  

**2. 内连接 (INNER JOIN):**
   - 内连接返回两个表中匹配的行。即返回左边表和右边表中满足连接条件的行。
   - 只有当两个表中至少有一行满足连接条件时，才会返回结果。
   - 内连接的语法为：`SELECT * FROM table1 INNER JOIN table2 ON table1.column = table2.column`

**3. 全连接 (FULL OUTER JOIN):**
   - 全连接返回左边表和右边表中的所有行，并将它们组合在一起。如果某行在左边表中没有匹配项，则对应的右边表列值将为NULL；反之亦然。  
   - 即使在一个表中没有匹配项，也会将该表中的所有行包括在结果中。  
   - 全连接在某些数据库中可能不被支持，可以通过使用`UNION ALL` 和`LEFT JOIN` 以及`RIGHT JOIN` 的组合来模拟实现。  
   - 全连接的语法可以为：  
     ```sql
     SELECT * FROM table1 FULL OUTER JOIN table2 ON table1.column = table2.column
     ```
     或者：
     ```sql
     SELECT * FROM table1 LEFT JOIN table2 ON table1.column = table2.column
     UNION ALL
     SELECT * FROM table1 RIGHT JOIN table2 ON table1.column = table2.column
     ```

**4. 右连接 (RIGHT JOIN)**:
   - 右连接返回右边表（第二个表）的所有行，以及左边表（第一个表）中与右边表匹配的行。如果左边表中没有与右边表匹配的行，则会返回`NULL` 值。  
   - 右连接是以右边表为基础的，因此无论左边表是否有匹配的行，右边表中的所有行都会包含在结果中。  
   - 右连接的语法为：`SELECT * FROM table1 RIGHT JOIN table2 ON table1.column = table2.column`  

总的来说，左连接、内连接、全连接和右连接的区别在于它们返回结果的方式以及对待表中没有匹配项的方式。选择哪种连接取决于数据之间的关系以及所需的结果。

## 获取系统当前时间  
使用 **`NOW()`** 函数来获取系统的当前日期和时间。以下是一个简单的示例：  
```sql
SELECT NOW() AS current_datetime;
```

这将返回一个包含当前日期和时间的结果集，类似于以下格式：
```diff
+---------------------+
| current_datetime    |
+---------------------+
| 2022-02-23 12:34:56 |
+---------------------+
```

你还可以使用 **`CURRENT_TIMESTAMP`** 函数，它等效于 **`NOW()`** ：
```sql
SELECT CURRENT_TIMESTAMP() AS current_datetime;
```

## 时间转换成字符串  
将时间转换为字符串通常使用 **`CAST`** 或 **`CONVERT`** 函数： 
```sql
SELECT CAST(NOW() AS CHAR) AS formatted_date;
```

或者使用 **`DATE_FORMAT`** 函数：  
```sql
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i:%s') AS formatted_date;
```

## MySQL的索引
MySQL索引是一种用于提高数据库查询性能的关键工具。它们是一种数据结构，通过存储特定列的值的快速引用，加速对表中数据的检索。索引可用于加速`SELECT` 、`UPDATE` 和`DELETE` 查询，但也会增加`INSERT` 和 `UPDATE` 操作的开销。

以下是一些关于MySQL索引的基本知识：

**1. B-Tree 索引**：MySQL默认使用的索引类型是B-Tree（平衡树）索引。B-Tree索引适用于各种不同的查询操作，如精确匹配、范围查询和排序。
**2. 唯一索引**：唯一索引确保了列中的值是唯一的，即不允许重复值。这在避免数据重复和维护数据完整性方面非常有用。
**3. 主键索引**：主键索引是一种唯一索引，但它还具有特殊性，即被用作表的主键。主键索引确保了表中每一行的唯一性，并且不允许`NULL` 值。
**4. 复合索引**：复合索引是指在多个列上创建的索引。当查询涉及到复合索引的所有列时，MySQL可以更有效地使用该索引。
**5. 全文索引**：全文索引允许在文本列上进行全文搜索。这种类型的索引对于需要在大量文本数据中进行搜索的应用程序非常有用。
**6. 空间索引**：空间索引用于地理数据类型，允许对空间数据进行高效查询。
**7. 索引优化**：在设计索引时，需要权衡查询性能和索引维护成本。添加过多的索引可能会降低写操作的性能，因为每次修改表时都需要更新索引。
**8. 索引优化器**：MySQL具有查询优化器，它会根据查询和可用索引的情况选择最佳执行计划。有时需要手动调整查询或索引设计来优化查询性能。
**9. 查询分析器**：MySQL提供了工具来分析查询执行计划，以便识别潜在的性能瓶颈和优化查询。
**10. 索引覆盖**：当查询只需要索引中的数据而不需要访问表中的实际行数据时，MySQL可以利用索引覆盖来提高查询性能。

总的来说，良好设计的索引可以显著提高MySQL数据库的性能，但需要注意避免过度索引以及定期优化索引以适应数据访问模式的变化。  

## SELECT和GROUP BY之间的限制  
所有select字段，都必须出现在group by中，例外：
1. 该字段为主键  
2. 该字段包含在聚合函数中

示例一：
```sql
select id, name from employee group by name;
```
`id` 为主键，符合  

示例二：  
```sql
select name, sum(salary) from employee group by name;
```
`salary` 在聚合函数中，符合

示例三：
```sql
select name,age, sum(salary) from employee group by name;
```
`group by` 字段没有包含`age` ， `age` 不是主键，不符合

那`select *`和`group by` 子句如何搭配使用？

把分组结果当成一个表

示例四：
```sql
select * from table1, (select id as t from table2 group by id) m where table 1.id=m.t；
```
