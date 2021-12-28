#### Linux系统运行mysql
- 在终端或命令行连接数据库
```mysql -uroot -p```

##### 与数据库相关的sql
1. 查看所有数据库
```show databases```
2. 创建数据库
```create database 数据库名;```
3. 查看数据库详情
```show create database 数据库名;```
4. 创建数据库指定字符集
```create database 数据库名 character set utf8/gbk;```（二选一）
5. 修改数据库指定字符集
```alter database 数据库 character set 字符集;```(utf8/gbk)
6. 删除数据库
```drop database 数据库名;```
7. 使用数据库
```use 数据库名;```

##### 与表相关sql
1. 创建表
```
create table 表名(
	字段1名 字段1类型,
    字段2名 字段2类型
);
```
2. 查询所有表
```show tables```
3. 查看表字段
```desc 表名```
4. 查看表详情
```show create table 表名```
5. 表引擎：
	- innodb：支持数据库的高级操作包括：是无、外键等
	- myisam：只支持数据基础的增删查改操作
6. 创建表指定引擎和字符集
```
create table 表名(
	字段1名 字段1类型,
    字段2名 字段2类型
) engine=myisam charset=gbk;
```
7. 删除表
```drop table 表名```
8. 修改表名
```rename table 原名 to 新名;```
9. 修改表引擎
```alter table 表名 engine=myisam/innodb charset=utf8/gbk;```
10. 添加表字段
 - 最后面添加格式：```alter table 表名 add 字段名 字段类型;```
 - 最前面添加格式：```alter table 表名 add 字段名 字段类型 first;```
 - xxx后面添加格式：```alter table 表名 add 字段名 字段类型 after xxx;```
11. 删除表字段
 - alter table 表名 drop 字段名;
12. 修改字段名和类型
 - alter table 表名 change 原名 新名 新类型;(*类型必须有*，但是可以不改)
13. 修改字段类型和位置
 - alter table 表名 modify 字段名 新类型 first;/after xxx;


##### 与数据相关的sql
	create table person(id int,age ing,name varchar(10));
1. 插入数据
 - 全表插入格式：insert into 表名 values(值1,值2，值3)；
 - 指定字段格式：insert into 表名 (字段1,字段2,字段3)values(值1,值2，值3)；
 - 批量插入：insert into 表名 values(值1,值2，值3)，(值1,值2，值3)，(值1,值2，值3)；
   insert into 表名 (值1,值2，值3) values(值1,值2，值3)，(值1,值2，值3)，(值1,值2，值3)；
2. 查询数据
 格式：select 字段信息 from 表名 where 条件;
  - select * from 表名;
  - select 值1 from 表名;
  - select 值1,值1 from 表名;
  - select * from 表名 where name='xiao';
3. 修改数据
 - 格式：update 表名 set 字段名 =值，字段名=值 where 条件;
 	update person set age=35 where id>4;(name='tom'等格式)
4. 删除数据
 - delete from 表名;//删除所有数据（可以回退）
 - truncate table 表名;// truncate是DDL语句，立即生效，删除所有数据（无法回退，表中的数据量较大时，效率高）
 - delete from 表名 where 条件;

#####中文乱码问题
 - insert into person values(10,"中文",18);
  //以上代码个别人会出现执行出错的情况
  //通过在终端中执行set names gbk;解决
 - 如果出现的不时报错二是查询数据时出现乱码
  //把现有的数据库和现有的表删除掉，重新创建表 保证字符集全部为utf8

##### 主键约束
- 什么是主键：用于表示数据唯一性的字段
- 什么是约束：就是创建表的时候给字段添加的限制条件
- 主键约束：c插入数据必须是唯一并且不能为空
- 格式：

```
	create table 表名(
    	id int primary key,
        name varchar(10)
    );
    insert into 表名 values(1,'刘备');
    insert into 表名 values(1,'关羽');//报错
    insert into 表名 values(2,'刘备');//不报错
```
##### 主键约束+自增
- 自增数值只增不减（从历史最大值基础上+1）
- 格式：
```
create table 表名(
    	id int primary key auto_increment,
        name varchar(10)
    );
    insert into 表名 values(null,'刘备');
    insert into 表名 values(null,'关羽');
```

