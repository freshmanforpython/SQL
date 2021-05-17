## Window Functions [https://zhuanlan.zhihu.com/p/92654574]
### DENSE_RANK() 创造新列，为order排名，且连续（eg. 1, 2, 2, 3而非4）
```
DENSE_RANK() OVER (
    PARTITION BY <expression>[{,<expression>...}]
    ORDER BY <expression> [ASC|DESC], [{,<expression>...}]
) 
```
