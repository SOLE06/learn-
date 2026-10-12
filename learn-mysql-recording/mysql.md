 # 菜鸟教程 MySQL教程
- 数据库：按照数据结构组织、存储、管理数据的仓库
- RDBMS：关系型数据库管理系统
    - 特点：
        - 1.数据以表格形式出现
        - 2.每行为各种记录名称
        - 3.每列为记录对应数据域
        - 4.行列组成表格
        - 5.表格组成数据库
- 一些术语：
    - 数据库
    - 数据表 
    - 行、列
    - 冗余：存储两倍数据
    - 主键、外键（关联两个表）、复合键（多个列作为索引）
    - 索引
    - 参照完整性：要求关系中不允许引用不存在的实体，即一个表中的外键（非空时）必须能在另一个表中找到
    - 表头：每列名称
-表格：表头、行、列、值、键（唯一性）    

# 《MySQL必知必会（第二版）》学习  
**作者**：Ben Forta
## 第1章 了解SQL
- DBMS：数据库管理系统
- 表：特定类型数据的清单，数据库中的表是唯一的，但是不同数据库可以有相同的表名
- 模式：数据库和表的布局特性
- 行：一个记录
- **主键**：唯一标识某一行，称为键/键码
    - 任两行的主键不能相同
    - 主键列不允许为NULL,没黄都必须有一个主键
    - 主键可以是一个元组
    - 主键的设定规则：
        - 不更新主键列的值
        - 不重用主键列的值
        - 不在主键列使用可能会修改的值
- SQL：结构化查询语言，任两个DBMS的SQL都不完全相同

## 第2章 MySQL简介
- DBMS分为两类：
    - 基于共享文件系统：桌面
    - 基于客户机-服务器：服务器负责数据访问和处理，客户机和用户打交道
        - 服务器软件：MySQL DBMS
        - 客户端软件：MySQL提供的工具、脚本语言、程序设计语言、web应用程序开发语言和平台
- MySQL实用工具
    - mysql命令行实用工具
        - 命令提示符输入 `mysql -u root -p`,打开mysql
        - 命令用";"或者"\g"结束
        - 输入help或\h获得帮助
        - 输入quit或exit退出命令行实用程序
    - MySQL workbench
        - 图形交互式客户端