##### 注释
- 对表的字段进行描述  ```comment 'z注释'```

```
create table 表名(
    	id int primary key auto_increment comment ‘这是主键字段’,
        name varchar(10) comment ‘这是姓名’
    );
   
```
##### ‘和`的区别
- ’是用来修饰字符串的
- `是用来修饰表名和字段名的 可以省略
```
create table `表名`(
    	`id` int primary key,
        `name` varchar(10)
    );
```

##### 数据冗余
- 如果数据库中的表设计不够合理，随着数据量的增长出现大量的重复数据，这种重复数据的现象称为数据冗余，通过**拆分表**的形式解决此问题。


##### 事务
- 什么是数据库中的事务
  事务是数据库中执行统一业务多条SQL语句的工作单元，可以保证多条sql语句全部执行成功或者全部执行失败
- 事务相关指令:
 1. 开启事务 begin；
 2. 提交事务 commit；
 3. 回滚事务 rollback；

```
create table user(id int primary key auto_increment,name varchar(10),money int,state varchar(5));
insert into user values(null,'钢铁侠',5000,'正常'),(null,'绿巨人',500,'正常'),(null,'超人',100,'冻结');
```
- 钢铁侠给绿巨人转账1000

```
update user set money=money-1000 where id=1 and state='正常';
update user set money=money+1000 where id=2 and state='正常';
```
- 钢铁侠给超人转账1000

```
update user set money=money-1000 where id=1 and state='正常';
update user set money=money+1000 where id=3 and state='正常';
```

- 在事务保护下执行

```
begin;//开启事务
update user set money=money-1000 where id=1 and state='正常';
//这里查询数据表，数据改变，另开窗口，数据不变
update user set money=money+1000 where id=3 and state='正常';
rollback;//转账失败 回滚事务
```
- savepoint; 保存回滚点
```
begin;
update user set money=money-1000 where id=1 and state='正常';
savepoint s1;
update user set money=money+1000 where id=3 and state='正常';
savepoint s2;
update user set money=money-1000 where id=2 and state='正常';
rollback to s2;//回滚到s2
```

##### SQL分类
1. DDL：数据定义语言
 - create,drop,alter(修改),truncate,不支持事务
 - truncate table 表名;//删除并创建新表 自增数值清零
2. DML:  数据操作语言
 - insert、delete、update。select（DQL），支持事务
3. DQL：数据查询语言
 - select
4. TCL：事务控制语言
 - begin、savepoint xxx、rollback、commit、rollback to xxx；
5. DCL：数据库控制语言
 - 分配用户权限相关的SQL

#####数据类型
1. 整数：常用类型int(m),bigint(m),m代表显示长度，需要结合zerofill关键字使用
```
create table t_int(id int(5) zerofill);
insert into t_int values(3);
```
2. 浮点数：常用类型 double（m，d）m代表总长度 d代表小数长度 例25.123（m=5，d=3） decimal超高精度浮点数，当涉及超高运算时使用
3. 字符串：char（m）固定长度 执行效率高 最大长度255 varchar（m）可变长度 节省资源 最大65535（超过255建议使用text，<255一般使用varchar） text可变长度 最大65535
4. 日期：date 只能保存年月日,默认null，time只能保存时分秒,默认null，datetime 最大值 9999-12-31 默认值为null， timestamp 最大值2038-1-19 默认值 当前的系统时间
```
create table t_date(t1 date,t2 datetime,t3 time,t4 timestamp);
insert into t_date values('2019-2-17','2039-2-17','16:4:20',null);
```

##### 导入*.sql文件
1. window系统
 - source d:/tables.sql;
2. linux 系统 把文件放在桌面
 - source /home/soft01/桌面/tables.sql

##### is null 和is not null
1. 查询奖金为null的员工信息，
```select * from 表 where 字段名 is null；```
2. 查询mgr不为null值的员工姓名
```select mgr from emp where name is not null；```

##### 别名
```
select ename as '姓名',sal as '工资' from emp；
select ename '姓名',sal '工资' from emp；
select ename 姓名,sal 工资 from emp；//只显示ename和sal列的数据
```
##### 去重
```select distinct job from emp；```

* * *

##### 比较运算符
- ！=或者<>都是代表不等于，<,>,<=,>=,=
- 语句后面加\G按行显示
```
select empno,ename,job,sal from emp where sal>2000;
select empno,ename,sal from emp where sal<=1600;
select ename,job,deptno from emp where deptno=20;
select ename,job from emp where job='menager';
select empno,ename,deptno from emp where deptno!=10;
select empno,ename,deptno from emp where deptno<10>;
select *from t_item where price=23;
select title,price from t_item where price!=8443;
```
##### and和or
- and 并且 && 需要同时满足多个条件时使用
- or或||      需要满足多个条件中的某一个条件时使用
```
select *from emp where deptno=20 and sal>2000;
select *from emp where deptno=10 and sal is null;
select *from emp where job='MANAGER' or mgr is null;
select *from emp where deptno=20 or sal<1000;
select *from emp where ename='king' or ename='james;
```
##### in
- 查询工资为5000，950，3000的员工信息
```
select *from emp where sal in(5000,950,3000);
相当于
select *from emp where sal=5000 or sal=950 or sal=3000;
```
- 查询工资不为5000，950，3000的员工信息
```
select *from emp where sal not in(5000,950,3000);
```
##### between x and y 包括xy
- 查询工资在2000到3000之间的员工信息
```
select * from emp where sal between 2000 and 3000;
```
- 查询工资<2000到3000>工资的员工信息

 ```select * from emp where sal not between 2000 and 3000;```

##### 模糊查询 like 'xxx’
- _代表单个未知字符
- %代表0或多个未知字符
 - 例子
 以a开头    a%
 以b结尾    %b
 包含c     %c%
 第一个字符是a 倒数第二个字符是b a%b_       倒数第三个  %b__
 匹配163邮箱  %163.com
 任意右相     %@%.com
- 查询员工姓名以k开头的员工信息

 ```
