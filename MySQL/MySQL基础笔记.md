# MySQL数据库服务器

什么是数据库
存储数据的仓库。
常见的数据库: MySQL、 Oracle、 Sqlserver、 DB2等。

## 1、MySQL简介

MySQL是一个关系型数据库管理系统，由瑞典MySQL AB 公司开发，目前属于 Oracle 旗下产品

MySQL结构：

![](imgs\1.jpg)

在一个MySQL服务器中有多个数据库，每个数据库又有多个数据表。 数据是存储在数据表当中的。

数据表的结构和excel一模一样

![](imgs\2.jpg)

表结构: 
    和excel表的结构是一样的。
    每一列都是一类数据 --- 字段
    每一行代表一条数据 --- 记录

## 2、安装客户端

MySQL是一款C/S结构的软件。

MySQL本身是服务器端。 phpStudy集成的是MySQL服务器

常见的客户端: CMD 、 Navicat、 Sqlyog、 phpmyadmin等等

![](imgs\3.png)



安装 Navicat

1）解压

![](imgs\4.jpg)

2) 选择安装版本

![](imgs\5.jpg)

3)选择安装路径

![](imgs\6.png)

 继续下一步...

4)链接服务器

![](C:\Users\Administrator\Desktop\MySQL\imgs\7.jpg)

 ① 点击“链接”按钮 ---  选择要链接的数据库种类

![](imgs\8.jpg)

 

 ② 配置链接信息

   用户名： root       该用户是MySQL服务器系统的最高用户，拥有该系统的所有权限
   密码：  root             phpstudy中MySQL系统root用户的默认密码

![](imgs\9.jpg)

 

  ③ 点击“localhost”结果

![](imgs\10.jpg)

 

左侧的localhost下的内容都是数据库名称。

  information_schema、mysql、performance_schema 这三个是系统数据库（千万别动）。
  其他的都是自建数据库

看到上图，说明已经使用navicat 客户端正常链接到了 MySQL服务器了。

## 3、创建/删除数据库

操作数据库需要使用SQL语句（结构化查询语言）

### 1) 创建数据库

~~~mysql
语法格式: create database 库名;

示例：create database study；   创建一个名为 study 的数据库
	 create database cms；     创建一个名为 cms 的数据库
~~~

步骤：

![](imgs\11.png)

![](imgs\12.png)

![](imgs\13.png)

### 2) 删除数据库:

语法格式:  drop database 库名;
示例:     drop database school;  删除school库

~~~mysql
	drop database cms;      删除cms库
~~~

![](imgs\14.png)

## 4、创建数据表

### 4.1 创建表的基本格式

语句格式:

CREATE TABLE 表名 (
       列名     数据类型(长度)    完整性约束条件,
       列名     数据类型(长度)    完整性约束条件,
       ......
) engine=myisam default charset=utf8

**注意事项:**
   列名、数据类型、长度是必有的； 
   完整性约束条件可以没有；
   最后的 utf8 没有 -

### 4.2 创建用户表 -- users

案例:  在study数据库中，创建一张users数据表，该表中只保存用户id和用户名两个字段

一定要先选中 study 库，再去打开查询编辑器

id   int (5) :  id字段是整型的，长度是5。  特点： 整型限制长度无效

name varchar(20): name字段是字符串类型，长度是20 

## 5、数据类型和完整性约束条件

### 5.1 数据类型

#### 1) 整型:  

| 数据类型   | 解释                                            | 特点               |
| ---------- | ----------------------------------------------- | ------------------ |
| int(n)     | 整型 n  =  (-2,147,483,648  ~   2,147,483,647 ) | 整型限制长度无效   |
| tinyint(n) | 微整型 n  =  (-128 ~~ 127)                      | 取值区间小，速度快 |

#### 2) 字符串:

| 数据类型   | 解释         | 特点                                            |
| ---------- | ------------ | ----------------------------------------------- |
| varchar(n) | 可变长度     | 存储abc，varchar会占用3位长度                   |
| char(n)    | 固定长度     | 存储abc，char会占用30位长度                     |
| 执行速度   | 两者速度比较 | char的执行速度比varchar快，所以能用char就用char |

#### 3) 枚举: 

| 数据类型                   | 解释                                                        | 特点         |
| -------------------------- | ----------------------------------------------------------- | ------------ |
| enum(a, b, c , d, e,.....) | enum(‘男’, ‘女’) ；enum(‘武侠’, ‘玄幻’, ‘恐怖’, ‘都市言情’) | 只能选择一个 |
| set(a, b, c, d, e,......)  | set('古装','喜剧','警匪','恐怖','穿越','动作')              | 可以多选     |

