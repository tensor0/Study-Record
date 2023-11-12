### 查询操作
基础语法：select 列名 from 表名;
实际上，select语句是由子句组合而成，子句是语句的一部分，不能单独使用。
select语句包含的子句(查询子句)主要有7种：
```
子句        说明
select      查询那些列
from        从哪个表查询
where       查询条件
group by    分组
having      分组条件
order by    排序
limit       限制行数
```
\
\
子句顺序 p92
```
select 列名
from 表名
where 条件
group by 列名
having 条件
order by 列名
limit n;
```

###数据库的操作
1. 创建数据库 p169
    ```
    //创建一个名字为test的数据库
    creat datebase test;
    creat schema test；//等价
    ```
2. 查看数据库 p171
    ```
    //查看当前可用数据库有哪些，注意databases后有s
    show databases;
    ```
3. 修改数据库 p172
    ```
    //修改test数据库的属性，把默认字符集修改成gb2312，把默认校对规则修改成gb2312_chinese_ci;
    alter database test
    default character set = gb2312
    default collate = gb2321_chinese_ci;
    ```
4. 删除数据库 p172
    ```
    //删除test数据库，但是被删除的数据库如果不存在就会报错
    drop database test;
    //删除test数据库，被删除的数据库不存在也不会报错
    drop database if exists test;
    ```
####表
创建好数据库后，我们就可以创建表了。一个数据库往往包含多个表。
0. 创建好数据库后，要选择对哪一个数据库进行操作 p175
    ```
    //对test数据库进行操作
    use test;
    ```
1. 创建表 p174
    ```
    /*
    * creat table 表名
    * (
    *   列名1 数据类型 列属性，
    *   列名2 数据类型 列属性，
    *   列名3 数据类型 列属性，
    *   ······
    *   列名n 数据类型 列属性
    * );
    */

    create table fruit
    (
        id      int primary key,
        name    varchar(10),
        type    varchar(10),
        season  varchar(5),
        price   decimal(5,1),
        date    date
    );
    ```
2. 查看表
    ```
    //查看当前数据库有哪些表
    show tables;
    ```
    ```
    //查看fruit表对应的创建代码
    show create table fruit;
    ```
    查看表结构
    ```
    //查看某个表的结构
    describe 表名;
    desc     表名;
    //查看fruit表结构
    describe fruit;
    ```
3. 修改表
    1. 修改表名
    ```
    alter table 旧表名
    rename to   新表名;
    ```
    2. 修改字段
    ```
    //1. 添加字段
    alter table 表名
    add 字段名 数据类型

    //给fruit表添加一个新的字段city，该字段类型为varchar(10)
    alter table fruit
    add city varchar(10);
    

    //2. 删除字段
    alter table 表名
    drop 字段名;
    
    //删除fruit表中的city字段
    alter table fruit
    drop city;


    //3. 修改字段名
    alter table 表名
    change 原字段名 新字段名 新数据类型;

    //将fruit表中的name字段重新命名为fname字段，并且类型设为varchar(10)
    alter table fruit
    change name fname varchar(10);


    //4. 修改数据类型
    alter table 表名
    modify 字段名 新数据类型

    //将fruit表中的price字段修改成int类型
    alter table fruit
    modify price int;
    ```
4. 复制表
    1. 只复制结构
    ```
    create table 新表名
    like 旧表名;

    //复制fruit表的结构，并根据这个结构创建新表，新表命名为fruit_a
    create table fruit_a
    like fruit
    ```
    2. 同时复制结构和数据
    ```
    create table 新表名
    as (select * from 旧表名);

    //复制fruit表的数据和结构创建新表，新表命名为fruit_b
    create table fruit_b
    as (select * from fruit);
    ```
    \
    **注意：
    creat table...like...
    creat table...as...
    的区别：
    create table ... like ...语句会复制旧表的完整结构，包括主键、自动递增、索引等。不过create table...like...语句只能复制结构，不能复制数据。
    create table...as...语句并不会复制旧表的完整结构，包括主键、自动递增、索引等。这些属性需要我们手动去添加。不过这个语句的优势就是可以复制数据**

5. 删除表
    ```
    drop table 表名;

    //删除fruit_a表,如果被删除的fruit表不存在会报错
    drop table fruit_a;
    //删除fruit_a表，如果被删除的fruit表不存在也不会报错
    drop table if exists fruit_a;
    ```