select * from emp where ename like 'k%';
else
select title,price from t_item where title like '%记事本%';
select * from t_item where price<200 and title like '%记事本%' \G;
select * from t_item where price not in(100,200) and title like '%联想%';
select * from t_item where category_id in(238,917) and title like '%齐心%';
select * from t_item where title not like '得力';
select ename,sal from emp where ename like '%a%' and sal<3000;
select * from emp where ename not like 'k%' and sal is not null;
select ename,job,deptno from emp where job like '%man%' and deptno=30;
```
##### 排序
- order by 字段名 asc/desc;(升序/降序)
1. 查询员工姓名和工资减序
```select ename,price from t_item order by sal desc;```
2. select *from emp where deptno=30 order by sal desc;
- 查询所有员工信息按照部门编号升序排序,工资降序，
```select *from emp order by deptno asc,sal desc;```
	- select *from t_item where title like '%燃%' order by price;
	- select title,price,category_id from t_item where title like '%dell%' order by category_id asc,price desc;

##### 分页排序
- limit 跳过的条数，请求的条数（每页的条数）
1. 查询员工表工资最高的前五条数据
```select *from emp order by sal desc limit 0,5;```
 - 以上数据的第二页数据
 - select *from emp order by sal desc limit 5,5;
- limit （页数-1）*每页的条数,每页显示的条数

##### concat()函数
- 可以将字符串进行拼接
- 查询员工每个员工和工资 要求工资显示单位元
```select ename,concat(sal,'元') from emp;```
- 查询商品表，显示商品名称，单价（价格：25元）
```select title,concat('价格：',price,'元') from t_item;```
- select concat('a','b');//ab
- select concat('hellow');//hellow

##### 数值计算 + - * / %  7%2==mod(7,2)等价
- 查询员工的姓名，工资，年终奖（年终奖=工资*5）;

 ```select ename,sal,sal*5 年终奖 from emp;//在显示的表中添加了年终奖的字段和值 ```

