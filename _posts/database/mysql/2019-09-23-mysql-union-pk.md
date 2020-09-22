---
title: Mysql使用联合主键时，每个主键字段都能使用索引吗？
author: Old Seed
date: 2019-09-23 16:34:00 +0800
categories: [数据库,mysql]
tags: [mysql]
toc: true

---



对于这个问题，很多mysql的初学者都是搞不清楚的，今天作者做了一个实验，来验证这个问题，防止在应用环境中不当的使用索引，导致mysql性能下降。


### 实验环境：
- mysql 5.7
-  InnoDB引擎

### 表结构：
用两个测试表做实验，testuser1:
```sql
CREATE TABLE `testuser1` (
	`id` INT(11) NOT NULL,
	`name` VARCHAR(50) NULL DEFAULT NULL COLLATE 'utf8_bin',
	`age` INT(11) NOT NULL,
	`gender` VARCHAR(50) NULL DEFAULT NULL COLLATE 'utf8_bin',
	PRIMARY KEY (`id`, `age`)
)
COLLATE='utf8_bin'
ENGINE=InnoDB
;
```
testuser2:
```sql
CREATE TABLE `testuser2` (
	`id` INT(11) NOT NULL,
	`name` VARCHAR(50) NULL DEFAULT NULL COLLATE 'utf8_bin',
	`age` INT(11) NOT NULL,
	`gender` VARCHAR(50) NULL DEFAULT NULL COLLATE 'utf8_bin',
	PRIMARY KEY (`age`, `id`)
)
COLLATE='utf8_bin'
ENGINE=InnoDB
;
```

可以看到，两个表的字段顺序是一样的，也同样使用了联合主键，id和age两个字段，不同的是testuser1使用的联合主键顺序是id在前age在后，testuser2是age在前id在后。

这里使用explain语句来看索引的使用情况

![ ](/media/shang/extdata/old-seed-blog/assets/img/20190108132101106.png)

![ ](/media/shang/extdata/old-seed-blog/assets/img/20190108132149873.png)

可以看到对于testuser1表来说。用id字段查询的时候用到了主键索引，用age字段查询的时候没有用到索引

对于testuser2表来说，用id字段查询的时候没有用到主键索引，用age字段查询的时候用到了主键索引。

### 结论
这就说明，在mysql中，主键索引的顺序很重要，只有在第一位的字段在查询时才可以用到索引，后面的字段都用不到索引。如果要频繁查询，需要另外建立索引。