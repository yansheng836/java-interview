[TOC]

# MySQL索引的创建与使用

<https://blog.csdn.net/justry_deng/article/details/81458470>

索引有很多，且按不同的分类方式，又有很多种分类。不同的数据库，对索引的支持情况也不尽相同。

声明：本人主要简单示例MySQL中的**单列索引**、**组合索引**的创建与使用。

------

# 索引的创建：

### 建表时创建：

**CREATE TABLE 表名(**

**字段名 数据类型 [完整性约束条件],**

​       **……，**

**[UNIQUE | FULLTEXT | SPATIAL] INDEX | KEY**

**[索引名](字段名1 [(长度)] [ASC | DESC]) [USING 索引方法]**

**);**

说明：

- UNIQUE:可选。表示索引为唯一性索引。
- FULLTEXT:可选。表示索引为全文索引。
- SPATIAL:可选。表示索引为空间索引。
- INDEX和KEY:用于指定字段为索引，两者选择其中之一就可以了，作用是    一样的。
- 索引名:可选。给创建的索引取一个新名称。
- 字段名1:指定索引对应的字段的名称，该字段必须是前面定义好的字段。
- 长度:可选。指索引的长度，必须是字符串类型才可以使用。
- ASC:可选。表示升序排列。
- DESC:可选。表示降序排列。

注：索引方法默认使用B+TREE。

单列索引(示例)：

```sql
CREATE TABLE projectfile (



	id INT AUTO_INCREMENT COMMENT '附件id',



	fileuploadercode VARCHAR(128) COMMENT '附件上传者code',



	projectid INT COMMENT '项目id;此列受project表中的id列约束',



	filename VARCHAR (512) COMMENT '附件名',



	fileurl VARCHAR (512) COMMENT '附件下载地址',



	filesize BIGINT COMMENT '附件大小，单位Byte',



	-- 主键本身也是一种索引（注:也可以在上面的创建字段时使该字段主键自增）



        PRIMARY KEY (id),



	-- 主外键约束（注:project表中的id字段约束了此表中的projectid字段）



	FOREIGN KEY (projectid) REFERENCES project (id),



	-- 给projectid字段创建了唯一索引(注:也可以在上面的创建字段时使用unique来创建唯一索引)



	UNIQUE INDEX (projectid),



	-- 给fileuploadercode字段创建普通索引



	INDEX (fileuploadercode)



	-- 指定使用INNODB存储引擎(该引擎支持事务)、utf8字符编码



) ENGINE = INNODB DEFAULT CHARSET = utf8 COMMENT '项目附件表';
```

注：这里只为示例如何创建索引，其他的合理性之类的先放一边。

组合索引(示例)：

```sql
CREATE TABLE projectfile (



	id INT AUTO_INCREMENT COMMENT '附件id',



	fileuploadercode VARCHAR(128) COMMENT '附件上传者code',



	projectid INT COMMENT '项目id;此列受project表中的id列约束',



	filename VARCHAR (512) COMMENT '附件名',



	fileurl VARCHAR (512) COMMENT '附件下载地址',



	filesize BIGINT COMMENT '附件大小，单位Byte',



	-- 主键本身也是一种索引（注:也可以在上面的创建字段时使该字段主键自增）



        PRIMARY KEY (id),



        -- 创建组合索引



	INDEX (fileuploadercode,projectid)



	-- 指定使用INNODB存储引擎(该引擎支持事务)、utf8字符编码



) ENGINE = INNODB DEFAULT CHARSET = utf8 COMMENT '项目附件表';
```

### 建表后创建：

**ALTER TABLE 表名 ADD [UNIQUE | FULLTEXT | SPATIAL]  INDEX | KEY  [索引名] (字段名1 [(长度)] [ASC | DESC]) [USING 索引方法]；**

或

**CREATE  [UNIQUE | FULLTEXT | SPATIAL]  INDEX  索引名 ON  表名(字段名) [USING 索引方法]；**

示例一：

```sql
-- 假设建表时fileuploadercode字段没创建索引(注:同一个字段可以创建多个索引，但一般情况下意义不大)



-- 给projectfile表中的fileuploadercode创建索引



ALTER TABLE projectfile ADD UNIQUE INDEX (fileuploadercode);
```

示例二：

```sql
ALTER TABLE projectfile ADD INDEX (fileuploadercode, projectid);
```

示例三：

```sql
-- 将id列设置为主键



ALTER TABLE index_demo ADD PRIMARY KEY(id) ;



-- 将id列设置为自增



ALTER TABLE index_demo MODIFY id INT auto_increment;  
```

------

# 查看已创建的索引：

**show index from 表名;**

提示:我们也可以直接使用工具查看

示例：