##### 日期相关函数
1. 获取当前日期+时间
   select now();
2. 获取当前年月日 和 时分秒
	select curdate(),curtime();
3. 从完整的年月日时分秒中 提取年月日 和时分秒
	select date(now());//date('1990-1-12');
    select time(now());
4. 从完整的年月日时分秒中提取时间分量 extract
	select extract(year/month/day/hour/minute/second from now());
    *查询每个员工的姓名和入职的年份 入职时间字段：hiredate
    ```select ename,extract(year from hiredate) 入职年份 from emp;```
5. 日期格式化 date_format()
 - date_format(时间,'格式');
 - 格式：
 	%Y 四位年    %y 两位年
    %m 两位月    %c 一位月
    %d 日
    %H 24小时   %h 12小时
    %i分        %s 秒
 - 把默认的时间格式转成 年月日时分秒
 select date_format(now(),'%Y年%m月%d日%H时%i分%s秒');
6. 把非标准时间格式转成标准格式 str_to_date();
 - str_to_date(字符串时间，格式)
 select str_to_date('2019年12月21日14时39分30秒','%Y年%c月%d日%H时%i分%s秒') 标准格式;
 
##### ifnull()
- age = ifnull(x,y) 如果x值为null则age=y 如果x不为null则age=x
- 把emp表中的奖金为null的改为0
 update emp set comm=ifnull(comm,0);

##### 聚合函数
- 对多行数据进行统计查询：求和 平均值 最大值 最小值 计数
- **求和**：sum(求和的字段)
	统计20号部门的工资总和
    select sum(sal) from emp where deptno=20;
- **平均值**：avg(字段)
	统计所有员工的平均工资
    select avg(sal) from emp;
- 最大值：max(字段)
	查询30号部门的最高工资
    select max(sal) from emp;
- 最小值：min(字段)
	查询30号部门的最低工资
    select min(sal) from emp;
- 计数：count(字段) -统计该字段除了null的值的数量，
	查询员工表中10号部门的员工数量
    select count(*) from emp where deptno=10;

##### 字符串相关函数
- char_length(str)获取字符串的长度
	select ename,char_length(ename) 名字长度 from emp;
- instr(str,substr)获取substr在str中出现的位置 从1开始
	select instr('asdfdaasdfgg','g');
- insert(str,start,length,newstr)将newstr插入
	select insert('abcdefg',3,2,'m');将m插入cd中，cd消失
- lower(str) upper(str) 大小写转换
	select lower('nAb'),upper('Nam');
- trim(str) 去掉两端空白
	select trim(' sd adf ');
- left(str,index) 从左边截取
- right(str,index)
- substring(str,index,length)从指定位置截取，没有length,截取到最后
- repeat(str,count)重复
	select reprat('ab',3);
- replace(str, old,new)替换
	select replace('123456789','4','7');
- reverse() 反转

##### 数学相关的函数
- floor(num) 向下取整
	select floor(3.56);//3
- round(num) 四舍五入 只保留整数
- round(num,m)m代表保留几位小数
- truncate(num,m)非四舍五入，m代表保留几位小数
- rand()  0-1的随机数
  select floor(rand()*6);//0-5的随机数
  select floor(rand()*4)+3;

* * *
#####分组查询
- select avg(age) 平均年龄 group by 字段名,字段名;
- 查询每个部门的平均工资
```	
	select deptno,avg(sal) from emp group by deptno;
    esle
    select deptno,avg(sal) from emp where sal>1000 group by deptno ;
    select deptno,max(sal) from emp group by deptno;
    select deptno,count(*) from emp group by deptno;
    select mrg,count(*) from emp group by mrg;
    select deptno,count(*),sum(sal) from emp group by deptno order by count(*),sum(sal) desc; 或者
    select deptno,count(*) c,sum(sal) s from emp group by deptno order by c,s desc;
    select deptno,avg(sal),min(sal),max(sal) from emp where sal between 1000 and 3000 group by deptno order by avg(sal);
    select mgr,count(*) c,sum(sal),avg(sal) a,min(sal) from emp where mgr is not null  group by job order by c desc,a;
    select avg(sal) from emp where avg(sal)>2000 group by deptno; //?
```

