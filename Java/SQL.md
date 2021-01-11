## MySQL基础语法

### DISTINCT关键字

只返回不同的值。

`SELECT DISTINCT vend_id FROM products;`

+ DISTINCT必须直接放在列名的前面
+ DISTINCT应用于所有列，而不仅是前置它的列
+ 除非指定的多列都相同，否则将返回所有行

### LIMIT关键字

返回前N行

`SELECT vend_id FROM products LIMIT 5;`

指定从行6开始读取3行。行号从0开始，行6就是第七行。

`SELECT vend_id FROM products LIMIT 6,3;`

更易于理解的语法

`SELECT vend_id FROM products LIMIT 3 OFFSET 6;`

### 排序

```
SELECT prod_id,prod_price,prod_name 
FROM products 
ORDER BY prod_id, prod_name;
```

+ 排序完全按所规定的顺序进行
+ 对多个列排序，列名之间逗号分隔
+ 仅在多个行具有相同的prod_id，才按prod_name排序；

默认升序。降序使用关键字DESC

```
SELECT prod_id,prod_price,prod_name 
FROM products 
ORDER BY prod_id DESC, prod_name;
```

+ DESC只应用到直接位于其前面的列名

ORDER BY子句必须位于FROM子句之后，LIMIT子句之前。