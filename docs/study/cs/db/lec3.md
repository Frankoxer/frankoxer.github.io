---
comments: true
---

# Lecture 3: SQL

SQL（Structured Query Language）是一种用于操作关系数据库的语言，包括 DDL、DML、DCL。

## 1 Data Definition Language

举例:

```sql
CREATE TABLE Students (
    ID INT PRIMARY KEY,
    Name VARCHAR(255),
    Age INT
);
```

SQL 中的数据类型：

* char(n)
* varchar(n)：可变长度字符串
* int
* smallint：小整数
* numeric(p, s)：精确数值，fixed-point，p 是总位数，s 是小数位数
* real：单精度浮点数
* double precision：双精度浮点数
* float(n)：浮点数，n 是精度
* NULL：空值
* date
* time
* timestamp

在创建表的时候可以添加一些约束条件（Integrity Constraints）：

* NOT NULL
* Primary Key (A1, A2, ..., An)
* Check(P)，这里的 P 是一个判断句

需要删除表的时候可以使用 drop 命令，例如：

```sql
DROP TABLE branch
```

The drop table command deletes all information about the dropped relation from the database. 使用的时候要小心哦~

对已经存在的表添加新的属性，可以使用 alter 语句，例如：

```sql
ALTER TABLE r ADD A D
alter table r add (A1 D1, A2 D2, ..., An Dn)
```

这里的 A 是属性名称，D 是 domain。**对于新加入的属性，所有行的这一属性被设置为 null**。

ALTER 语句不仅可以添加属性，还可以删除和修改：

```sql
alter table r DROP A
ALTER TABLE branch MODIFY (branch_name char(30), assets not null)
```

对于索引来说，也可使用 create 和 drop 语句：

```sql
create index b_index on branch (branch_name)
create unique index b_idnex on account (account_number) // specify a candidate key
drop index b_index
```

## 2 Basic Structure

这一节介绍以下内容:

* select
* where
* from
* rename
* string operations
* ordering
* duplicates

### 2.1 Select

典型的 select 语句（SQL 查询语句）长这样：

```sql
select A1, A2, ..., An
from r1, r2, ..., rm
where P
```

它等价于关系代数表达式：

$$\Pi_{A1, A2, ..., An}(\sigma_{P}(r_1 \times r_2 \times ... \times r_m))$$

其中 A 是属性，r 是关系表，P 是条件。执行的结果也是一个关系。

**注意**：SQL 不允许名字中带有连字符 ‘-’；SQL 关键字对大小写不敏感。

此外，SQL 允许关系中有重复，如果需要删除重复行，添加 distinct 关键字：

```sql
select distinct branch_name
from loan
```

distinct 的反义词是 all，这也是 SQL 的默认模式。

如果想选择表中的所有属性，用一个 * 号即可。此外，* 号也可表达算术运算：

```sql
select * from loan

select loan_number, branch_name, amount * 100
from loan
```

### 2.2 Where

where 后面接条件。这个语句可以包含多个条件，用 and、or 或是 not 来修饰连接。表达区间也可用 and 配合 between。

```sql
select loan_number
from loan
where amount between 9000 and 10000
```

### 2.3 From

from 后面列出与这条查询有关的表。如果 from 后面接了不止一张表，实际上在做笛卡尔积。例如：

```sql
select *
from borrower, loan
```

等价于关系代数表达式：

$$borrower \times loan$$

??? "举例：Find the customer name, loan number and loan amount of all customers having a loan at the Perryridge branch"

    loan(loan_number, branch_name, amount)
    
    borrower(customer_name, loan_number)

    ```sql
    select customer_name, borrower.loan_number, amount
    from borrower, loan
    where borrower.loan_number = loan.loan_number and
        branch_name = 'Perryridge'
    ```

    同名字的属性需要在前面打上 xxx. 以示区分。

### 2.4 Rename

重命名允许使用 as 关键字给属性或是关系来重命名：

```sql
select customer_name, borrower.loan_number as loan_id, amount
from borrower, loan
where borrower.loan_number = loan.loan_number
```

常用来做表达式简化：

```sql
SELECT customer_name, T.loan_number, S.amount 
FROM borrower as T, loan as S 
WHERE T.loan_number = S.loan_number 
```

这里先执行 from，所以就知道 T 和 S 是谁了。

同时对于**自身的比较**，需要重命名操作来区分：

```sql
SELECT distinct T.branch_name
FROM branch as T, branch as S
WHERE T.assets > S.assets and S.branch_city = ‘Brooklyn’ 
```

### 2.5 String Operations

与文件系统类似，我们可以对字符串使用通配符：

% 寻找子字符串，类似文件系统中的 *。
_ 下划线寻找单个字符，类似文件系统中的 ?。

执行模糊查询需要使用 like 关键字。例如需要寻找客户名字带“泽”的：

```sql
select customer_name
from customer
where customer_name like '%泽%'
```

此外还支持非常多的字符串操作，如下图：

![20240614091613](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240614091613.png)