##### having (过滤掉)
- where后面自能写普通字段的条件，不能写聚合函数的条件
- having 后面可以写普通字段的条件，但是不建议这么做，having一般要和分组查询结合使用，后面使用聚合函数的条件，having写在分组查询的后面
1. 查询每个部门的平均工资，要平均工资大于2000
	select deptno,avg(sal) a from emp group by deptno having a>2000;
    else
    select category_id,avg(price) p from t_item group by  category_id having p<100;
    select category_id,avg(price) from t_item where category_id in(238,917) group by category_id;
    select deptno,count(*) c,sum(sal) s,avg(sal) a from emp where sal between 1000 and 3000 group by deptno having a>2000 order by a;
    select ename,count(*),sum(sal) s,max(sal) from emp where job is like 's%' group by job having avg(sal)=3000 order by c,s desc;
    select extract(year from hiredate) 入职人数,count(*) from emp group by extract(year from hiredate);
    select deptno,avg(sal) a from emp groud by deptno order by a desc limit(0,1); 

##### 子查询
- 查询emp中工资最高的员工信息
  select max(sal) from emp;
  select *from emp where sal=5000;
  写成
  select *from emp where sal=(select max(sal) from emp);
  else
  select *from emp where sal>(select avg(sal) from emp);
  select *from emp where job=(select job from emp where ename='Jones') and ename!='Jones';
  select *from emp where sla in (select min(sal) from emp group by job);
  select *from emp where hiredate=(select max(hiredate) from emp);
  select deptno from emp where ename='KING';
- 查询名字为king的部门编号和部门名称（需要用到dept表）
  select deptno,dname from dept where deptno=(select deptno from emp where ename='KING');
- 查询平均工资最高的部门信息
	select deptno,avg(sal) a from emp group by deptno order by a desc limit(0,1);

- 查询平均工资最高的部门信息(先获取最高平均值，再用最高值获取部门的no，再放进in里面获取部门的信息)
	select avg(sal) a from emp group by order by a desc limit 0,1;
    select deptno from emp group by deptno having avg(sal)=(select avg(sal) a from emp group by order by a desc limit 0,1);
    select *from dept where deptno in(select deptno from emp group by deptno having avg(sal)=(select avg(sal) a from emp group by order by a desc limit0,1));
- 子查询可以写在创建表的时候
	create table emp_10 as (select * from emp where deptno=10);
- 子查询还可以写在from后面 **当成虚拟表时必须有别名**
	select ename from (select * from emp where deptno=10) newtable;

#####关联查询
- 同时查询多张表的数据的查询方式称为关联查询
- **关联查询时必须写关联关系，如果不写会得到两张表的乘积，这个乘积称为笛卡尔积。这是一个错误的查询结果，切记工作中不要出现**
	select e.ename,d.dname from emp e,dept d where e.deptno=d.deptno;
- 查询部门地点在new york的部门名称以及该部门下所有的员工姓名
	select d.dname,e.ename from emp,dept where e.deptno=d.deptno and d.loc='new york';
    
##### 等值连接和内连接
- 关联查询的两种查询方式：
1. 等值连接：select * from x,y where x.age=y.age and x.gender='男';
2. 内连接：select *from a join b on a.age=b.age where a.gender='男';（两个表的交集部分）
3. 外连接（左外连接，右外连接）：select *from a  left/right join b on a.age=b.age where a.gender='男';（左表和两表的交集部分）
4. 自关联：把一张表当成两张表
- 查询每个员工的姓名和对应的部门名称
	select e.ename,d.dname from emp e join dept d on e.deptno=d.deptno;
    select d.*,a,name from......

