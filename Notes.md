## 1. 语法
### 1.1 Select
### 1.2 Delete
#### Question 196. Delete Duplicate Emails
```
delete from person where id not in 
(select * from (
select min(id) from person group by email) as p)
```

### 1.3 order by 可使用聚合函数
#### Question 586. Customer Placing the Largest Number of Orders
```
select customer_number
from Orders
group by customer_number 
order by count(*) desc
limit 1
```
## 2. Functions
### 2.1 SQL Aggregate 函数
SQL Aggregate 函数计算从列中取得的值，返回一个单一的值。
有用的 Aggregate 函数：

AVG() - 返回平均值
COUNT() - 返回行数
FIRST() - 返回第一个记录的值
LAST() - 返回最后一个记录的值
MAX() - 返回最大值
MIN() - 返回最小值
SUM() - 返回总和

### 2.2 Date
SELECT DATEDIFF(year, '2017/08/25', '2011/08/25') AS DateDiff;
SELECT DATEADD(year, 1, '2017/08/25') AS DateAdd; 增加year, day, month
SELECT DAY('2017/08/25') AS DayOfMonth;
SELECT GETDATE();
ISDATE(expression)
##### Question 197. Rising Temperature
```
select distinct b.id
from Weather a, Weather b
where b.recordDate = ADDDATE(a.recordDate, 1) and b.Temperature > a.Temperature
```

#### Case when
```
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;
```

### 3. Window Functions [https://zhuanlan.zhihu.com/p/92654574]
```
<窗口函数> over (partition by <用于分组的列名>
                order by <用于排序的列名>)
```

#### 3.1 DENSE_RANK() 
Dense_Rank: 创造新列，为order排名，且连续（eg. 1, 2, 2, 3而非4）
Rank: 非连续, eg. 1, 2, 2, 4
Row_Number: 按row前后顺序排列，eg. 1, 2, 3, 4
```
DENSE_RANK() OVER (
    PARTITION BY <expression>[{,<expression>...}]
    ORDER BY <expression> [ASC|DESC], [{,<expression>...}]
) 
```
##### Question 184. Department Highest Salary
```
select 
    d.name as 'Department',
    e.Name as 'Employee',
    e.salary
from
    (
    select
        *, 
        (dense_rank() over(
        partition by DepartmentId
        order by Salary desc)) as 'Rank'
    from
        Employee
    ) as e 
    right join 
    Department as d
    on e.DepartmentId = d.Id
where 
    e.rank = 1
```
**Better Solution**
```
# Write your MySQL query statement below
SELECT d.Name Department,e.Name Employee,e.Salary Salary
FROM Employee e
JOIN Department d
ON e.DepartmentId=d.id
WHERE (e.DepartmentId,e.salary) IN (SELECT e.Departmentid,MAX(e.Salary) FROM Employee e GROUP BY e.DepartmentId);
```

#### 3.2 lag() lead() lag向前，lead向后
```
LAG语法：LAG(<expression>[,offset[, default_value]]) OVER ( PARTITION BY expr,... ORDER BY expr [ASC|DESC],... )

LEAD语法：LEAD(<expression>[,offset[, default_value]]) OVER ( PARTITION BY (expr) ORDER BY (expr))

```

#### 3.3 sum()

## 4. Join
join/inner join: 可以用作self join
seat_id	free	seat_id	free
```
select a.seat_id, a.free, b.seat_id, b.free
from cinema a join cinema b;
```
```
1	1	1	1
2	0	1	1
3	1	1	1
4	1	1	1
5	1	1	1
1	1	2	0
2	0	2	0
3	1	2	0
4	1	2	0
5	1	2	0
1	1	3	1
2	0	3	1
3	1	3	1
4	1	3	1
5	1	3	1
1	1	4	1
2	0	4	1
3	1	4	1
4	1	4	1
5	1	4	1
1	1	5	1
2	0	5	1
3	1	5	1
4	1	5	1
5	1	5	1
```
left join
right join
outer join


## * Notes
1. 单引号双引号，mysql都可以用。使用双字符:
插入时          库中
'aa''b''cc'     aa'b'cc
"aa"b""cc"      aa"b"cc

使用转义字符(\):
插入时          库中
'aa\'b\'cc'     aa'b'cc
"aa\"b\"cc"     aa"b"cc

在单引号包裹的字符串中使用双引号、在双引号包裹的字符串中使用单引号 不需要使用双引号或转义字符。
插入时          库中
"aa'b'cc"       aa'b'cc
'aa"b"cc'       aa"b"cc
2. distinct: 非重复值，可用在变量之前。只可用于select，不可用于delete，insert，update等语句
#### Question 596. Classes More Than 5 Students
```
select class
from courses
group by class
having count(distinct student) >= 5
```
3. 大部分需要group by 