![img](https://img-blog.csdn.net/2018080618133679?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

------

# 索引的删除：

**DROP INDEX 索引名 ON 表名**

或

**ALTER TABLE 表名 DROP INDEX 索引名**

示例一：

```sql
drop index fileuploadercode1 on projectfile;
```

示例二：

```sql
alter table projectfile drop index s2123;
```

------

# 查看SQL语句对索引的使用情况(即:查询SQL的查询执行计划QEP)：

**在select语句前加上EXPLAIN即可。**

示例：

```sql
EXPLAIN SELECT * FROM `index_demo` ii WHERE ii.e_name = 'Jane';
```

分析该SQL的性能为：

![img](https://img-blog.csdn.net/20180806181942164?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

提示：我们也可以使用SQL工具查看，如：navicat中的“解释”选项即可查看。

说明：

**id：**SELECT识别符。这是SELECT的查询序列号。

**select_type：**SELECT类型。

1. SIMPLE： 简单SELECT(不使用UNION或子查询)
2. PRIMARY： 最外面的SELECT
3. UNION：UNION中的第二个或后面的SELECT语句
4. DEPENDENT UNION：UNION中的第二个或后面的SELECT语句，取决于外面的查询
5. UNION RESULT：UNION的结果
6. SUBQUERY：子查询中的第一个SELECT
7. DEPENDENT SUBQUERY：子查询中的第一个SELECT，取决于外面的查询
8. DERIVED：导出表的SELECT(FROM子句的子查询)

**table：**表名

**type：**联接类型。是SQL性能的非常重要的一个指标，结果值从好到坏依次是：system > const > eq_ref > ref
            \> fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL。
            一般来说，得保证查询至少达到range级别。

1. system：表仅有一行(=系统表)。这是const联接类型的一个特例。
2. const：表最多有一个匹配行，它将在查询开始时被读取。因为仅有一行，在这行的列值可被优化器剩余部分认为是常数。const用于用常数值比较PRIMARY KEY或UNIQUE索引的所有部分时。
3. eq_ref：对于每个来自于前面的表的行组合，从该表中读取一行。这可能是最好的联接类型，除了const类型。它用在一个索引的所有部分被联接使用并且索引是UNIQUE或PRIMARY KEY。eq_ref可以用于使用= 操作符比较的带索引的列。比较值可以为常量或一个使用在该表前面所读取的表的列的表达式。
4. ref：对于每个来自于前面的表的行组合，所有有匹配索引值的行将从这张表中读取。如果联接只使用键的最左边的前缀，或如果键不是UNIQUE或PRIMARY KEY(换句话说，如果联接不能基于关键字选择单个行的话)，则使用ref。如果使用的键仅仅匹配少量行，该联接类型是不错的。ref可以用于使用=或<=>操作符的带索引的列。
5. ref_or_null：该联接类型如同ref，但是添加了MySQL可以专门搜索包含NULL值的行。在解决子查询中经常使用该联接类型的优化。
6. index_merge：该联接类型表示使用了索引合并优化方法。在这种情况下，key列包含了使用的索引的清单，key_len包含了使用的索引的最长的关键元素。
7. unique_subquery：该类型替换了下面形式的IN子查询的ref：value IN (SELECT primary_key FROMsingle_table WHERE some_expr);unique_subquery是一个索引查找函数，可以完全替换子查询，效率更高。
8. index_subquery：该联接类型类似于unique_subquery。可以替换IN子查询，但只适合下列形式的子查询中的非唯一索引：value IN (SELECT key_column FROM single_table WHERE some_expr)
9. range：只检索给定范围的行，使用一个索引来选择行。key列显示使用了哪个索引。key_len包含所使用索引的最长关键元素。在该类型中ref列为NULL。当使用=、<>、>、>=、<、<=、IS NULL、<=>、BETWEEN或者IN操作符，用常量比较关键字列时，可以使用range
10. index：该联接类型与ALL相同，除了只有索引树被扫描。这通常比ALL快，因为索引文件通常比数据文件小。
11. all：对于每个来自于先前的表的行组合，进行完整的表扫描。如果表是第一个没标记const的表，这通常不好，并且通常在它情况下很差。通常可以增加更多的索引而不要使用ALL，使得行能基于前面的表中的常数值或列值被检索出。

**possible_keys：**possible_keys列指出MySQL能使用哪个索引在该表中找到行。注意，该列完全独立于EXPLAIN输出所示的表的次序。这意味着在possible_keys中的某些键实际上不能按生成的表次序使用。

**key：**key列显示MySQL实际决定使用的键(索引)。如果没有选择索引，键是NULL。要想强制MySQL使用或忽视possible_keys列中的索引，在查询中使用FORCE INDEX、USE INDEX或者IGNORE INDEX。

**key_len：**key_len列显示MySQL决定使用的键长度。如果键是NULL，则长度为NULL。注意通过key_len值我们可以确定MySQL将实际使用一个多部关键字的几个部分。

**ref：**ref列显示使用哪个列或常数与key一起从表中选择行。

**rows：**rows列显示MySQL认为它执行查询时必须检查的行数。

**Extra：**该列包含MySQL解决查询的详细信息。

1. Distinct：MySQL发现第1个匹配行后，停止为当前的行组合搜索更多的行。
2. Not exists：MySQL能够对查询进行LEFT JOIN优化，发现1个匹配LEFT JOIN标准的行后，不再为前面的的行组合在该表内检查更多的行。
3. range checked for each record (index map: #)：MySQL没有发现好的可以使用的索引，但发现如果来自前面的表的列值已知，可能部分索引可以使用。对前面的表的每个行组合，MySQL检查是否可以使用range或index_merge访问方法来索取行。
4. Using filesort：MySQL需要额外的一次传递，以找出如何按排序顺序检索行。通过根据联接类型浏览所有行并为所有匹配WHERE子句的行保存排序关键字和行的指针来完成排序。然后关键字被排序，并按排序顺序检索行。
5. Using index：从只使用索引树中的信息而不需要进一步搜索读取实际的行来检索表中的列信息。当查询只使用作为单一索引一部分的列时，可以使用该策略。
6. Using temporary：为了解决查询，MySQL需要创建一个临时表来容纳结果。典型情况如查询包含可以按不同情况列出列的GROUP BY和ORDER BY子句时。
7. Using where：WHERE子句用于限制哪一个行匹配下一个表或发送到客户。除非你专门从表中索取或检查所有行，如果Extra值不为Using where并且表联接类型为ALL或index，查询可能会有一些错误。
8. Using sort_union(...), Using union(...), Using intersect(...)：这些函数说明如何为index_merge联接类型合并索引扫描。
9. Using index for group-by：类似于访问表的Using index方式，Using index for group-by表示MySQL发现了一个索引，可以用来查询GROUP BY或DISTINCT查询的所有列，而不要额外搜索硬盘访问实际的表。并且，按最有效的方式使用索引，以便对于每个组，只读取少量索引条目。

------

# 单列索引的使用：

### 准备工作：

给id加主键索引：

![img](https://img-blog.csdn.net/20180806182741565?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

再分别给name、city、country、address加上普通索引：

![img](https://img-blog.csdn.net/20180806182755720?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

注：以上五个索引都是单列索引。

### 使用情况：

**只涉及到其中的一个字段时，都能使用到索引**(以e_name为例):

![img](https://img-blog.csdn.net/20180806182921391?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 

![img](https://img-blog.csdn.net/20180806182951165?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

注：模糊查询时，%如果在前面，那么不会使用索引。

**涉及到多个索引字段时,如果这些索引字段中，存在主键索引，那么只会使用该索引**（即:MYSQL优化器会选出并先执行最“严”的索引）:

![img](https://img-blog.csdn.net/20180806183117535?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

提示：possible_key中，只是SQL语句里涉及到的索引；key中才是实际上执行查询操作时使用到了的索引。

**涉及到多个索引字段时,如果这些索引字段中，不存在主键索引的话，那么就会使用该使用的索引**（注：如果通过其中的部分索引就能准确定位的话，那么其余的索引就不再被使用）:

![img](https://img-blog.csdn.net/20180806183230607?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 

![img](https://img-blog.csdn.net/20180806183243612?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

注：多个索引时，先使用哪个索引后使用哪个索引，是由MySQL的优化器经过一些列计算后作出的抉择。

**当对索引字段进行** **>， <，>=， <=，not in，between …… and ……，函数(索引字段)，like模糊查询%在字段前时**，**不会使用该索引**

![img](https://img-blog.csdn.net/20180806183445354?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

注：这里对e_age字段进行了 “<” ，所以实际查询时，并没有使用e_age的索引。

提示：在实际使用时，如果涉及到多列，我们一般都不会将这些列一 一创建为单列索引，而是将这些列创建为组合索引。

------

# 组合索引的使用：

### 最左原则：

​       **假设组合索引为：a,b,c的话；那么当SQL中对应有：a或a，b或a，b，c的时候，可称为完全满足最左原则；当SQL中查询条件对应只有a，c的时候，可称为部分满足最左原则；当SQL中没有a的时候，可称为不满足最左原则**。

注：**MySQL5.7开始，会自动优化，如：会把c，b，a优化为a，b，c使之完全遵循最左原则；会把c，a优化为a，c使之部       分遵循最左原则**。即：SQL语句中的对应条件的先后顺序无关。

### 准备工作：

![img](https://img-blog.csdn.net/20180806183823434?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

创建了组合索引:**e_name，e_age，e_country，e_city**。

### 使用情况：

**完全满足最左原则****：**

![img](https://img-blog.csdn.net/20180806183950720?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

注：与条件的先后无关(这是因为MYSQL5.7开始，对索引全排列有优化，会自动优化为按组合索引的顺序进行查询)，
       即：下面这样的话，也是会完整的走组合索引的：

![img](https://img-blog.csdn.net/20180806184004390?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**部分满足最左原则****：**

![img](https://img-blog.csdn.net/20180806184124542?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

注：此SQL中，只有e_name和e_country满足部分最左原则(e_name满足)，所以到e_name字段时会走组合所以，但是
       只会走到e_name那里，到e_country时就不会使用组合索引了。

**不满足最左原则****：**

![img](https://img-blog.csdn.net/20180806184203183?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**满足(部分满足)最左原则的字段里，有字段不满足“索引”自身的使用规范****：**

说明：如果SQL语句里的字段里，满足了最左原则，但是不满足“索引”自身的使用规范，那么组合索引走到这里之后，
           不会再往下走了。

![img](https://img-blog.csdn.net/20180806184244788?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p1c3RyeV9kZW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

如图所示：由于e_age字段使用了“>”符号，不符合“索引”自身的使用规范，那么当“e_name”走完组合索引后，
                  走到“e_age”时，该字段及其后面的字段不会再走组合索引了。

------

# 【补充】使用组合索引时，不遵循最左原则仍然会走索引的特殊种情况：

## 相关概念：

**聚集索引与非聚集索引：**

​       每个InnoDB表具有一个特殊的索引称为聚簇索引（也叫聚集索引，聚类索引，簇集索引）。如果表上定义有主键，该主键索引就是聚簇索引。如果未定义主键，MySQL取第一个唯一索引（unique）而且只含非空列（NOT NULL）作为主键，InnoDB使用它作为聚簇索引。如果没有这样的列，InnoDB就自己产生一个这样的ID值，它有六个字节，而且是隐藏的，使其作为聚簇索引。

​       表中的聚簇索引（clustered index ）就是一级索引，除此之外，表上的其他非聚簇索引都是二级索引，又叫辅助索引（secondary indexes）。

**回表：**

​        当二级索引无法直接查询到(SQL中select需要的所有)列的数据时，会通过二级索引查询到聚簇索引(即:一级索引)后，再根据(聚集索引)查询到(二级索引中无法提供)的数据，这种通过二级索引查询出一级索引，再通过一级索引查询(二级索引中无法提供的)数据的过程，就叫做回表。

## 当无需回表时，不遵循最左原则也是会走组合索引：

如，现有表：

![img](https://img-blog.csdnimg.cn/20200425150055474.png)

id是主键，其余三个字段组成联合索引：

![img](https://img-blog.csdnimg.cn/20200425150127514.png)

当不需要进行回表时，即便我们的SQL不满足组合索引最左原则，也会走组合索引的，如:

![img](https://img-blog.csdnimg.cn/20200425150209577.png)

​        这里where后直接是gender时， 是不遵循组合索引的最左原则的，但是查询计划显示使用了索引的。这是因为: 对这张表进行select *，相当于进行select id,name,age,gender，其中，id是主键(一级索引)，name、age、gender是组合索引(二级索引)，这里查询时，能直接从索引中拿到想要查询的所有列的数据，是不需要回表查询的，所以这里哪怕sql写法上不遵循最左原则，但是仍然是会走索引的。

如果这个时候，我们加一个普通的motto字段：

![img](https://img-blog.csdnimg.cn/20200425150248638.png)

 

使用相同的SQL进行查询，可看到：

![img](https://img-blog.csdnimg.cn/20200425150257444.png)

 

此时进行select *，相当于进行select id,name,age,gender,motto，其中motto字段是从索引(一级索引、二级索引)里面获取不到数据的，是肯定需要回表的。而查询条件又不遵循最左原则，所以不会走组合索引。

注：其它情况下，只有(完全或部分)遵循了最左原则，才会走组合索引。

------

### ^_^ 如有不当之处，欢迎指正

### ^_^ 参考链接   

https://www.cnblogs.com/DreamDrive/p/7752960.html               

https://www.cnblogs.com/tommy-huang/p/4317305.html

https://blog.csdn.net/linjpg/article/details/56054994

https://www.jb51.net/article/118371.html

<https://www.csdn.net/gather_2a/MtTaMgwsNzY0OS1ibG9n.html>

### ^_^ 如涉及侵权问题，请及时联系我

### ^_^ 本文已经被收录进《程序员成长笔记(二)》，笔者JustryDeng
