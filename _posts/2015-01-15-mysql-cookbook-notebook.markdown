---
layout: post
title:  "MySQL Cookbook读书笔记"
date:   2015-01-15 21:17:43
categories: mysql
---

##参数定义

假设数据库

	CREATE TABLE limbs (thing VARCHAR(20), legs INT, arms INT);
	INSERT INTO limbs (thing,legs,arms) VALUES('human',2,2);
	INSERT INTO limbs (thing,legs,arms) VALUES('insect',6,0);
	INSERT INTO limbs (thing,legs,arms) VALUES('squid',0,10);
	INSERT INTO limbs (thing,legs,arms) VALUES('octopus',0,8);
	INSERT INTO limbs (thing,legs,arms) VALUES('fish',0,0);
	INSERT INTO limbs (thing,legs,arms) VALUES('centipede',100,0);
	INSERT INTO limbs (thing,legs,arms) VALUES('table',4,0);
	INSERT INTO limbs (thing,legs,arms) VALUES('armchair',4,2);
	INSERT INTO limbs (thing,legs,arms) VALUES('phonograph',0,1);
	INSERT INTO limbs (thing,legs,arms) VALUES('tripod',3,0);
	INSERT INTO limbs (thing,legs,arms) VALUES('Peg Leg Pete',1,2);
	INSERT INTO limbs (thing,legs,arms) VALUES('space alien',NULL,NULL);

	select @max_limbs := max(arms+legs) from limbs;
	select * from limbs where arms+legs = @max_limbs;


如果statement返回多行，则最后一行被赋值给此参数
小结：
1. 格式定义：`@var_name := value`
2. 设置值：`SET @sum = 4 + 7`, 赋值采用`:=` or` =`都可以
3. 大小写不敏感
SET @x = 1, @X = 2; SELECT @x, @X;
```
+------+------+
| @x   | @X   |
+------+------+
|    2 |    2 |
+------+------+
```

## 显示连接进程id

    SELECT CONNECTION_ID()

## ENUM类型 SET类型(待继续研究)

    CREATE TABLE sizes (
        name ENUM('small', 'medium', 'large')
    ); 

## DATE_FROMAT函数

## NULL值
判断要使用`IS NULL`和`IS NOT NULL`

    SELECT *, IF(score IS NULL, 'unkown', score) FROM expt
    或者用下面这种更简洁的方式
    SELECT *, IFNULL(score, 'unkown') FROM expt;
即下面两种方式是等价的:
    IF(expr1 IS NOT NULL,expr1,expr2)
    IFNULL(expr1,expr2)














