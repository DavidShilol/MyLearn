# MySQL基础语法

## 检索数据

### DISTINCT关键字

只返回不同的值。

`SELECT DISTINCT vend_id FROM products;`

+ DISTINCT必须直接放在列名的前面
+ DISTINCT应用于所有列，而不仅是前置它的列
+ 除非指定的多列都相同，否则将返回所有行

### LIMIT关键字

返回前N行

`SELECT vend_id FROM products LIMIT 5;`

指定从行6开始读取3行。行号从0开始，行6就是第七行。

`SELECT vend_id FROM products LIMIT 6,3;`

更易于理解的语法

`SELECT vend_id FROM products LIMIT 3 OFFSET 6;`

## 排序检索数据

### 排序

```SQL
SELECT prod_id,prod_price,prod_name 
FROM products 
ORDER BY prod_id, prod_name;
```

+ 排序完全按所规定的顺序进行
+ 对多个列排序，列名之间逗号分隔
+ 仅在多个行具有相同的prod_id，才按prod_name排序；

默认升序。降序使用关键字DESC

```SQL
SELECT prod_id,prod_price,prod_name 
FROM products 
ORDER BY prod_id DESC, prod_name;
```

+ DESC只应用到直接位于其前面的列名

ORDER BY子句必须位于FROM子句之后，LIMIT子句之前。

## 过滤数据

+ WHERE子句中指定搜索条件
+ WHERE必须位于FROM之后，ORDER BY之前

### BETWEEN操作符
+ 匹配范围中的所有值，包括开始值和结束值
+ 低端值和高端值之间必须用AND关键字隔开

### 空值检查
NULL，无值。

判断空值的WHERE子句：`WHERE prod_price is NULL`。

过滤选择出不具有特定值的行时，无法返回具有NULL值的行。
```SQL
SELECT cust_id
FROM customers
WHERE cust_money != 100;
```
无法返回cust_money为NULL的行，因为NULL未知具有特殊含义，数据库不知道它们是否匹配。

## 数据过滤
### 操作符
用来联结或改变WHERE子句中的子句的关键字。也称为逻辑操作符。

### AND操作符
指示检索满足所有给定条件的行。

### OR操作符
指示检索匹配任一条件的行。

### IN操作符
WHERE子句中用来指定要匹配值的清单的关键字。

```SQL
SELECT prod_name, prod_price
FROM vend_id IN (1002,1003)
ORDER BY prod_name;
```

+ IN语法更清晰
+ IN因为操作符少，计算次序更容易管理
+ 与OR相同的功能
+ IN操作符一般比OR操作符清单执行更快
+ IN的最大优点是可以包含其他SELECT子句

### NOT操作符
WHERE子句中用来否定后跟条件的关键字

支持使用NOT对IN、BETWEEN和EXISTS子句取反

## 通配符过滤
### LIKE操作符
通配符：匹配值的一部分的特殊字符
搜索模式：由字面值、通配符或两者组合构成的搜索条件

为了在搜索子句中使用通配符，必须使用LIKE操作符。
LIKE指示后跟的搜索模式利用通配符匹配。

### %通配符
任何字符出现任意次数。NULL除外。

### _通配符
只匹配单个字符。

### 通配符使用技巧
+ 其他操作符能达到目的，应该使用其他的操作符
+ 通配符置于搜索模式的开始处，搜索起来最慢