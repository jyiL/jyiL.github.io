---
layout: post
title: 数据库优化
description: ""
tags: [sample post]
image:
  background: triangular.png
---

1、 表结构的优化
    
        字段报小不要大


        每个字段都尽量加上NOT NULL

        合理给字段添加索引

```yaml
xxx：
	xxx
```
2、 引擎选择
    一般建议使用Innodb
Innodb与Myisam

    事务、
    锁：myiam只支持表锁：  表锁特点：读阻塞写、  写阻塞读、写


        innodb锁：行锁特点：只会影响到对应哪一行。   

    、速度、文件存放方式


3、 SQL语句优化

    select * from user where name = 'jack' 

    a.  不要使用*,只查需要的字段
    b.  给经常作为where条件的字段添加索引 
    c.  避免子查询
    d.  优化传统分页limit

                limit 1000000,10;

                where id > 1000000   limit 10;

    e. 字段可以适度冗余，减少多表联查，达到以空间换取时间目的

    f. 索引一定要使用到。
          f1.   or  左右两边的字段有应该有索引 
          f2.   like   "%amin"这种用不到索引 
          f3.   name = 123  //用不到索引
          f4.   字段使用函数，将不能用到索引


    g.  最先出现的条件，一定是过滤和排除掉更多结果的条件，第二出现的次
之。

        //select * from user where name = 'jack' and sex =1;//速度更快
        //select * from user where sex =1 and name = 'jack'//速度慢

    h. 多使用limit

    i. 尽量不要在数据库中做运算、减少MySQL内置函数

    j.  SQL关键字大写

    h.  使用预处理语句来操作SQL语句。

    $admin = 1;

    select * from user where name = $admin; 预处理 :先发送SQL模板，再发送数据1。  

    $admin = 2  //2 

    防SQL注入：先发送的模板，模板已经翻译。
              不会破坏SQL语句结构  

              PDO预处理真的可以防所有的SQL注入吗？不能！！！！

              具体：上http://stackoverflow.com/这个网站，搜索
              PDO SQL injection 
              那么你可以看到有一些PDO放不了注入。



4、配置文件优化

5、架构
    主从数据库
    优点：减少单库压力，加快查询速度、从库是主库备份、提高整个系统可靠性。






### 如何防止SQL注入
1. 使用PDO预处理功能
2. 过滤用户提交的数据。 mysql_real_escape_string()、addslashes()

3. 具体参考手册： 安全=》数据库安全