## 第3章 使用MySQL
- 使用命令行
    - 打开MySQL：`mysql --user=<用户名> --password
    - 选择数据库：`USE <数据库名>`
    - 了解数据库和表：  

    | 语句 | 效果 |
    | :---: | :---: |
    | SHOW DATABASES; |返回数据库列表|
    | SHOW TABLES; | 获得一个数据库中的表 |
    | SHOW COLUMNS FROM `表名`;|获得表的列信息|
    |SHOW STATUS;|显示广泛的服务器状态|
    |SHOW CREATE DATABASE `数据库名`;|显示创建数据库的语句|
    |SHOW CREATE TABLE `表名`;|显示创建表的语句|
    |SHOW GRANTS;|显示授予用户的权限|
    |SHOW ERRORS/WARNINGS;|显示服务器错误/报警信息|
- 使用MySQL workbench
    - 选择数据库：双击数据库标签，变粗体即可

## 第4章 检索数据
- SELECT语句
- 检索单列：SELECT `列名` FROM `表名`
- `;`结束SQL语句，多个SQL语句必须以`;`分隔
- SQL大小写：
    - SQL大小写不敏感，某些标识符大小写敏感
    - 格式化样式：对关键字大写，对列名、表名小写
- 检索多列：`SELECT col1,col2,... FROM `，列名之间用逗号隔开
- 检索所有列：`SELECT * FROM `，（可以检索出未知列名的列）
- 检索不同行：`SELECT DISTINCT 列名 FROM`,当DISTINCT后接了多列的话，那么检索的是不同组合的行
- 限制结果：最后添加 `LIMIT 数字`，`LIMIT 数字n`:返回前n行，`LIMIT n1,n2`:返回第n1行开始的前n2行
    - LIMIT的第一个参数从0开始计数，0,1是指跳过第0行的第一行
- 使用完全限定的表名/列名：
`SELECT 表名.列名 FROM 数据库名.表名`
- 注释：
    - 单行注释： -- 或 #
    - 多行注释：/*       */

## 第5章 排列检索数据
- 使用 ORDER BY +列名 进行排序，列可以选择要检索的列和不检索的列
```
SELECT cust_id,cust_name,cust_contact 
FROM customers
ORDER BY cust_zip;
```
- 按多列排序，从左往右的优先级顺序
- 按列位置排序，`ORDER BY num1,num2...`是按照SELECT语句中要检索的列的位置进行先后排序，当要排序的列不在SELECT 后，则不能使用按对应列的位置进行排列，但是可以列名和列位置混合使用
- 排序方向：
    - 升序排列 ASC：默认
    - 降序排列 DESC ：每个DESC只适用于其前面的列名，当要指定多个列降序排列时，要降序的每个列之后都要有DESC
    - 在字典排序顺序中，大小字母视为相同（大多数DBMS默认）
- ORDER BY 子句的位置：位于FROM 之后，LIMIT之前

## 第6章 过滤数据
使用WHERE子句过滤，WHERE子句要位于ORDER BY子句前
- WHERE子句运算符  

| 运算符 | 说明 |
| :---: | :---: |
| = | 等于 |
|<>|不等于|
|!=|不等于|
|<|小于|
|<=|小于等于|
|>|大于|
|>=|大于等于|
|between|指定的两个值之间|

!=和<>是等价的
- 空值检查
    - 空值：NULL，表示未知，不是空字符串也不是0
    - 检查： WHERE xxx is NULL
    - 对数据进行过滤操作时，普通的比较运算符会天然
    过滤掉NULL行，当过滤条件隐含的涉及“某列是否为NULL”（没有明显写出NULL），可能被漏掉或错判
  

## 第7章 高级数据过滤
- 组合WHERE语句：and子句和or子句
    - and运算符的优先级高于or运算符
    - 同时使用and 和 or 时，要用圆括号包起对应的表达式
- IN 运算符
    - 指定一系列条件，任一条件都可以匹配
    - 后跟逗号分隔的有效值列表，所有值括在圆括号中
    - IN (xxx,xxx)，和or实现一样功能
    - 优点：清楚易读、执行效率快、计算次序容易管理、可以包含其他SELECT语句
- NOT 运算符
    - 否定后面的条件
    - 在复杂的语句中NOT有优势
    - MySQL允许NOT否定 IN、BETWEEN、EXISTS子句，大多数其他DBMSNOT否定所有子句

##  第8章 用通配符进行过滤
在搜索子句中使用通配符，必须使用LIKE谓词
通配符可以在任意位置使用
- %通配符：匹配任意数量的任意字符，可以是0个、1个或多个，不匹配NULL
- 尾空格会干扰通配符的匹配，解决方法：1、最后附加一个% 2.用函数去除尾空格
- 下划线通配符：匹配单个字符
- 使用通配符的技巧：
    - 不要过度使用通配符
    - 确实需要使用通配符时，不要放在搜索的开头处，会降低搜索效率
    - 注意通配符位置

## 第9章 用正则表达式进行搜索
正则表达式：用来匹配文本的特殊字符串  
MySQL仅支持大多数正则表达式实现的一个很小的子集
- `REGEXP +正则表达式` 构成匹配条件
- 正则表达式中： `.` 匹配任意一个字符
- `REGEXP` 是部分匹配，当要匹配整个列值实现和`LIKE`一样的功能时，需要使用定位符`^`和`$`
- `|` 是正则表达式中的`or`运算符
- `[]`匹配几个字符之一 e.g.[123]是匹配1、2、3中的一个
- `[^xxx]`是匹配除了方括号中的字符中的任意一个
- 用`-`定义匹配范围，如[0-9]指的是0-9中的任意一个数字
- 匹配特殊字符，用`\\`转义，`\\`可引用元字符

| 元字符 | 说明 |
| :---: | :---: |
| \\f | 换页|
| \\n | 换行 |
| \\r | 回车 |
| \\t | 制表 |
| \\v | 纵向制表 |

- 匹配字符类

|类|说明|
|:---:|:---:|
|[:alnum:]|任意字母或数字|
|[:alpha:]|任意字母|
|[:blank:]|空格或制表符|
|[:cntrl:]|ASCII控制字符|
|[:digit:]|任意数字|
|[:graph:]|任意可打印字符（不包括空格）|
|[:lower:]|任意小写字符|
|[:print:]|任意可打印字符|
|[:punct:]|不在[:alnum:]和[:cntrl:]中的任意字符|
|[:space:]|包括空格在内的任意空白字符|
|[:upper:]|任意大写字母|
|[:xdigit:]|任意十六进制数|


- 匹配多个实例

|元字符|说明|
|:---:|:---:|
|*|0或多个匹配|
|+|1或多个匹配|
|?|?前面的单个字符的0或1个匹配|
|{n}|指定数目的匹配|
|{n,}|不小于指定数目的匹配|
|{n,m}|匹配数目的范围(m<=255)|

正则表达式中的()当不用转义字符时表示分组，即将括号内的视为一个整体


- 定位符

|元字符|说明|
|:---:|:---:|
|^|文本的开头|
|$|文本的结尾|
|[[:<:]]|词的开头|
|[[:>:]]|词的结尾|
    
- 简单的正则表达式测试
在不使用数据库的情况下进行测试，REGEXP总是返回0或1
e.g.`SELECT "HELLO" REGEXP "[0-9]"
    

## 第10章 创建计算字段
计算字段：运行时在SELECT语句内创建
- 拼接字段：大多数DBMS使用+或||拼接，MySQL使用Concat()函数实现
e.g. 
```
SELECT Concat(prod_name,'(price:',prod_price,')')
FROM products;
```
- Trim()函数：Trim():删除字符串左右两边空格；LTrim():删除左边空格；RTrim():删除字符串左右两边空格；
- MySQL函数不区分大小写
- 使用别名(alias):
    - 使用 as 关键字 `SELECT xxx AS xxx `

    ```
    SELECT Concat(prod_name,'(price:',prod_price,')') as prod_info
    FROM products;
    ```
    - 别名也称导出列
