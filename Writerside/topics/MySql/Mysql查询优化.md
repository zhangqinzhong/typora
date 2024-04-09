

例如：

```mysql
-- EXPLAIN EXTENDED
SELECT
	*
FROM
	box
WHERE
	box.create_time >= '2021-11-02 00:00:00' 
	AND box.create_time <= '2021-11-25 23:59:59' 
GROUP BY
	box.number;
-- 	SHOW WARNINGS
```

Using where;

条件查询

Using index; 

索引下推

Using temporary; 

临时表

Using filesort

文件排序

![image-20220920160407957](image-20220920160407957.png)



select_type： select子句的查询类型

table：从上到下，代表SQL优化器选择的表join顺序

type：索引类型

possible_keys：有助于高效查找行的索引

key：出于最小化查询成本考虑，SQL优化器实际使用的索引

key_len：MySQL在索引里使用的字节数

ref：之前的表在key列记录的索引中查找值所用的列或常量

rows：MySQL读取行数的估计值

filtered：过滤

extra：sql解析的额外信息，当出现using index时，表示sql使用覆盖索引，性能较好，而当出现using filesort、using temporary、using where时，查询需要优化