###列的属性
```
MySQL常见的列的属性：

属性            说明
--------        --------  
default         默认值
not null        非空(不允许为空)
auto_increment 自动递增
check           条件检查
unique          唯一键
primary key     主键
foreign         外键
comment         注释
```
列的部分属性又叫“列的约束”，比如默认值属性又叫作“默认值约束”，而非空属性又叫做“非空约束”。”约束“这种叫法很常见。
```
语法：
    /*
    * creat table 表名
    * (
    *   列名1 数据类型 列属性，
    *   列名2 数据类型 列属性，
    *   列名3 数据类型 列属性，
    *   ······
    *   列名n 数据类型 列属性
    * );
    */
```
**一个列可以拥有一个或多个属性，如果有多个属性，那么属性和属性之间需要使用空格来分隔。**

1. 默认值：
    使用insert语句可以只给部分列插入数据。对于没有被插入数据的列，他的值就会被设置为null。换一种说法就是：**默认情况下，列的默认值是null**。
    在MySQL中，如果希望列的默认值不是null，而是其他值，那么可以使用default这个属性来实现
    ```
    语法：
    列名 数据类型 default 默认值

    //创建fruit表，并让price默认值是10.0
    create tabel
    (
        id      int,
        name    varchar(10),
        type    varchar(10),
        season  varchar(5),
        price   decimal(5,1) default 10.0,
        date    date
    );
    ```
2. 非空
    在实际开发中，有时要求表中的某些列必须有值而不能是null，此时可以使用not null这个属性来实现
    ```
    语法：
    列名 数据类型 not null
    ```
3. 自动递增 p196    **(7条注意事项见书)**
    在实际开发中，很多时候我们希望某一列(如主键列)的值是自动递增的，此时可以使用auto_increment属性来实现
    ```
    语法：
    列名 数据类型 auto_increment=初始值
    ```
4. 条件检查 p200 **(MySQL8.0+)** 
    在实际开发中，我们可能会遇到这样一个场景：有一个age列，需要限制它的值在0-200，以防止输入的年龄值超过正常的范围
    在MySQL中，可以使用check属性来为某一列添加条件检查
    ```
    语法：
    列名 数据类型 check(表达式)
    ```
5. 唯一键 p201
    在MySQL中，如果我们希望某一列中存储的值都是唯一的，那么可以使用unique属性来实现。使用了unique属性的列，也叫作"唯一键"或“唯一键列”
    ```
    列名 数据类型 unique
    ```
    **注意：
    在MySQL中，唯一键是可以同时存在多个null值的；但是在SQL Sever中，唯一键只能存在一个null值** (p203有列子)
6. 主键 p205
    **某一列要想当主键，要具有两个特点：
    1.具有唯一性
    2.不允许为空(null)**
    ```
    语法：
    列名 数据类型 primary key
    ```
    **主键和唯一键的区别：
    1.主键的值不能为null，唯一键的值可以是null
    2.一个表只能有一个主键，但是一个表可以有多个唯一键
    3.主键可以作为外键，但是唯一键不可以作为外键**
7. 外键 p209
    在MySQL中，可以使用foreign key属性来设置一个外键。外键指的是子表中的某一列受限(或依赖于)父表中的某一列。
    **外键不一定是子表的主键**
    **外键可以重复**
    **插入数据时，必须插入父表，然后才能插入子表**
    **删除表时，必须先删除子表，然后才能删除父表**
    ```
    语法：
    constraint 外键名 foreign key(子表的列名) reference 父表名(父表的列名);
    ```
8. 注释 p214
    在MySQL中，可以使用comment属性来给列添加注释
    ```
    语法：
    列名 数据类型 comment 注释内容
    ```
9. 操作已有的表
    前面8个都是在创建表的同时给列添加属性。实际上，还可以给已经创建好的表的列添加属性，我们需要分以下两种情况来考虑。
    ```
    1. 约束型属性
    2. 其他属性
    ```
    1. 约束型属性：
        约束型属性分为以下四种：
        1. 条件检查
        2. 唯一键
        3. 主键
        4. 外键
    ```
    语法：
    //添加属性
    alter table 表名
    add constraint 标识名
    属性;

    //删除属性
    alter table 表名
    drop constraint 标识名;
    ```
    ```
    例子：
    //添加条件检查
    alter table fruit8
    add constraint ck_season
    check(season in ('春','夏','秋','冬'))；

    //删除条件检查
    alter table fruit8
    drop constraint ck_season;
    ```



