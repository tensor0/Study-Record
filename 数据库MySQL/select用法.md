### 基本的select语句
#### 最基本的select ... from ... 结构 
(视频p15)
```sql
# 选出employee表中的全部列
select * from employees

# 筛选出employee表中的employee_id列，last_name列，salary列
select employee_id,last_name,salary
from employees
```
1. '*'代表全部的(列名)字段名，from后面跟表名
2. 不同的列名要用逗号隔开

#### 修改为别名 列名+as+别名
as全称：alias(别名)
```sql
# 1. 用空格
# 筛选出employees表中的employee_id列，用别名emp_id代替，last_name列,department_id列
select employee_id emp_id,last_name,salary
from employees

# 2. 用as关键字 
# 筛选出employees表中的employee_id列，last_name列，用l_name代替，department_id列
select employee_id,last_name as l_name,salary
from employees

# 3. 用双引号""，不要使用单引号''，因为ANSI标准规定的双引号，MySQL不严谨（当别名有空格时用的，其他两种不可以起带空格的，除非加双引号）
# 筛选出employees表中的employee_id列，last_name列，用annual sal代替的salary列，
select employee_id,last_name ,salary "annual sal"
from employees
# 或
select employee_id,last_name ,salary as "annual sal"
from employees
```
#### 去除重复行 distinct+列名
```
# 查询员工表中一共有哪些部门id

# 错误的：没有去重的情况
select department_id 
from empolyees;
# 正确的：去重的情况
select distinct department_id 
from empolyees;

### distinct只能放在列名最前面，要不然对不上
###
```
#### 空值：null
1. 空值是null
2. null和0不是一回事
3. null参与的运算，结果也一定为null

#### 着重号 ``
1. 必须保证你的字段没有和关键字冲突，如果坚持使用，则需要用着重号``引起来

#### 查询常数 
1. 把想被查询的列名用常量代替，得到的是整列都是该常量
```sql
select "尚硅谷",empolyee_id,last_name
from employees;
```
#### 显示表结构 describe+表名
```sql
# 关键字
describe empolyees;

# 关键字缩写
desc deparments;
```


#### 过滤数据(筛选出满足某些条件的行) select ... from ... where ...
```
# 过滤条件，声明在from结构的后面

# 查询last_name为'King'的员工信息（ANSI标准规定字符串用单引号）
select * 
from employees
where last_name='King';
```

### 第4章_运算符


### 第5章_排序与分页
1. 排序数据 order by
```sql
# 排序
# 如果没有使用排序操作，默认情况下返回的数据是按照添加数据的顺序显示的
select * from empolyees;

# 使用order by对查询到的数据进行排序操作 
# (order:排序 by：通过xx order by：用 xx(列名) 进行排序)
# 默认order by 是升序排列(从第一行往后升序)
# 升序：ASC(ascend)
# 降序：DESC(descend)

# 练习：按照salary从高到低的顺序显示员工信息
select empolyee_id,last_name,salary
from employees
order by salary ASC;

#练习：按照salary从低到高的顺序显示员工信息
select empolyee_id,last_name,salary
from employees
order by salary DESC;

select empolyee_id,last_name,salary
from employees
order by salary;# 如果在order by后面没有显式指明排序方式的话，默认按照从低到高(升序)排列

#列的别名只能在order by中使用，不能在where中使用

# 强调格式：where需要声明在from后，order by之前(顺序错了会报错)

# 多列排序
#可以使用不被select的列排序
#在对多列进行排序时，首先排序的第一列必须有相同的列值，才会对第二列进行排序。如果第一列数据中的所有值都是唯一的，将不再对第二列进行排序

#练习：显示员工的信息，按照department_id降序排列，salary的升序排列
select department_id,last_name,salary
from employees
order by department_id desc,salary asc;
```
2. 分页 limit

```sql
# MySQL使用limit实现数据的分页显示

#需求1：每页显示20条记录，此时显示第1页
select employee_id,last_name,salary
from employees
limit 0,20;

#需求2：每页显示20条记录，此时显示第2页
select employee_id,last_name,salary
from employees
limit 20,20;

#需求2：每页显示20条记录，此时显示第3页
select employee_id,last_name,salary
from employees
limit 40,20;

#需求：每页显示pageSize条记录，此时显示第pageNo页
select employee_id,last_name,salary
from employees
limit (pageNo-1)*pageSize,pageSize;
```
3. where ... order by ... limit的声明顺序(不是执行顺序)
```
# limit的格式： 严格的来说，limit 位置偏移量，条目数
# 结构"limit 0,条目数"等价于"limit 条目数"
select employee_id,last_name,salary
from employees
where salary>6000
order by salary desc
limit 10;

#练习：表里有107条数据，我们只想显示第32、33条数据怎么办呢？
select employee_id,last_name,salary
from employees
limit 31,2;

#练习：查询员工表中工资最高的员工信息
select employee_id,last_name,salary
from employees
order by salary desc
limit 0,1;
```
注意：limit子句必须放到整个select语句最后