#### 4) 日期: 

~~~mysql
date :   年-月-日
datetime :  年-月-日  时:分:秒
(time： 时:分:秒)
int: 存储时间戳
~~~

#### 5) 文本: text  大文本

应用于文章等字符多的时候

### 5.2 完整性约束条件

 完整性约束条件，就是进一步限制字段中保存的数据

#### **unsigned**

1) **unsigned**: 无符号，只用在整型字段上。 将负数部分移动到正数部分

  ~~~mysql
tinyint unsigned  '无符号微整型 0~~255' 
int unsigned  '0~~42亿'
  ~~~

#### auto_increment

2) auto_increment: 自增长，只用在整型字段上。 

#### primary key

3) primary key: 主键。 

~~~mysql
作用: 用来标志唯一一行数据的。
特点: 唯一 、 非空 、经常和auto_increment结合使用。 主键和自增长配合就能确定唯一的一条数据了。 主键一般都会选择整型字段。
~~~

#### unique

4)  unique: 唯一。 该列中不能出现重复值

#### not null

5) not null: 非空。 该列中不能有 NULL 数据。

### 5.3 创建管理员基本信息表 （ali_admin）

所需字段:
    id: 主键 
    邮箱(账号、用户名):  字符串  唯一  非空
    昵称:  字符串  唯一  非空
    密码: 32位固定长度  非空
    手机: 11位固定长度
    性别: 枚举 
    生日: date
    个人介绍:   text
    添加时间:   int 时间戳

~~~mysql
create table ali_admin(
  admin_id int UNSIGNED auto_increment primary key,
  admin_email varchar(30) UNIQUE not null,
  admin_nickname varchar(30) UNIQUE not null,
  admin_pwd char(32) not null,
  admin_tel char(11),
  admin_gender enum('男','女','人妖') defualt '人妖',
  admin_brith datetime,
  admin_sign text,
  admin_addtime int UNSIGNED
)engine=myisam DEFAULT charset=utf8;
~~~

## 6、数据查询

语法格式: 

> SELECT  字段名1, 字段名2, ......   FROM 表名
>
> WHERE <条件表达式>     查询条件
>
> ORDER BY <字段名> [ ASC|DESC ]    顺序或倒序
>
> LIMIT  START, LENGTH   从数据库表中 START 条记录开始检索 LENGTH 长度条记录 

### 6.1 基本查询

格式:  select  字段名1, 字段名2,....  from  表名 

案例1: 查询所有栏目的id和名称
查询的表:  ali_cate
查询字段:  cate_id,cate_name

~~~mysql
select cate_id,cate_name from ali_cate
~~~

案例2: 查询管理员信息 (全部字段信息)
查询的表：ali_admin
查询字段： *   (通配符，代表该表中的所有字段)

~~~mysql
select * from ali_cate
注意： 在实际开发中，需要将所有的字段全列出来。因为 * 转字段是需要时间。
~~~

### 6.2 带where子句的查询

select  field1, field2... from 表名  查询表中的所有数据

where 可以使用条件来筛选查询出的结果

![](imgs\01.jpg)

案例3: 查询id值为2的栏目的所有信息
查询的表：  ali_cate
查询字段：  *
筛选条件： cate_id=2

~~~mysql
select * from ali_cate
where cate_id=2
~~~

案例4: 查询年龄大于等于25的管理员的邮箱和昵称
表： ali_admin
字段： admin_email, admin_nickname
筛选条件： admin_age>=25

~~~mysql
select admin_email, admin_nickname from ali_admin
where admin_age>=25
~~~

案例5: 查询年龄在23-28之间的管理员的所有信息
表： ali_admin
字段： *
筛选条件： admin_age>=23  and  admin_age<=28

~~~mysql
select * from ali_admin
where admin_age>=23  and  admin_age<=28
// 还可这么写
where admin_age between 23 and 28
~~~

![](imgs\15.jpg)

![](imgs\16.jpg)

案例6: 查询年龄不在23-28之间的管理员的所有信息
表： ali_admin
字段： *
筛选条件： admin_age  not  between 23 and 28

~~~mysql
select * from ali_admin
where admin_age not between 23 and 28
筛选方法2：
where admin_age<23 or admin_age>28
~~~

![](imgs\17.bmp)

~~~mysql
select * from ali_admin
where admin_age>25 and admin_gender='男'

后一句是在前一句基础上执行的:
select * from ali_admin
where admin_age>25

~~~

### 6.3 in关键词