#### 关联查询总结
- 关联查询的查询方式有几种？ 3种：等值连接、内连接、外链接
- 如果查询的是两张表的交集数据使用等值连接或内连接（推荐）
- 如果查询的数据是一张表的全部数据和另外一张表的交集数据使用外链接

##### 表设计之关联关系
- 什么是一对一：有AB两张表，A表中一条数据对应B表中的一条数据，同时B表中一条数据也对应A表中的一条数据
- 应用场景：用户表和用户信息扩展表，商品表和商品信息扩展表
- 如何建立关系：在从表中添加外键字段指向主表的主键
- 练习： 创建用户表user（id,username,password）  和扩展表userinfo(user_id,nick,loc) 并且保存以下数据
- 建表：
	create table user(id int primary key auto_increment,username varchar(10),password varchar(10));
	create table userinfo(user_id int,nick varchar(10),loc varchar(10));

	libai  admin    李白    建国门外大街
	liubei 12345    刘皇叔   荆州
	liudehua aabbcc  刘德华   香港
	insert into user values(null,'libai','admin'),(null,'liubei','12345'),(null,'liudehua','aabbcc');
	insert into userinfo values(1,'李白','建国门外大街'),(2,'刘皇叔','荆州'),(3,'刘德华','香港');
1. 查询libai的密码是什么
	select password from user where username='libai';
2. 查询每个用户的用户名和昵称
	select u.username,ui.nick
	from user u join userinfo ui
	on u.id=ui.user_id;
3. 查询刘德华的用户名
	select u.username
	from user u join userinfo ui
	on u.id=ui.user_id
	where ui.nick='刘德华';
####一对多
- 什么是一对多：有AB两张表：A表中一条数据对应B表的多条数据，同时B表中的一条数据对应A表的一条数据
- 场景： 员工表和部门表  商品表和商品分类表
- 如何建立关系：在多的表中添加外键指向另外一张表的主键
- 练习：创建t_emp(id,name,dept_id)和t_dept(id,name)
	create table t_emp(id int primary key auto_increment,name varchar(10),dept_id int);
	create table t_dept(id int primary key auto_increment,name varchar(10));
1. 保存以下数据：神仙部门的孙悟空和猪八戒，妖怪部门的蜘蛛精和白骨精
	insert into t_dept values(null,'神仙'),(null,'妖怪');
	insert into t_emp values(null,'孙悟空',1),(null,'猪八戒',1),(null,'蜘蛛精',2),(null,'白骨精',2);
2. 查询每个员工的姓名和对应的部门名称
	select e.name,d.name
	from t_emp e join t_dept d
	on e.dept_id=d.id;
3. 查询猪八戒的所在的部门名
	select d.name
	from t_emp e join t_dept d
	on e.dept_id=d.id where e.name='猪八戒';
4. 查询妖怪部的员工都有谁
	select e.name
	from t_emp e join t_dept d
	on e.dept_id=d.id where d.name='妖怪';
#### 多对多
- 什么是多对多：有AB两张表，A表中一条数据对应B表的多条数据，同时B表的一条数据对应A表的多条，称为多对多
- 应用场景： 老师和学生
- 如何建立关系：创建第三张关系表，在关系表中有两个字段指向另外两个表的主键
- 练习：创建老师表teacher(id,name)和学生表student(id,name)保存以下数据     关系表t_s(tid,sid)
- 创建表
	create table teacher(id int primary key auto_increment,name varchar(10));
	create table student(id int primary key auto_increment,name varchar(10));
	create table t_s(tid int,sid int);
		苍老师:小米、小红、小绿
		传奇哥：小白、小绿
	insert into teacher values(null,'苍老师'),(null,'传奇哥');
	insert into student values(null,'小米'),(null,'小红'),(null,'小绿'),(null,'小白');
	insert into t_s values(1,1),(1,2),(1,3),(2,3),(2,4);