- 执行算术运算
```
SELECT order_num,order_item,item_price*quantity AS total_price
FROM orderitems
ORDER BY total_price;
```

- MySQL支持基本运算：加、减、乘(*)、除(/)
- SELECT测试运算：省略FROM语句可以简单的访问和处理表达式

## 第11章 使用数据处理函数
- 文本处理函数

==常用的文本处理函数==

|函数|功能|
|:---:|:---:|
|Left()|返回字符串左边的字符|
|Length()|字符串长度|
|Locate()|找出字符串子串|
|Lower()|转为小写|
|LTrim()|去除左边空格|
|Right()|返回右边字符|
|RTrim()|去除右边空格|
|Soundex()|返回字符串的SOUNDEX值|
|Substring()|返回子字符串字符|
|Upper()|转为大写|

Soundex():将任何文本字符串转换为代表其发音的4个字符值

- 日期和时间处理函数

 ==常用的日期和时间处理函数==

|函数|功能|
|:---:|:---:|
| AddDate() | 增加一个日期（天、周等） |
| AddTime() | 增加一个时间（时、分等） |
| CurDate() | 返回当前日期 |
| CurTime() | 返回当前时间 |
| Date() | 返回日期时间的日期部分 |
| DateDiff() | 计算两个日期之差 |
| Date_Add() | 高度灵活的日期运算函数 |
| Date_Format() | 返回一个格式化的日期或时间串 |
| Day() | 返回一个日期的天数部分 |
| DayOfWeek() | 对于一个日期，返回对应的星期几 |
| Hour() | 返回一个时间的小时部分 |
| Minute() | 返回一个时间的分钟部分 |
| Month() | 返回一个日期的月份部分 |
| Now() | 返回当前日期和时间 |
| Second() | 返回一个时间的秒部分 |
| Time() | 返回一个日期时间的时间部分 |
| Year() | 返回一个日期的年份部分 |

日期格式：yyyy-mm-dd 

- 数值处理函数
==常用数值处理函数==

| 函 数 | 说 明 |
|:---:|:---:|
| Abs() | 返回一个数的绝对值 |
| Cos() | 返回一个角度的余弦 |
| Exp() | 返回一个数的指数值 |
| Mod() | 返回除操作的余数 |
| Pi() | 返回圆周率 |
| Rand() | 返回一个随机数 |
| Sin() | 返回一个角度的正弦 |
| Sqrt() | 返回一个数的平方根 |
| Tan() | 返回一个角度的正切 |


## 第12章 汇总数据
- 聚合函数
聚合函数：运行在行组上，计算和返回单个值的函数

==SQL聚合函数==

| 函 数 | 说 明 |
|:---:|:---:|
| AVG() | 返回某列的平均值 |
| COUNT() | 返回某列的行数 |
| MAX() | 返回某列的最大值 |
| MIN() | 返回某列的最小值 |
| SUM() | 返回某列值之和 |

- AVG():确定特定数值列的平均值，列名必须作为函数参数给出，忽略值为NULL的行
- COUNT():确定表中行数或符合条件的行数
    - COUNT(*):计算表中行数，不管是NULL还是非空值
    - COUNT(column):计算特定列中有值的参数,NULL 值会被忽略
- 所有聚合函数都可以用来执行多列上的计算
- 聚合不同值
    - 对所有行执行运算：ALL 或不指定任何参数
    - 对不同值：DISTINCT   
        - 通配符不使用DISTINCT，DISTINCT必须与列名一起使用，不能和计算式或表达式一起使用（不能用在计算式或者表达式中）

## 第13章 数据分组

- GROUP BY 子句：创建分组
- 一些规定
    - GROUP BY子句可以包含任意数目的列
    - GROUP BY 子句中的分组依据不能是聚合函数表达式（聚合函数在分组后给出）
    - 在子句中嵌套了分组，数据将在最后规定的分组上汇总
    - 子句列出的每个列都必须是检索列或有效表达式，不能是聚集函数。在SELECT 中使用表达式，必须在子句中指定相同的表达式，不能使用别名
    - 除聚集函数计算语句外，SELECT语句中的每个列必须在GROUP BY 子句中给出
    - 列中有多行NULL值，会被分成一组
    - 必须在WHERE 子句后，ORDER BY子句前
    - 使用了GROUP BY子句，要进行排序时，要么是分组依据，要么是聚合函数表达式
- 过滤分组 HAVING
    - HAVING支持所有的WHERE运算符
    - WHERE在数据分组前过滤，HAVING在数据分组后过滤
  
## 第14章 使用子查询
子查询：嵌套在其他查询中的查询
- 利用子查询进行过滤：可以把一个SELECT语句返回的结果用在另一个SELECT语句的WHERE子句
    - 子查询由内向外处理
    - 在WHERE子句中使用子查询时，SELECT语句具有和WHERE子句相同数目的列
    - 实际中不能过多嵌套子查询