集合:  一组相同类型的数据，使用()来包含，括号内使用 ， 分隔开
(1, 2, 3, 4, 5)    (‘潮科技’, ‘会生活’, ‘奇趣事’, ‘美奇迹’)  

案例8: 查询年龄为20、28的女性管理员信息
表： ali_admin
字段： *
筛选条件： (admin_age=20 or admin_age=28)  and  admin_gender='女'

~~~mysql
select * from ali_admin
where (admin_age=20 or admin_age=28)  and  admin_gender='女'
~~~

in 关键字来写

~~~mysql
筛选条件2：  admin_age  in  (20,28)  and admin_gender='女'
select * from ali_admin
where admin_age  in  (20,28)  and admin_gender='女'
~~~

### 6.4 模糊查询

通配符:

  %: 代表任意长度(包括0)的任意字符
  _:  代表1位长度的任意字符

like: 在执行模糊查询时，必须使用like来作为匹配条件

~~~mysql
a%b :  ab  abb  a对萨达b  
a_b: acb  atb  
a_b%:  acb  a&baaad
~~~

案例9: 查询邮箱地址中包含字符y的管理员信息
表：ali_admin
字段：*
筛选条件： admin_email like  '%y%'

~~~mysql
select * from ali_admin
where admin_email like '%y%'
~~~

![1540307020619](imgs\19)

案例10: 查询邮箱以a字符开头并且包含n的管理员信息
表： ali_admin
字段： *
筛选条件： admin_email  like  'a%n%'

~~~mysql
select * from ali_admin
where admin_email like 'a%n%'
~~~

![](C:\Users\Administrator\Desktop\MySQL\imgs\20.bmp)

### 6.5 order by 排序
order by 可以对查询结果按某个字段的升降进行排序
升序 asc （默认值） ，  降序 desc 
可进行排序的字段通常是  整型  英文字符串型  日期型  (中文字符串也行,但一般不用)

案例11: 查询所有的栏目信息，并按别名的降序排列
表： ali_cate
字段：  *
筛选条件： 无
排序： order  by  cate_slug  desc

~~~mysql
select * from ali_cate
order by cate_slug desc
~~~

![](C:\Users\Administrator\Desktop\MySQL\imgs\21.bmp)

案例12: 查询所有栏目信息，并按发布时间升序排列
表： ali_cate
字段： *
筛选条件：无
排序： order  by  cate_addtime  asc

~~~mysql
select * from ali_cate
order  by  cate_addtime  asc
~~~

案例13: 查询所有男性的管理员信息，并按照创建时间降序排列
表： ali_admin
字段： *
筛选条件： admin_gender='男'
排序： order by  admin_addtime  desc

~~~mysql
select * from ali_admin
where admin_gender='男'
order by admin_addtime desc
~~~

![](imgs\22.bmp)

### 6.6 limit 限制

limit用来限制查询结果的起始点和长度
 格式:  limit  var1, var2
 var1: 起始点。 查询结果的索引，从0开始。 0代表第一条数据
 var2: 长度。就是查询几条数据

![1540308292377](imgs\23.png)

例14: 查询年龄最大的3名男性管理员的信息
表： ali_admin
字段： *
筛选条件： admin_gender='男'
排序： order by admin_age desc
限制： limit 0,3

~~~mysql
select * from ali_admin
where admin_gender='男'
order by admin_age desc
limit 0,3
~~~

![1540308440383](imgs\24.png)

limit通常用于分页程序：

  第一页： select * from  goods  limit  0,20
  第二页： select * from  goods  limit  20,20
  第三页： select * from  goods  limit  40,20
  ......

![1540308644372](imgs\25.png)

## 7、关系型数据库

### 7.1 关系型数据库简介

案例: 创建一个数据表，能够保存学生的基本信息(学号、姓名、年龄等)和学生每一科的考试成绩

1)  一张表的形式

![1540308757987](imgs\26.png)

缺点: 重复数据太多（数据冗余）

2) 关系型数据库:

![1540308861461](imgs\27.png)

使用多张数据表联合保存数据。

核心重点： 字段的对应关系
sc表中的 sno要和  student表中的sno对应，sc.sno的值一定要存在于 student.sno
sc表中的 cno 要和 course表中的cno对应，sc.cno的值一定要存在于course.cno

缺点: 表多
优点:
   数据耦合性低
   每个数据表都能够独立管理

### 7.2 student 和 dept表

目标: 创建student和dept表用来存储学生的基本信息、学院基本信息和学生所属的学院信息

![1540308995302](imgs\28.png)

student：学生表，所需字段  学号、姓名、性别、年龄、系别
dept：系别表，所需字段  系号  系名‘