### 2.6 Ordering

使用 order by 语句：

```sql
SELECT distinct customer_name
FROM borrower A, loan B 
WHERE A.loan_number = B.loan_number and 
      branch_name = ‘Perryridge’
order by customer_name
```

这里会按照字母表顺序排列。在 order by 语句最后加一个 desc 就是逆序。

还可以组合使用：

```sql
SELECT * FROM customer 
ORDER BY customer_city, customer_street desc, customer_name 
```

不说怎样排序的话就是默认 asc 正序。

### 2.7 Duplicates

如前面所述，想要避免重复就加上 distinct。

## 3 Set Operations

SQL 支持对几个集合进行交并差操作，使用 UNION、INTERSECT 和 EXCEPT 关键字。

**这些操作会自动消除重复行，如果需要保留，在这些关键字后面加 ALL**。

举例：

```sql
(SELECT customer_name FROM depositor)
EXCEPT
(SELECT customer_name FROM borrower)
```

## 4 Aggregate Functions

与上一节的关系代数中的合计函数相似。这些函数输入一个**列**，返回平均值等类似信息。

具体有:

* avg
* min
* max
* sum
* count

举例：

```sql
select avg(balance) avg_bal
from account
where branch_name = 'Perryridge'
```

注意这里不能写成：

```sql
select branch_name, avg(balance) avg_bal
from account
where branch_name = 'Perryridge'
```

这是因为选择语句中，合计函数外面的属性必须在 group by 语句中出现。因此可以修改为：

```sql
select branch_name, avg(balance) avg_bal
from account
group by branch_name
```

若要根据合计函数的值进行选择，可以使用 having 语句：

```sql
select A.branch_name, avg(balance)
from account A, branch B
where A.branch_name = B.branch_name
and branch_city = 'Brooklyn'
group by A.branch_name
having avg(balance) > 1200
```

总结一下 select 语句的格式：

```sql
select <[distinct] c1, c2, ...>
from <r1, ...>
where <condition>
group by <c1, c2, ...> having <condition>
order by <c1, c2, ...>
```

select 语句执行的顺序：

from -> where -> group(aggregate) -> having -> select -> distinct -> order by.

注意：

* having 语句的判断条件是在 group by 之后的，而 where 是在 group by 之前的。
* 合计函数不能在 where 语句中使用，因为 where 是在 group by 之前的。

## 5 Null Values

注意不能写 amount = null，而应该写 amount is null。

此外合计函数会自动忽略 null 值。

## 6 Nested Subqueries

嵌套语句常用来解决复杂的查询问题，一般是如下格式：

```sql
select A1, A2, ...
from r
where A in (select B1, B2, ...
            from s
            where condition)
```

举例：Find all customers who have both an account and a loan at the bank.

```sql
SELECT distinct customer_name
FROM borrower
WHERE customer_name in (SELECT customer_name
                        FROM depositor) 
```

Find the account_number with the maximum balance for every branch.

```sql
SELECT account_number AN, balance 
FROM account  A 
WHERE balance >= (SELECT max(balance) 
                    FROM account B 
                WHERE A.branch_name = B.branch_name) 
ORDER by balance 
```

这种自比较一般情况都需要使用重命名操作建立两张独立的相同表。

Find the names of all branches that have greater assets than all branches located in Brooklyn. 

```sql
SELECT branch_name 
FROM branch 
WHERE assets > all (SELECT assets 
                     FROM branch 
                     WHERE branch_city = ‘Brooklyn’) 
```

这里的 all 也可以换成 some，如果需求是大于其中任意一个。

## 7 Views

常被用来隐藏某些数据，~~如老师的工资~~，或是简化查询。创建视图、丢弃视图的语句是：

```sql
create view view_name as <query>
drop view view_name
```

举例：Create a view consisting of branches and their customer names.

```sql
CREAT view all_customer as 
         ((SELECT branch_name, customer_name 
          FROM depositor, account 
          WHERE depositor.account_number = account.account_number) 
       union       
         (SELECT branch_name, customer_name 
          FROM borrower, loan 
          WHERE borrower.loan_number = loan.loan_number)) 
```

## 8 Derived Relations

派生关系是在查询时生成的关系，不会被存储在数据库中。这种关系只在查询时存在。查询的时候可以将语句导出为一个派生关系，例如：

```sql
SELECT branch_name, avg_bal 
FROM (SELECT branch_name, avg(balance) 
        FROM account 
        GROUP BY branch_name) 
as result (branch_name, avg_bal) 
WHERE avg_bal > 500 
```

也可使用 with 语句来讲视图定义为本地而非全局：Find all accounts with the maximum balance.

```sql
WITH max_balance(value) as 
                SELECT max(balance) 
                FROM account 
SELECT account_number 
FROM account, max_balance 
WHERE account.balance = max_balance.value  
```

with 语句可以用来进行较为复杂的查询。

![41593ade3bb8f80fde6a9f134288365b](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/41593ade3bb8f80fde6a9f134288365b.png)