- 作为计算字段使用子查询
```
SELECT cust_name,cust_id,
		(SELECT COUNT(*) FROM orders
        WHERE orders.cust_id=customers.cust_id ) AS order_num
FROM customers
ORDER BY cust_name
```
- 相关子查询：涉及外部查询的子查询

## 第15章 表连接  
可伸缩性：可以适应不断增加的工作量而不失败
- 连接用来在SELECT语句中关联表，可以连接多张表返回一组输出
- 创建连接：规定要连接的表以及它们之间如何关联
- 当引用的列可能出现二义性时，需要完全限定列名，在列名前添加表名
- 笛卡尔积：由没有连接关系的表关系返回的结果
- 交叉连接：返回笛卡尔积的连接  
<br>
- 等值连接（内连接）：` FROM xxx INNER JOIN xxx ON <条件>`
- 连接多张表：列出所有表，定义表之间关系

## 第16章 创建高级连接
- 表别名：可用于WHERE子句、SELECT子句、ORDER BY 子句及其他部分。表别名是DBMS所特有的，不会返回客户端
- 自连接：在单个SELECT语句中多次引用一张表，有时自连接的效率高于子查询，在引用表时为了区分开表可以使用表别名
- 自然连接：排除出现多次的情况，使每列都只返回一次；对一张表使用通配符，对所有其他表的列使用明确子集
- 外连接：在连接中包含了在相关表中没有关联行的那些行 OUTER JOIN
    - 使用时需指定LEFT OUTER JOIN (包括左边表所有行) 或者 RIGHT OUTER JOIN(包括右边所有行)
    - MySQL中没有*=和=*的用法，两种运算符在其他DBMS中流行
- 聚合函数可以和连接一起使用
- 使用连接：
    - 注意连接类型
    - 使用正确的连接条件
    - 总是提供连接条件
    - 多个表连接时，同时测试之前要分别测试每个连接，便于故障排查

## 第17章 组合查询
利用UNION运算符把多个SELECT语句组合成结果集
- 复合查询（并查询/UNION）：执行多个查询，将结果作为单个结果集返回
两种情况：
    - 单个查询中从不同表返回类似结构的数据
    - 对单张表执行多个查询，按单个查询返回数据
- 在SELECT语句之间加入UNION
```
SELECT prod_id,prod_name,vend_id,prod_price
FROM products
WHERE prod_price>=5
UNION 
SELECT prod_id,prod_name,vend_id,prod_price
FROM products
WHERE vend_id IN (1001,1003);
``` 
- UNION 使用规则
    - 必须由两个或以上的SELECT语句组成
    - 每个查询必须包含相同数量的列、表达式或聚集函数，但列顺序不要求一致
    - 按位置对应的列，列数据类型必须兼容
- UNION 会过滤所有重复的行
- UNION ALL 不会过滤重复行
- UNION几乎总是可以达到多个WHERE子句的效果，但是UNION ALL 可以完成WHERE子句完成不了的工作,实现每个条件的匹配行全部出现（包括重复行）
- 在用UNION组合查询结果集时，可以进行排序，但是只能使用一条ORDER BY子句，必须出现在最后一条SELECT语句后，不允许使用多条ORDER BY

## 第18章 全文搜索

- 并非所有引擎都支持全文搜索，MySQL支持几种基本的数据库引擎
- 通配符和正则表达式的限制：
    - 当行数多时，搜索可能很耗时
    - 明确控制：很难明确匹配什么和不匹配什么
    - 智能化的结果：不能提供智能化选择结果的方法
- 全文搜索只能在具有特定索引列的表上执行，在创建表时即可创建索引
    - MATCH()指定被搜索的列
    - 传递给MATCH()中的值必须与FULLTEXT()中定义的相同，FULLTEXT()是专门用于全文搜索的索引类型
    - AGAINST()指定要使用的搜索表达式
    - 对于英文等按照空格分割的语言，按照空格分割的词来搜索
    - 除非使用BINARY模式，搜索不区分大小写
    - 全文搜索的结果按照匹配程度排序，排名较高的行先返回
```
SELECT note_text,
	(MATCH(note_text) AGAINST ("customer")) AS suit
FROM productnotes
```
- 使用查询扩展  `AGAINST("XXX" WITH QUERY EXPANSION`)
    - 放宽全文搜索结果范围
    - 对数据和索引进行两次搜索
    - 找出可能相关的结果，即使不精确包含所查找的词

```
SELECT note_text
FROM productnotes
WHERE MATCH(note_text) AGAINST ("customer" WITH QUERY EXPANSION) 
```
- 布尔文本搜索
    -  布尔模式可以提供：要匹配的词、要排斥的词、排列提示、表达式分组
    - 即使没有FULLTEXT()也可以使用
e.g.要匹配的词
```
SELECT note_text
FROM productnotes
WHERE MATCH(note_text) AGAINST ("customer" IN BOOLEAN MODE) 
```

