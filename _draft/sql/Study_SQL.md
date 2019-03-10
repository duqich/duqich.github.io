 ### 1.2sql语句执行顺序

**一般sql的语法:select .. from .. where .. group by .. having .. union .. order by;**

 _但是执行顺序一般是_：

 - from ... 
 - where ...
 - group by ...
 - having ...
 - select ...
 - union ...
 - order by ...

## 1.1some pieces of produce
select top 50 percent * from emp;

select
in out inout
delimiter $$

set @p_out=1;

call out_param(@p_out);

select p_out;
set p_out=1;

declare l_varchar varchar(255) default 'this is american';

select 'hello world' into @x;
select @x;
set @y='goodbye';
select @y;

concat(@gg,'world');


if  then end if;
if then else end if

case var
when 0 then
when 1 then
else
end case;

while var<6 do
end while;

repeat
until v>5
end repeat;

loop
if then 
leave;
end if;
end loop;

iterate


join 
merge into
with as


- 游标的使用,以及repeat，while，loop的使用

```sql
DECLARE CONTINUE HANDLER FOR NOT FOUND  SET done = 1;
	
	OPEN c_mycursor;
		
		while done!=1 do
			FETCH NEXT FROM c_mycursor into v_code;
			set ooo = v_code;
		end while;
		
	CLOSE c_mycursor;
```

```sql
delimiter $$
create procedure testcursor(
	out ooo VARCHAR(10)
)
BEGIN
	DECLARE v_code VARCHAR(10);
    DECLARE done INT DEFAULT 0;
   
	DECLARE c_mycursor CURSOR for
	SELECT tobacode 
	from pubtoba;

DECLARE CONTINUE HANDLER FOR NOT FOUND  SET done = 1;
	
	
	OPEN c_mycursor;
		
		l1: loop
			IF done=1 then LEAVE l1; end if;
			FETCH c_mycursor into v_code;
			set ooo = v_code;
		end loop;
	
	CLOSE c_mycursor;
END $$
```

```sql
delimiter $$
create procedure testcursor(
	out ooo VARCHAR(10)
)
BEGIN
	DECLARE v_code VARCHAR(10);
    DECLARE done INT DEFAULT 0;
   
	DECLARE c_mycursor CURSOR for
	SELECT tobacode 
	from pubtoba;

DECLARE CONTINUE HANDLER FOR NOT FOUND  SET done = 1;
	
	
	OPEN c_mycursor;
		
		REPEAT
			FETCH c_mycursor into v_code;
			set ooo = v_code;
		until done=1 END REPEAT;
	
	CLOSE c_mycursor;
END $$
```


## SQL高级语法

select b.* from (select a.*,rownum rn from (select * from emp) a where rownum<=5) b where rn>=3;

### TOP

select top 2 * from emp;
select top 50 percent *from emp;

### 通配符

like '%ojbk%'
not like '%ojbk%'

select * from persons where city like '[ABC]ejing';
select * from persons where city like '[!DE]ejing';

_ejing

### IN

select * from peosons where lastname in ('jack','duke');

### BETWEEN AND
select * from persons where lastname between 'Adams' AND 'Carter';

between 1 and 5;

### JOIN