~~~mysql
create table student(
  sno int UNSIGNED auto_increment PRIMARY key comment '学号 主键',
  sname varchar(10) not null comment '姓名',
  sage tinyint UNSIGNED not null comment '年龄',
  sgender enum('男','女','人妖') default '人妖' comment '性别',
  dno tinyint UNSIGNED comment '系别'
)engine=myisam default charset=utf8;


create table dept(
  dno tinyint UNSIGNED auto_increment primary key,
  dname varchar(20) UNIQUE not null
)engine=myisam default charset=utf8;
~~~

### 7.3 ali_admin、ali_cate和ali_article的关系
ali_admin:  管理员信息表
ali_cate: 栏目信息表
ali_article: 文章表

![1540309129957](imgs\29.png)

ali_cate表（栏目表）： 栏目id (主键)、 栏目名、栏目别名、栏目创建时间
ali_article（文章表）： 文章id（主键）、文章标题、文章内容、文章作者、所属栏目、发布时间、文章状态（草稿、已发布）

## 8、多表查询

关键词: join   on
格式：
链接条件一定是   表1的某个字段 =  表2的某个字段

目标: 查询学生的基本信息，但要显示系名而不是系号
表： student， dept
字段： sno,sname,sage,sgender,dname
链接字段：  student.dno=dept.dno

~~~mysql
select sno, sname, sage, sgender, dname from student
join dept on student.dno=dept.dno
~~~

![1540309499976](C:\Users\Administrator\Desktop\MySQL\imgs\30.png)

理解方式:

![1540309559422](imgs\31.png)

案例1: 查询所有文章信息，作者使用昵称显示
表：  ali_article， ali_admin
字段： ali_article.*,  ali_admin.admin_nickname
关联关系： article_adminid=admin_id

![1540309664599](imgs\32.png)

案例2: 查询大牛发布的所有文章信息，作者使用昵称显示
表： ali_article，ali_admin
字段： ali_article.*, ali_admin.admin_nickname
链接条件： article_adminid=admin_id
筛选条件： admin_nickname='大牛'

![1540309739823](imgs\33.png)

案例3: 查询属于潮科技和奇趣事的所有文章
表： ali_article， ali_cate
字段： ali_article.*, ali_cate.cate_name
链接条件： article_cateid=cate_id
筛选条件： cate_name  in ('潮科技','奇趣事')

![1540309805891](imgs\34.png)

案例4: 查询所有文章信息，作者使用昵称显示，栏目使用栏目名显示
表： ali_article，ali_admin，ali_cate
字段： ali_article.*, ali_admin.admin_nickname, ali_cate.cate_name
链接条件：
​      article_adminid=admin_id
​      article_cateid=cate_id

![1540309940390](imgs\35.png)

多表联合查询也可以使用筛选、排序、限制

![1540309979404](imgs\36.png)

## 9、添加数据

格式:  insert into 表名(字段名1，字段名2,....)  values(值1，值2，....)

 注意: **字段的顺序要和值的顺序是完全匹配的**
           **自增长类型的主键，可以使用null来填充；MySQL会自动填充数据**
           **如果每个字段都有数据，那么表名后面可以不跟字段名，但是values里面的顺序必须正确**

 案例1: 向ali_cate表中添加一条数据
![1540310089478](imgs\37.png)

1) 使用 null  来填充主键，因为主键是自增长的所以MySQL会自动填充该值
2) 因为填充了ali_cate表的每一个字段信息，所以ali_cate之后不需要再提供字段名称了

![1540310178990](imgs\38.png)

 案例2: 向ali_admin表中添加一条数据
 只有： 邮箱   昵称   年龄   性别

![1540310248384](imgs\39.png)

 **单独添加某几个字段的数据时，字段列表和数据列表都要有，并且顺序要一致**

## 10、修改数据

格式:  
  update  表名   set   字段1=值1, 字段2=值2,...  where  修改条件
  修改表中的哪一条（几条）数据的 字段1=值1...

案例1: 将id为6的管理员昵称改为凯子，性别改为女

![1540310321061](imgs\40.png)

案例2: 将所有男性管理员的年龄都+1
表： ali_admin
设置的值：  admin_age=admin_age+1
修改条件： admin_gender='男'

~~~mysql
update ali_admin set admin_age=admin_age+1
where admin_gender='男'
~~~

![1540310498177](imgs\41.png)

## 11、删除数据

格式:  delete from 表名  where 删除条件

案例: 删除cate_id=5的栏目

![1540310614783](imgs\42.png)