==全文布尔运算符==
|布尔运算符|说明|
|:---:|:---:|
|+|包含，词必须存在|
|-|排除，词必须不出现|
|>|包含，且提高匹配度|
|<|包含，且降低匹配度|
|~|取消一个词的排序值|
|*|词尾的通配符|
|""|使用文本短语匹配包含或删除|


e.g. 
('rabbit bait' IN BOOLEAN MODE):包含两个词中的至少一个
('"rabbit bait"' IN BOOLEAN MODE):包含rabbit bait 这个短语

- 全文索引的使用说明
    - 索引全文数据时，会默认忽略短词（长度小于等于3）
    - MySQL有内带的停用词列表，在索引全文数据时会被忽略
    - 当一个词出现在50%及以上，将它作为停用词忽略（不用于布尔模式）
    - 表中行数少于3行，不返回结果
    - 搜索时忽略单引号 don't 索引为dont
    - 不具有词分隔符的语言不能恰当的返回全文搜索结果


## 第19章 插入数据
INSERT：将行插入/添加到数据库表中  
方式：插入完整行、插入行的一部分、插入多行、插入某些查询结果  
INSERT一般没有输出
- 插入完整行：
指定表名和被插入到新行中的值
格式：
`INSERT INTO <tablename> VALUES()`，必须对每列赋值，如果某列没有值则应该赋NULL，各列必须以在表中定义的顺序被填充。
或者`INSERT INTO <tablename>(列名) VALUES()`，提供的列名次序无需和实际表中定义的次序一致。  
一般不要使用没有明确给出列的列表的INSERT语句。使用列的列表会继续发挥作用，改变表的结构  
必须给出VALUES的正确数目，当表中的某些列满足：  
该列定义允许NULL值/该列有默认值  
则可省略列
- INSERT 操作可能很耗时，为了提高性能，在INSERT和INTO中插入关键字`LOW_PRIORITY`
降低优先级，同样适用于UPDATE和DELETE
- 插入多个行：
    - 使用多条INSERT语句，分号结束
    - 使用单个INSERT语句，值()分组，即每组值用圆括号括起来，逗号分隔
    - 单个INSERT的性能高于多个INSERT语句 
- 插入检索出的数据  
    - INSERT SELECT语句
    - 将一个SELECT语句的结果插入表中
    - SELECT语句可以包含WHERE子句过滤

## 第20章 更新数据和删除数据
- 更新数据——UPDATE语句
    - 更新表中特定行/更新表中所有行
    - UPDATE语句的组成部分：要更新的表、列名和它们的新值、要更新行的过滤条件
    - SET 命令用来将新值赋给被更新的列
    - UPDATE语句以WHERE子句结束
    - 更新多个列：只需要单个SET命令，每个column=value对
    之间用逗号分隔，最后一列之后不用逗号
    - 更新多行时，在更新时可能会出现错误，整个UPDATE操作会被取消，当发生错误也要继续更新时，
    可以用IGNORE关键字，格式为`UPDATE IGNORE 表名`
    - UPDATE删除数据：将对应数据更新为NULL
```
UPDATE customers
SET cust_email='ertd@opnecd.com'
WHERE cust_id=10002
```
- 删除数据——DELETE语句
    - 从表中删除特定行/删除表中所有行
    - 格式： `DELETE FROM 表名 WHERE...`
    - DELETE是删除整行而不是删除列，要删除指定列要用UPDATE语句
    - DELETE是删除表的内容而不是表本身
    - 删除所有行可以使用TRUNCATE TABLE语句，实际上是删除原来表并重新创建新表，速度更快
- 使用UPDATE和DELETE的原则
    - 执行UPDATE或DELETE时一定要有WHERE子句，除非确实打算更改每一行
    - 确保每张表都有一个主键，尽可能使用主键作为WHERE子句的条件
    - 使用WHERE子句前应先用SELECT语句进行测试
    - 使用数据库强制地引用完整性

## 第21章 创建和操作表
- 创建表——CREATE TABLE
    - 两种方法：使用具有交互式创建和管理表的工具/直接使用MySQL语句
    - 创建表必须给出：新表的名字、表列的名字和定义，用逗号分隔
    - CREATE TABLE+表名  
    (  
       列定义  
    )
    - 创建新表时，指定的表名必须不存在否则将出错，为了防止意外覆盖已有的表，要手动删除后再重建，而不是单纯用创建表重建
    - 仅想在表不存在时创建，要在表名后给出`IF NOT EXISTS`
    - 使用NULL值：NOT NULL：不接受该列没有值；NULL：接受该列没有值
    - AUTO_INCREMENT:增加一行时，该列自动递增；每张表只允许一列AUTO_INCREMENT，它必须被索引
        - last_insert_id():返回最后一个AUTO_INCREMENT值
    - 指定默认值：`DEFAULT 默认值`，默认值不允许是函数（8.0.13版本之后允许）
    - 引擎类型：写在最后，`ENGINE=引擎名`
        - InnoDB：可靠的事务处理引擎，任何情况下都推荐，5.5版本以上默认
        - MEMORY:数据存储在内存，速度快，适用于临时表
        - MyISAM:支持全文搜索，但不支持事务处理
        - 引擎类型可以混用，但是外键不能跨引擎，使用一种引擎表的外键不能引用另一种使用不同引擎的表

