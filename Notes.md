## Window Functions [https://zhuanlan.zhihu.com/p/92654574]
```
<窗口函数> over (partition by <用于分组的列名>
                order by <用于排序的列名>)
```

### DENSE_RANK() 
Dense_Rank: 创造新列，为order排名，且连续（eg. 1, 2, 2, 3而非4）
Rank: 非连续, eg. 1, 2, 2, 4
Row_Number: 按row前后顺序排列，eg. 1, 2, 3, 4
```
DENSE_RANK() OVER (
    PARTITION BY <expression>[{,<expression>...}]
    ORDER BY <expression> [ASC|DESC], [{,<expression>...}]
) 
```
#### Question 184. Department Highest Salary
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

### lag() lead() lag向前，lead向后
```
LAG语法：LAG(<expression>[,offset[, default_value]]) OVER ( PARTITION BY expr,... ORDER BY expr [ASC|DESC],... )

LEAD语法：LEAD(<expression>[,offset[, default_value]]) OVER ( PARTITION BY (expr) ORDER BY (expr))
```

## Notes
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
2. 
