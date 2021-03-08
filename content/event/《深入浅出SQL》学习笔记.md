---
title: "《深入浅出SQL》学习笔记"
date: 2021-03-08T04:45:24+08:00
---

1.使用 `CREATE DATABASE`语句来创建存储所有表的数据库

``` mysql
CREATE DATABASE website;
```

2.使用 `USE`语句进入特定数据库，然后才能在里面创建表

``` mysql
USE website;
```

3.使用 `CREATE TABLE`语句创建表，句中包含列名及其数据类型

``` mysql
CREATE TABLE introspect
(
url VARCHAR(20),
title VARCHAR(20),
email VARCHAR(50),
create_date DATE
);
```

4.使用 `DESC`语句检查表的结构

``` mysql
DESC introspect;
```

5.不可重建已经存在的表或数据库，可以删除然后重建，使用`DROP TABLE`语句能删除表，无论表里有无数据

``` mysql
DROP TABLE introspect;
```

6.常用数据格式

| 名称                | 描述                                                    |
| ------------------- | ------------------------------------------------------- |
| VARCHAR             | 可变长字符串，最大255个字符                             |
| CHAR或CHARACTER     | 固定长字符串，必须是设定好的长度                        |
| INT或INTERGER       | 整数                                                    |
| DEC                 | 提供数值空间，直到装满为止                              |
| DATE                | 日期                                                    |
| DATETIME或TIMESTAMP | DATETIME适合存储将来的时间，TIMESTAMP则通常记录当下时刻 |
| BLOB                | 大量文本数据                                            |

7.使用`INSERT INTO your_table(column_name1, column_name2, ...) VALUES('value1', 'value2', ...);`来对表插入值，其中字段名与值要一一对应，字符串要用单引号。改变列顺序是可以的；省略列名是可以的，但需要按顺序填入全部列的数据；省略部分列是可以的

```mysql
INSERT INTO introspect
(url, title, email, create_date)
VALUES
('lusuzi.github.io/introspect', 'introspect', 'letter_chat@outlook.com', '2021-03-06');
```

8.使用`SELECT xxx FROM my_table;`以查看表中的内容，其中*表示筛选所有列，如果要选择某些列，可以用逗号分隔

``` mysql
SELECT * FROM introspect
```

``` mysql
SELECT url, title FROM introspect
```

9.NULL表示未定义的值，只要在创建表时的数据类型后加`NOT NULL`就可以把对应列改为不接受NULL，但这样就不允许出现空值，否则会报错

``` mysql
CREATE TABLE introspect
(
url VARCHAR(20) NOT NULL,
title VARCHAR(20) NOT NULL,
email VARCHAR(50),
create_date DATE NOT NULL
);
```

10.DEFAULT表示默认值，根在`DEFAULT`关键字后的值会在每次新增记录时自动插入表中，只要没有另外指派其他值，当然默认值类型必须和列的类型相同

``` mysql
CREATE TABLE introspect
(
url VARCHAR(20) NOT NULL DEFAULT 'lusuzi.github.io/introspect',
title VARCHAR(20) NOT NULL,
email VARCHAR(50),
create_date DATE NOT NULL
);
```

11.使用`WHERE`子句为SELECT提供搜索的特定条件，只会返回符合条件的行

``` mysql
SELECT * From introspect
WHERE title='introspect';
```

12.在`WHERE`子句用`AND`可**并**多个条件，用`OR`可进行**或**多个条件；用`BETWEEN x AND y`来代表区间条件（闭区间），注意顺序很重要，表示从x到y

``` mysql
SELECT location FROM doughnut_ratings
WHERE type='plain glazed'
AND
rating=10;
```

``` mysql
SELECT drink_name FROM drink_info
WHERE
calories BETWEEN 30 AND 60;
```

13.比较符号可以对字符串进行比较，不过是以字母排序，如选出以L开头的所有饮料名字

``` mysql
SELECT drink_name
FROM drink_info
WHRER
drink_name >= 'L'
AND
drink_name < 'M';
```

14.无法直接选出NULL，但可用`IS NULL`来选择

``` mysql
SELECT drink_info
FROM drink_info
WHERE
calories IS NULL;
```

15.用`LIKE`进行模糊匹配，通配符%可代表**任意数量**的字符，通配符_代表**一个**字符

``` mysql
SELECT first_name FROM my_contacts
WHERE first_name LIKE '_im';
```

``` mysql
SELECT * FROM my_contacts
WHERE location LIKE '%CA';
```

16.用`IN`来进行集合性匹配，反之为`NOT IN`（`NOT`和可以和`BETWEEN`和`LIKE`一起使用，不过要将其放`WHERE`之后，`IN`也可以，不过`NOT IN`更常用；与`OR`和`AND`连用时将`NOT`直接放在它们后面）

``` mysql
SELECT date_name
FROM black_book
WHERE
rating IN ('innovative', 'fabulous',
          'delightful');
```

17.用`DELETE`删除符合要求的行，其选择方式几乎与`SELECT`完全相同；慎用`DELETE`，需确保选择是精确的，必要时先用`SELECT`确认一下

``` mysql
DELETE FROM clown_info
WHERE
activities='dancing';
```

18.删除表中的每一行

``` mysql
DELETE FROM your_table
```

19.用`UPDATE`来更新一行或多行的一列或多列（相当于`DELETE`和`INSERT`的组合），不加上`WHERE`就会为每一列都更改

``` mysql
UPDATE doughtnut_ratings
SET type='glazed'
WHERE type='plain glazed';
```

``` mysql
UPDATE your_table
SET first_column='new_value',
second_column='another_value'
WHERE A > 3;
```

20.`UPDATE`中的`SET`可以进行基础的数学运算

``` mysql
UPDATE drink_info
SET cost=cost+1
WHERE drink_name='BlueBerry';
```



> 注意事项：
>
> 1. 如果数字与字符型数字比较，字符型数字会被强制转化为数字进行比较
> 2. 在SQL中不要使用双引号
> 3. SQL中转义有两种方式，如在单引号前加\，或者再加一个单引号
> 4. SQL的等号是'='，不等号是'<>'