```
CREATE TABLE students
(
	student_id	varchar(20) NOT NULL PRIMARY KEY,
    student_name	varchar(30) NOT NULL,
    student_grade	int NULL,
    age		int NULL,
    telephone_numbers	varchar(40) NOT NULL,
    student_email	varchar(30)  NULL
)
```
主键也可在最后一行写出：`PRIMARY KEY(student_id)`

- 变更表——ALTER TABLE
    - 必须给出：要更改的表名（必须存在）、要做更改的列表
    - 新增一列：ALTER TABLE 表名 ADD 列名 定义
    - 删除一列：ALTER TABLE COLUMN 列名 
    - 常见用途：定义外键
```
ALTER TABLE orderitems
ADD CONSTRAINT fk_orderitems_orders
FOREIGN KEY (order_num) REFERENCES orders(order_num)
```
- 表结构变更的一般步骤
    - 新的列布局创建新表
    - 使用INSERT SELECT 语句
    - 将旧表数据复制到新表 
    - 验证新表所需数据
    - 重命名旧表
    - 重命名新表
    - 根据需要创建触发器、存储过程、索引、外键
- 删除表——DROP TABLE 表名 
- 重命名表——`RENAME TABLE 旧表名 TO 新表名`

## 第22章 使用视图
- 视图——虚拟的表
    - 使用时动态检索数据的查询，一般用于数据检索而不是更新  
    - 常见用途：
        - 重用SQL语句
        - 简化复杂的SQL操作
        - 使用表的组成部分而不是整张表
        - 保护数据，对用户授予表特定部分的访问权限而不是整张表
        - 更改数据格式和表示
    - 视图本身不包含数据，返回的数据是从其他表中检索出来的
    - 常见规则和限制：
        - 视图名称必须唯一
        - 视图创建数目没有限制
        - 创建视图，操作者需要有足够的访问权限
        - 视图可以嵌套
        - 可以用ORDER BY 
        - 视图不能被索引，也不能有与之相关联的触发器和默认值
        - 视图可以和表一起使用

- 视图的使用和更新
    - CREATE VIEW viewname AS +SELECT 语句创建视图
    - SHOW CREATE VIEW viewname 查看创建视图的语句
    - DROP VIEW viewname 删除视图
    - 更新视图：可以先用DROP再用CREATE,也可以CREATE OR REPLACE VIEW,会在视图不存在时创建，存在时替代
        - 视图是可更新的，可以使用INSERT/DELETE/UPDATE语句，更新视图就是更新其基表
        - 当视图定义中有分组（GROUP BY 和HAVING）、子查询、连接、UNION、聚合函数、DISTINCT、派生（计算）列，则
        视图是不可更新的

## 第23章 存储过程
- 存储过程
    - 为以后的使用而保存的一个或多个MySQL语句的集合
    - 优点：简化复杂操作、保证数据的完整性、简化对变动的管理、提高性能
    - 执行存储过程：调用，`CALL 存储过程名 (参数)`,可以显示结果、也可以不显示结果
    - 创建存储过程：CREATE PROCEDURE 存储过程名()  
    BEGIN  
        SELECT 语句  
    END
    - DELIMITER ：定界符/分隔符，默认定界符";"，在创建
    存储过程时，内部的SQL语句用分号隔开，创建过程会被提前终止，需要临时改变定界符
    除了\，任何字符都可以当做定界符，只要字符在SQL中没有特殊含义
    ```
    DELIMITER //（//作为定界符）
    创建存储过程
    DELIMITER ;(恢复默认定界符)
    ```

```
DELIMITER //
CREATE PROCEDURE product_pricing()
BEGIN 
    SELECT AVG(prod_price) AS priceaverage
    FROM products;
END//
DELIMITER ;

调用：CALL product_pricing()
```
    
- 删除存储过程
    - DROP PROCEDURE 存储过程名
    - 存储过程名后不需要括号，上例中是：DROP PROCEDURE product_pricing
    - 仅当存在时删除：DROP PROCEDURE IF EXISTS 存储过程名

- 使用参数
    - 变量：内存中一个特定的位置，用来临时存储数据
    - IN:传递给存储过程
    - OUT:从存储过程传出
    - INOUT:对存储过程传入传出
    - 参数允许的数据类型和表中的数据类型相同
    - 所有MySQL变量必须以@开头