现有：用户表和订单表
![](https://ww1.sinaimg.cn/large/005YhI8igy1fwx738wyh6j31a00r041m)


- inner join

>找出有订单号的用户和订单号

```sql
select firstname,lastname from persons inner join orders on persons.p_id=orders.p_id;
```

- left join

>列出所有用户，以及他们的订单号

```sql
select firstname,lastname,orderno from persons left join orders on persons.p_id=orders.p_id;
```

- right join

>列出所有订单,以及购买的人

```sql
select orderno,firstname,lastname from persons right join orders on persons.p_id=orders.p_id;
```

- full join(**mysql不支持**)

>列出所有的人以及所有的订单

```sql
select firstname,lastname,orderno from persons full join orders on persons.p_id=orders.p_id;
```


### UNION 

现有表

![](https://ww1.sinaimg.cn/large/005YhI8igy1fwxa1x522vj31ac0q0go1)

>列出所有职员

```sql
select e_name from employees_china union all SELECT e_name from employees_usa;
```

### MERGE INTO table USING table ON (条件) WHEN MATCHED THEN WHEN NOT MATCHED THEN
 
 判断条件匹配是否匹配,做出对应操作

 常用与添加时，如果存在即更新，不存在则添加


 ### with as 公共表表达式   只能在接下里的一条语句多次使用


 ### NULLIF(A,0)  如果A=0返回null，否则返回A

 ### COALESCE(A,B,C,D) 返回第一个非null

 ### ROW_NUMBER() OVER(PARTITION BY A ORDER BY B) AS ROW_INDEX,*   对每一行添加一个序号，分组partition by，排序order by

 ### create trigger checkTrigger before insert on checkDemoTable for each row begin if new.a<=0 then set new.a=1; end if; end  因为mysql约束check无效用触发器

 ###




### 学生表练习

![](https://ww1.sinaimg.cn/large/007i4MEmgy1g0e3nxavnlj30n80bwjse.jpg)

![](https://ww1.sinaimg.cn/large/007i4MEmgy1g0e3qsmockj30lc0akq4r.jpg)

![](https://ww1.sinaimg.cn/large/007i4MEmgy1g0e3qsf6eej30fi04y74o.jpg)

![](https://ww1.sinaimg.cn/large/007i4MEmgy1g0e3qsjl5vj30b804wq36.jpg)

![](https://ww1.sinaimg.cn/large/007i4MEmgy1g0e3qsp3fsj30fa0kk40m.jpg)

#### 1.查询所有"01"课程比"02"课程成绩高的学生的信息及课程分数

```sql

select d.*,c.c1score,c2score 
from Student d right join (select a.Sid,a.score as 'c1score',b.score as 'c2score' 
							from (select * from SC where Cid='01') a  join (select * from SC where Cid='02') b on a.Sid=b.Sid where a.score>b.score
							) c 
							on c.Sid=d.Sid

 ```

![]()

#### 2.查询同时存在" 01 "课程和" 02 "课程的情况

```sql
select * from (select * from SC where Cid='01') a inner join (select * from SC where Cid='02') b on a.Sid=b.Sid
```

#### 3.查询存在" 01 "课程但可能不存在" 02 "课程的情况(不存在时显示为 null )

```sql
select a.Sid,a.score as score_1,case when b.score is null then 'null' else b.score end as score_2 from (select * from SC where Cid='01') a left join (select * from SC where Cid='02') b on a.Sid=b.Sid
```

#### 4.查询不存在" 01 "课程但存在" 02 "课程的情况

```sql
select * from (select * from SC where Cid='02') a inner join (select * from SC where Cid!='01') b on a.Sid=b.Sid
```

#### 5.查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩

```sql
select a.Sid,Student.Sname,a.avg_score from (select Sid,AVG(score) as avg_score from SC group by Sid having avg_score>=60) a left join Student on Student.Sid=a.Sid
```
#### 6.查询在 SC 表存在成绩的学生信息

```sql
select b.Sid,b.Sname,b.Sage,b.Ssex from (select * from SC) a left join (select * from Student) b on a.Sid=b.Sid group by b.Sid,b.Sname,b.Sage,b.Ssex
```

#### 7.查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩(没成绩的显示为 null )

```sql
select a.Sid,a.Sname,count(b.Cid) as '选课总数', case ISNULL(sum(b.score)) when '1' then 'null' else sum(b.score) end  as '总成绩' from (select * from Student) a left join (select * from SC ) b on a.Sid=b.Sid group by a.Sid,a.Sname
```


#### 8.查有成绩的学生信息

```sql
select b.Sid,b.Sname,b.Sage,b.Ssex from (select * from SC) a left join (select * from Student) b on a.Sid=b.Sid group by b.Sid,b.Sname,b.Sage,b.Ssex
```

#### 9.查询「李」姓老师的数量

```sql
select count(*) from Teacher where Tname like '李%' 
```

#### 10.查询学过「张三」老师授课的同学的信息

```sql
select * from Student right join (select * from SC where Cid in (select a.Cid from (select * from Course) a join (select * from Teacher where Tname='张三') b on b.Tid=a.Tid)) c on c.Sid=Student.Sid 
```

#### 11.查询没有学全所有课程的同学的信息 

```sql
select * from Student left join (select Sid,count(Cid) as learned from SC group by Sid) b on Student.Sid=b.Sid where b.learned<3 or ISNULL(b.learned)
```

#### 12.查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息

```sql
select * from Student right join (select Sid from SC where Cid in (select cid from SC where Sid='01') and Sid!='01' group by Sid) a on Student.Sid=a.Sid
```

#### 13.查询和" 01 "号的同学学习的课程完全相同的其他同学的信息

#### 14.查询没学过"张三"老师讲授的任一门课程的学生姓名

```sql
SELECT Sname from Student where sid not in (SELECT Student.Sid from Student join (SELECT Sid from SC join (SELECT Course.Cid from Course join Teacher on Teacher.Tid=Course.Tid where Teacher.Tname = '张三') a on a.cid = SC.cid GROUP by SC.Sid) b on b.Sid=Student.Sid)  
```


#### 15.查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩

```sql
SELECT b.Sid,Sname,b.avg from Student right join (SELECT Sid,avg(SC.score) as avg from SC  where SC.Sid in (SELECT Sid from (SELECT Sid,COUNT(Cid) as count from SC where score<60 GROUP by Sid) a where a.count >=2) group by Sid) b on b.Sid=Student.Sid
```

#### 16.检索" 01 "课程分数小于 60，按分数降序排列的学生信息

```sql
SELECT Student.*,a.score from Student RIGHT join (SELECT * from SC where Cid='01' and score<60) a on a.Sid=Student.Sid order by a.score desc
```

#### 17.按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩

```sql 
SELECT b.*,c.avgscore from (SELECT a.Sid,sum(a.01score) as '01score',sum(a.02score) as '02score',SUM(a.03score) as '03score' from (SELECT Sid,case Cid when '01' then score else 0 end as '01score',case Cid when '02' then score else 0 end as '02score',case Cid when '03' then score else 0 end as '03score' from SC) a group by Sid) b                                          
JOIN (SELECT Sid,avg(SC.score) as avgscore from SC GROUP by Sid) c on c.Sid=b.Sid order by c.avgscore desc
```




db2返回多结果集的存储过程格式如下：


```sql

delimiter $$;

CREATE PROCEDURE prc_
 (
  IN p_enddate 		DATE,
	OUT o_print VARCHAR(20)         
 ) 

  DYNAMIC RESULT SETS 2
  LANGUAGE SQL

  begin
  DECLARE v_errCode   INT;
  DECLARE v_errState  CHAR(5);
  DECLARE v_strSQL  VARCHAR(800);
  DECLARE v_CurrentMonthFirstDay  DATE;
  DECLARE v_CurrentYearFirstDay  DATE;
 
    --------------------------------------------------------
    -- DECLARE ERROR HANDLERS
    --------------------------------------------------------

--     DECLARE CONTINUE HANDLER FOR SQLEXCEPTION
--     BEGIN
--         VALUES( SQLCODE, SQLSTATE) INTO v_errCode, v_errState;
--     END;
-- 
    --------------------------------------------------------
    -- DECLARE GLOBAL TEMPORARY TABLES
    --------------------------------------------------------
 	DECLARE GLOBAL TEMPORARY TABLE captions
    (
	    caption     VARCHAR(500),
	    width   	INT,
	    property  	VARCHAR(40),
	    format    	VARCHAR(20),
	    fixcol    	INT,
	    align   	VARCHAR(20),
	    datatype  	INT,
        code        INT,
        ordertag    varchar(20)
    ) ON COMMIT PRESERVE ROWS WITH REPLACE NOT LOGGED;
    
     DECLARE GLOBAL TEMPORARY TABLE middle(
    	tobacode     varchar(50),
    	tobaname     varchar(50),
    	brandcode	 varchar(20),
    	brandname	 varchar(20),
    	pricetype    varchar(5),
    	pricesubtype    varchar(5),
    	compcode	 varchar(50),
    	compname	 varchar(50),
    	provcode 	 varchar(50),
    	provname	 varchar(50),
    	areaname	 varchar(50),
    	areacode	 varchar(50),
    	reportdate	 varchar(50),
    	outsell		 decimal(19,4),  --日销售量
    	outmoney	 decimal(19,4)  --日销售额
   ) ON COMMIT PRESERVE ROWS WITH REPLACE NOT LOGGED;
   
   
     DECLARE GLOBAL TEMPORARY TABLE results
  (
		areacode					varchar(50),
		areaname         			varchar(50),
		tobacode					varchar(50),
		tobaname					varchar(50),
		brandcode					varchar(50),
		pricetype					varchar(5),
		pricesubtype				varchar(5),
		
		sale_day					decimal(19,4),		--日销售量
		sale_month					decimal(19,4),		--销售量 月累计
		sale_year					decimal(19,4),		--销售量 年累计
		sale_lastyear				decimal(19,4),		--销售量 去年同期
		sale_YoY					decimal(19,4),		--销售量 同比
		sale_YoY1					decimal(19,4),		--销售量 同比增幅
		money_day					decimal(19,4),		--日销售额
		money_month					decimal(19,4),		--销售额 月累计
		money_year					decimal(19,4),		--销售额 年累计
		money_lastyear				decimal(19,4),		--销售额 去年同期
		price_YoY					decimal(19,4),		--销售额	同比
		price_YoY1					decimal(19,4),		--销售额	同比增幅
		
		flag               			int,		--区分四川和各个省、市、区
		ordertag					varchar(10),		--区分烟类别
		marker						int					--标记
  ) ON COMMIT PRESERVE ROWS WITH REPLACE NOT LOGGED;
  

   
    --------------------------------------------------------
    -- 组织返回结果集
    --------------------------------------------------------



    BEGIN
	    
		DECLARE v_captionCur CURSOR WITH RETURN TO CALLER FOR
			select * from SESSION.captions;
      	DECLARE v_resultCur CURSOR WITH RETURN TO CALLER FOR
	      	SELECT *
	      	from session.results;

    OPEN v_captionCur;
    OPEN v_resultCur;

    END;

END $$

    
```