1. 查询每个老师的姓名和对应的学生姓名
	select t.name,s.name
	from teacher t join t_s ts
	on t.id=ts.tid
	join student s
	on s.id=ts.sid;
2. 查询苍老师的学生都有谁
	select s.name
	from teacher t join t_s ts
	on t.id=ts.tid
	join student s
	on s.id=ts.sid where t.name='苍老师';
3. 查询小绿的老师都有谁
	select t.name
	from teacher t join t_s ts
	on t.id=ts.tid
	join student s
	on s.id=ts.sid where s.name='小绿';

#### 自关联
- 当前表的外键指向当前表的主键称为自关联
- 应用场景：需要保存如：上级领导、上级分类、上级部门
- 查询方式：把一张表当成两张表

#### 连接方式和关联关系
- 连接方式指内连接和外链接
- 关联关系是指两张表之间存在逻辑关系包括：一对一、一对多、多对多

#### 表设计案例：权限管理
- 需要创建三张主表：用户表user(id,name) 角色表role(id,name)  权限表module(id name)，还需要两张关系表：用户角色关系表 u_r(uid,rid)  、 角色权限关系表 r_m(rid,mid)
	create table user(id int primary key auto_increment,name varchar(10));
	create table role(id int primary key auto_increment,name varchar(10));
	create table module(id int primary key auto_increment,name varchar(10));
	create table u_r(uid int,rid int);
	create table r_m(rid int,mid int);
- 保存以下数据：
	用户表：刘德华、张学友、王菲
	角色表：男游客、男会员、女游客、女管理员
	权限表：男浏览、男发帖、男删帖、女浏览、女发帖、女删帖
	关系数据：刘德华(男会员，女游客) 张学友(男游客，女游客) 王菲(女管理员，男会员)
	男游客(男浏览) ，男会员（男浏览，男发帖），女游客（女浏览），女管理员（女浏览、女发帖、女删帖）
insert into user (name) values('刘德华'),('张学友'),('王菲');
insert into role (name) values('男游客'),('男会员'),('女游客'),('女管理员');
insert into module (name) values('男浏览'),('男发帖'),('男删帖'),('女浏览'),('女发帖'),('女删帖');
insert into u_r values(1,2),(1,3),(2,1),(2,3),(3,4),(3,2);
insert into r_m values(1,1),(2,1),(2,2),(3,4),(4,4),(4,5),(4,6);
1. 查询每个用户的名字和对应的权限名字
	select u.name,m.name
	from user u join u_r ur
	on u.id=ur.uid
	join r_m rm
	on ur.rid=rm.rid
	join module m
	on rm.mid=m.id;
2. 查询刘德华拥有的权限
	select m.name
	from user u join u_r ur
	on u.id=ur.uid
	join r_m rm
	on ur.rid=rm.rid
	join module m
	on rm.mid=m.id where u.name='刘德华';
3. 查询有男发帖权限的用户都有谁
	select u.name
	from user u join u_r ur
	on u.id=ur.uid
	join r_m rm
	on ur.rid=rm.rid
	join module m
	on rm.mid=m.id where m.name='男发帖';

### 面试题
	时间   金额  关系  性别  红包类型  姓名  
- 流水表trade:   id  time  money  type  pid
create table trade(id int primary key auto_increment,time date,money int,type varchar(5),pid int);
- 人员表person：  id  name gender  rel
create table person(id int primary key auto_increment,name varchar(10),gender varchar(5),rel varchar(5));
- 插入人员表数据：
insert into person values(null,'刘德华','男','亲戚'),(null,'杨幂','女','亲戚'),(null,'马云','男','同事'),(null,'特朗普','男','朋友'),(null,'貂蝉','女','朋友');

insert into trade values(null,'2018-03-20',1000,'现金',1),(null,'2018-04-14',500,'现金',2),(null,'2018-04-14',-50,'现金',2),(null,'2018-03-11',20000,'支付宝',3),(null,'2018-03-11',-5,'支付宝',1),(null,'2018-05-14',2000,'微信',4),(null,'2018-06-25',-20000,'微信',5);