```
DELIMITER //
CREATE PROCEDURE product_pricing(
    OUT pm DECIMAL(8,2),
    OUT ph DECIMAL(8,2),
    OUT pa DECIMAL(8,2)
BEGIN
    SELECT MIN(prod_price)
    INTO pm 
    FROM products;
    SELECT MAX(prod_price)
    INTO ph
    FROM products;
    SELECT avg(prod_price)
    INTO pa 
    FROM products;
END//
DELIMITER ;
调用：CALL product_pricing(@pricelow,@pricehigh,@priceaverage);
显示结果：SELECT @pricelow,@pricehigh,@priceaverage;
)
```
- 检查存储过程
    - SHOW CREATE PROCEDURE 存储过程名
    - 列出所有存储过程：SHOW PROCEDURE STATUS，可以用LIKE过滤

## 第24章 使用游标

- 游标cursor是一个存储在MySQL服务器上的数据库查询，是被SELECT语句检索出来的结果集
- 游标用于交互式应用程序，可以滚动浏览数据
- MySQL游标只能用于存储过程和函数
- 使用游标
    - 使用游标前必须声明
    - 声明后必须打开游标以供使用
    - 游标中有数据后可以根据需要逐行获取数据
    - 游标结束使用时必须关闭游标
- 创建游标——DECLARE语句 
    - `DECLARE 游标名 CURSOR FOR +SELECT 语句`
    - 存储过程处理完成后，游标就会消失
```
DELIMITER //
CREATE PROCEDURE process()
BEGIN
    DECLARE ordernum CURSOR
    FOR 
    SELECT order_num FROM orders;
END //
DELIMITER ;
```

- 打开和关闭游标
    - 打开游标：OPEN 游标名
    - 关闭游标：CLOSE 游标名
    - CLOSE 会释放游标使用的所有的内存和资源
    - 关闭后的游标再次使用需要打开，声明过的游标不需要再次声明
    - END 会自动关闭游标
- 使用游标数据
    - FETCH语句分别访问游标的每一行，可以指定检索数据、指定数据存储的位置、移动
    游标中的内部行指针
    - 格式：FETCH 游标名 INTO 变量名

    ```
    DELIMITER //

    CREATE PROCEDURE processorders()
    BEGIN 
        -- 声明变量
        DECLARE done BOOLEAN DEFAULT 0;
        DECLARE o INT;

        DECLARE ordernumbers CURSOR 
        FOR 
        SELECT order_num FROM orders;

        -- 声明继续处理程序
        DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;

        OPEN ordernumbers;
        REPEAT
            FETCH ordernumbers INTO o;
        UNTIL done END REPEAT;
        CLOSE ordernumbers;
    END//
    DELIMITER ;
    ```
## 第25章 使用触发器
- 触发器：MySQL响应DELETE、UPDATE、INSERT语句而自动执行的一个MySQL语句
- 创建触发器
    - 需要给出的信息：唯一的触发器名（每张表）、触发器关联的表、触发器应该响应的活动、触发器何时执行（处理之前或处理之后）
    - 格式：`CREATE TRIGGER 触发器名 BEFORE/AFTER +活动+操作`        
    - 仅表支持触发器，视图/临时表不支持，每张表的每个事件每次只允许有一个触发器，每张表最多有6个触发器（5.7.2以前，以后数量不受限制）
    - BEFORE 触发器失败，则不执行对应的请求操作；BEFORE 触发器或语句失败，则不会执行AFTER触发器
- 删除触发器
    - 语法：`DROP TRIGGER 触发器名`
    - 修改触发器要先删除再重新创建
- 不同的触发器
    - INSERT 触发器
        - INSERT语句执行前/后执行
        - INSERT 触发器代码内，可以引用一张名为new的虚拟表访问被插入的行
        - 在BEFORE INSERT 触发器中，NEW的值可以被更新（允许更改被插入的值）
        - 对AUTO_INCREMENT列，NEW在执行INSERT语句之前包含0，在执行后包含新的自动生成值

    ```
    DROP TRIGGER IF EXISTS newvendor;
    DELIMITER //
    CREATE TRIGGER newvendor BEFORE INSERT ON vendors
    FOR EACH ROW 
    BEGIN
         SET NEW.vend_state=UPPER(NEW.vend_state) ;

    END//
    DELIMITER ;
    ```

    - DELETE 触发器
        - DELETE 触发器代码内，可以引用一张名为OLD的虚拟表访问被删除的行
        - OLD中的值只读，不更新
    - UPDATE 触发器
        - UPDATE 触发器代码内，可以引用一张名为OLD的虚拟表访问更新前的值，
        引用一张名为NEW的虚拟表访问更新后的值，
        - 在BEFORE UPDATE 触发器中，NEW的值也可以被更新，可以更改要用于UPDATE语句的值
        - OLD中的值只读，不更新
- 使用触发器
    - 创建触发器可能需要特殊的安全访问权限
    - 用触发器保持数据的一致性
    - 触发器用来创建审计跟踪，可以把更改记录到另一张表
    - MySQL触发器不支持CALL，不能从触发器调用存储过程，只能将存储过程复制到触发器内

## 第26章 管理事务处理
- 事务处理
    - MyISAM不支持事务处理管理，InnoDB支持
    - 维护数据库的完整性，保证成批的MySQL语句要么全部执行，要么全部不执行
    - 管理必须成批执行的MySQL操作，保证一组操作不会中途停止，要么完全执行，要么完全不执行，发生错误会进行回滚
    - 事务：一组SQL语句
    - 回滚：撤销指定SQL语句的过程
    - 提交：将未存储的SQL语句结果写入数据库表
    - 保存点：事务处理中设置的临时占位符，可以对其进行回滚

- 控制事务处理
    - 将MySQL语句分解成逻辑块，明确规定数据何时应该回滚，何时不应该回滚
    - 标识事务的开始 `START TRANSACTION`
    - 回滚 `ROLLBACK`
    - ROLLBACK语句只能在一个事务内部使用，在执行一个 `START TRANSACTION`命令之后
    - INSERT/DELETE/UPDATE 语句可以回滚
    - 使用COMMIT
        - COMMIT语句仅在不出错的情况下会写入变更，当出错时，之前的变更会被自动撤销
    - 隐式事务关闭：COMMIT或ROLLBACK执行后，事务会自动关闭
    - 保存点
        - 当需要进行回滚到某个占位符时，占位符称为保存点
        - 创建占位符：`SAVEPOINT 占位符名`，每个保存点都有唯一名称标识
        - 回滚到某个占位符：`ROLLBACK TO 占位符名`
        - 保存点越多越好
        - 事务处理完成后保存点会自动释放，也可以执行`RELEASE SAVEPOINT 保存点名称`
    - 更改默认的提交行为
        - MySQL默认执行一个MySQL语句，更改会立刻生效
        - 指示MySQL不自动提交更改：`SET autocommit=0;`
        - autocommit 标志决定是否自动提交更改，为0是不自动提交更改，直到为1或真才是自动提交更改
        - autocommit 标志是针对每个连接而不是服务器
    
## 第27章 全球化和本地化
- 字符集和校对顺序
    - 字符集：字母、符号的集合
    - 编码：某个字符集成员的内部表示
    - 校对：规定字符集如何比较的指令
- 使用字符集和校对顺序
    - 查看MySQL支持的字符集的所有列表：`SHOW CHARACTER SET`,显示所有字符集及描述和默认校对
    - 查看所支持校对的所有列表：`SHOW COLLATION`，显示所有可用校对及适用的字符集
    - 确定所有的字符集和校对：`SHOW VARIABLES LIKE 'character%'`,`SHOW VARIABLES LIKE 'collation%' `
    - 可以在创建表时指定字符集和校对，如果只指定了CHARACTER SET 没有指定COLLATE,则用此字符集及其默认的校对
    - 可以对表的某一列指定字符集和校对
    - 可以在SELECT语句中的ORDER BY 子句中指定与创建表时不同的校对顺序
    - COLLATE 子句也可以在GROUP BY,HAVING,聚合函数，别名中使用
    - 字符串可以在字符集之间转换，用 CAST()/CONVERT()函数 

## 第28章 安全管理
- 访问控制
    - 提供用户所需的访问权限并只提供访问权限
    - 仅在绝对需要时使用root，不要在日常的MySQL操作中使用root
- 用户管理
    - MySQL用户账号和信息存储在名为mysql的MySQL数据库中
    - 获取所有用户账号 `USE mysql;SELECT user FROM User;`
    - 创建用户账号
        - `CREATE USER 用户名 IDENTIFIED BY 用户密码;`
        - 可以使用GRANT或 INSERT（不推荐直接插入）
    - 重命名用户
        - RENAME USER 原用户名 TO 新用户名
    - 删除用户
        - DROP USER 用户名
        - 删除用户账号和其所有用户权限
    - 设置访问权限
        - 查看授予用户账号的权限：`SHOW GRANTS FOR 用户名`
        - "USAGE ON *.*":表示根本没有权限
        - MySQL权限是通过用户名和主机名组合定义的，不指定主机名，默认主机名是%
        - 使用GRANT设置权限，给出：要授予的权限、被授予权限的数据库/表、用户名
        - 格式：`GRANT 权限 ON 数据库名.表名 TO 用户名`
        - 回收特定权限：`REVOKE 权限 ON 数据库名.表名 TO 用户名`
        - GRANT 和 REVOKE 可以在：整个服务器、整个数据库、整个表、特定列、特定存储过程控制访问权限
        - 使用GRANT 和REVOKE时，用户账户必须存在，但是所涉及的对象没有这个要求，允许管理员在创建数据库/表之前
        实现安全措施
        - 可以用','连接多个GRANT语句

## 第29章 数据库维护
- 备份数据
    - 使用命令行工具mysqldump将数据库内容导出到某个外部文件
    - 使用MySQL的`SELECT INTO OUTFILE 系统文件名`，系统文件必须不存在

- 进行数据库维护
    - 更新表的统计信息：`ANALYZE TABLE +表名`
    - 检查表的各种问题：`CHECK TABLE +表名`
- 诊断启动问题
- 查看日志文件：错误日志、查询日志、二进制日志、慢查询日志

## 第30章 性能优化
- 显示所有活动进程和他们的线程及时间：SHOW PROCESSLIST

        