3. 统计春节（2018年2月15号）到现在的红包收益
	select sum(money) from trade where time>str_to_date('2018年2月15号','%Y年%c月%d号');
4. 查询春节到现在 金额大于100所有女性亲戚的名字和金额
	select p.name,t.money
	from trade t join person p
	on t.pid=p.id
	where time>str_to_date('2018年2月15号','%Y年%c月%d号')
	and t.money not between -100 and 100
	and p.gender='女'
	and p.rel='亲戚';
5. 查询三个平台分别收入的红包金额
	select type,sum(money) from trade 
	where money>0 
	group by type;
### 视图
- 什么是视图：视图和表都是数据库中的对象，视图可以理解成是一张虚拟的表，视图本质就是取代了一段SQL查询语句。
- 为什么使用视图：使用视图可以起到SQL语句重用的作用，提高开发效率，还可以隐藏敏感信息

- 创建视图格式： 
	create view 视图名 as (子查询);
	1. 创建10号部门员工的视图
	create view v_emp_10 as (select * from emp where deptno=10);
	2. 创建没有工资的视图
	create view v_emp_nosal as (select ename,deptno,mgr from emp);
	3. 创建一个显示每个部门编号、平均工资、最高工资、最低工资、工资总和、员工人数的视图
	create view v_emp_info as (select deptno,avg(sal),max(sal),min(sal),sum(sal),count(*) from emp group by deptno);
- 视图的分类
	1. 简单视图：创建视图时的子查询不包含：去重、函数、分组、关联查询创建的视图称为简单视图，可以对简单视图进行增删改查操作
	2. 复杂视图：和简单视图相反，只能对复杂视图进行查询操作	
- 对简单视图进行增删改操作 操作方式和table一样
	1. 插入数据
	insert into v_emp_10 (empno,ename,deptno) values(10010,'Tom',10);//成功
	insert into v_emp_10 (empno,ename,deptno) values(10011,'Jerry',20); //成功 数据污染
	- 往视图中插入一条视图中不可见但是在原表中可见的数据称为数据污染，通过with check option 关键字避免出现数据污染现象
	create view v_emp_20 as(select * from emp where deptno=20) with check option;
	insert into v_emp_20 (empno,ename,deptno) values(10012,'Lucy',20);//成功
	insert into v_emp_20 (empno,ename,deptno) values(10013,'Lily',30); //失败 数据污染 插入不进去
	2. 修改和删除只能操作视图中存在的数据
	update v_emp_20 set sal=1000 where empno=10010;//修改失败
	delete from v_emp_20 where empno=10010;//删除失败
	delete from v_emp_10 where empno=10010;//删除成功
- 创建或修改视图
	create or replace view v_emp_20 as (select ename from emp where deptno=20);
- 删除视图
	drop view 视图名;
- 创建视图时如果子查询中使用了别名则之后对视图进行操作只能使用别名
	create view v_emp_30 as (select ename name,sal from emp where deptno=30);	
	select ename from v_emp_30; //报错 找不到ename

##### 外键约束(有外键约束时不能删除，要先删掉主键约束的才能删掉外键约束的)
- constraint 约束名称 foreign key(外键字段名) references 被依赖的表名（被依赖的字段名）
```
create table emp(id int primary key auto_increment,name varchar(10),deptid int);
create table dept(id int primary key auto_increment,name varchar(10),deptid int ,constraint fk_dept foreign key(deptid) references emp(id));
```

##### 创建索引
- create index 索引名 on 表名（字段名（？字段长度））;
- create index i_item_tile on item2(title);
- select * from item2 where title='100';
- 索引越多越好？
	不是，只针对常用的查询字段创建索引
1. 查看索引
 - show index from 表名;
2. 删除索引
 - drop index 索引名 on 表名;
3. 通过多个字段创建的索引称为复合索引
 - create index i_item_title_price on item2(title,price);

##### 事务
