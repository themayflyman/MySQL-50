# 	MySOL 50题

## 练习数据

–1.学生表

Student(s_id,s_name,s_birth,s_sex) –学生编号,学生姓名, 出生年月,学生性别

–2.课程表

Course(c_id,c_name,t_id) – –课程编号, 课程名称, 教师编号

–3.教师表

Teacher(t_id,t_name) –教师编号,教师姓名

–4.成绩表

Score(s_id,c_id,s_score) –学生编号,课程编号,分数

建表

```sql
--学生表
CREATE TABLE Student(
    s_id VARCHAR(20),
    s_name VARCHAR(20) NOT NULL DEFAULT '',
    s_birth DATE,
    s_sex VARCHAR(10) NOT NULL DEFAULT '',
    PRIMARY KEY(s_id)
);
--课程表
CREATE TABLE Course(
    c_id  VARCHAR(20),
    c_name VARCHAR(20) NOT NULL DEFAULT '',
    t_id VARCHAR(20) NOT NULL,
    PRIMARY KEY(c_id)
);
--教师表
CREATE TABLE Teacher(
    t_id VARCHAR(20),
    t_name VARCHAR(20) NOT NULL DEFAULT '',
    PRIMARY KEY(t_id)
);
--成绩表
CREATE TABLE Score(
    s_id VARCHAR(20),
    c_id  VARCHAR(20),
    s_score INT(3),
    PRIMARY KEY(s_id,c_id)
);
```

创建测试数据

学生表 Student

```sql
insert into Student values('01' , '赵雷' , '1990-01-01' , '男');
insert into Student values('02' , '钱电' , '1990-12-21' , '男');
insert into Student values('03' , '孙风' , '1990-05-20' , '男');
insert into Student values('04' , '李云' , '1990-08-06' , '男');
insert into Student values('05' , '周梅' , '1991-12-01' , '女');
insert into Student values('06' , '吴兰' , '1992-03-01' , '女');
insert into Student values('07' , '郑竹' , '1989-07-01' , '女');
insert into Student values('08' , '王菊' , '1990-01-20' , '女');

+------+--------+------------+-------+
| s_id | s_name | s_birth    | s_sex |
+------+--------+------------+-------+
| 01   | 赵雷   | 1990-01-01 | 男    |
| 02   | 钱电   | 1990-12-21 | 男    |
| 03   | 孙风   | 1990-05-20 | 男    |
| 04   | 李云   | 1990-08-06 | 男    |
| 05   | 周梅   | 1991-12-01 | 女    |
| 06   | 吴兰   | 1992-03-01 | 女    |
| 07   | 郑竹   | 1989-07-01 | 女    |
| 08   | 王菊   | 1990-01-20 | 女    |
+------+--------+------------+-------+
```

科目表 Course

```sql
insert into Course values('01' , '语文' , '02');
insert into Course values('02' , '数学' , '01');
insert into Course values('03' , '英语' , '03');

+------+--------+------+
| c_id | c_name | t_id |
+------+--------+------+
| 01   | 语文   | 02   |
| 02   | 数学   | 01   |
| 03   | 英语   | 03   |
+------+--------+------+
```

教师表 Teacher

```sql
create table Teacher(TId varchar(10),Tname varchar(10));
insert into Teacher values('01' , '张三');
insert into Teacher values('02' , '李四');
insert into Teacher values('03' , '王五');

+------+--------+
| t_id | t_name |
+------+--------+
| 01   | 张三   |
| 02   | 李四   |
| 03   | 王五   |
+------+--------+
```

成绩表 Score

```sql
insert into SC values('01' , '01' , 80);
insert into SC values('01' , '02' , 90);
insert into SC values('01' , '03' , 99);
insert into SC values('02' , '01' , 70);
insert into SC values('02' , '02' , 60);
insert into SC values('02' , '03' , 80);
insert into SC values('03' , '01' , 80);
insert into SC values('03' , '02' , 80);
insert into SC values('03' , '03' , 80);
insert into SC values('04' , '01' , 50);
insert into SC values('04' , '02' , 30);
insert into SC values('04' , '03' , 20);
insert into SC values('05' , '01' , 76);
insert into SC values('05' , '02' , 87);
insert into SC values('06' , '01' , 31);
insert into SC values('06' , '03' , 34);
insert into SC values('07' , '02' , 89);
insert into SC values('07' , '03' , 98);

+------+------+---------+
| s_id | c_id | s_score |
+------+------+---------+
| 01   | 01   |      80 |
| 01   | 02   |      90 |
| 01   | 03   |      99 |
| 02   | 01   |      70 |
| 02   | 02   |      60 |
| 02   | 03   |      80 |
| 03   | 01   |      80 |
| 03   | 02   |      80 |
| 03   | 03   |      80 |
| 04   | 01   |      50 |
| 04   | 02   |      30 |
| 04   | 03   |      20 |
| 05   | 01   |      76 |
| 05   | 02   |      87 |
| 06   | 01   |      31 |
| 06   | 03   |      34 |
| 07   | 02   |      89 |
| 07   | 03   |      98 |
+------+------+---------+
```

## 练习题目

1. 查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数 

   ```sql
   SELECT t3.SId,Student.*,t3.score1,t3.score2
   FROM (SELECT t1.SId,t1.score AS score1,t2.score AS score2
         FROM (SELECT SId,score FROM SC WHERE SC.CId='01') AS t1,
              (SELECT SId,score FROM SC WHERE SC.CId='02') AS t2
         WHERE t1.SId=t2.SId AND t1.score>t2.score) AS t3
   INNER JOIN Student
   ON t3.SId=Student.SId;
   
   +------+------+--------+---------------------+------+--------+--------+
   | SId  | SId  | Sname  | Sage                | Ssex | score1 | score2 |
   +------+------+--------+---------------------+------+--------+--------+
   | 02   | 02   | 钱电   | 1990-12-21 00:00:00 | 男   |   70.0 |   60.0 |
   | 04   | 04   | 李云   | 1990-08-06 00:00:00 | 男   |   50.0 |   30.0 |
   +------+------+--------+---------------------+------+--------+--------+
   ```



   1.1 查询同时存在" 01 "课程和" 02 "课程的情况 

   ``````sql
   SELECT t3.SId,Student.*,t3.score1,t3.score2
   FROM (SELECT t1.SId,t1.score AS score1,t2.score AS score2
         FROM (SELECT SId,score FROM SC WHERE SC.CId='01') AS t1,
              (SELECT SId,score FROM SC WHERE SC.CId='02') AS t2
         WHERE t1.SId=t2.SId) AS t3
   INNER JOIN Student
   ON t3.SId=Student.SId;
   
   +------+------+--------+---------------------+------+--------+--------+
   | SId  | SId  | Sname  | Sage                | Ssex | score1 | score2 |
   +------+------+--------+---------------------+------+--------+--------+
   | 01   | 01   | 赵雷   | 1990-01-01 00:00:00 | 男   |   80.0 |   90.0 |
   | 02   | 02   | 钱电   | 1990-12-21 00:00:00 | 男   |   70.0 |   60.0 |
   | 03   | 03   | 孙风   | 1990-05-20 00:00:00 | 男   |   80.0 |   80.0 |
   | 04   | 04   | 李云   | 1990-08-06 00:00:00 | 男   |   50.0 |   30.0 |
   | 05   | 05   | 周梅   | 1991-12-01 00:00:00 | 女   |   76.0 |   87.0 |
   +------+------+--------+---------------------+------+--------+--------+
   ``````

   1.2 查询存在" 01 "课程但可能不存在" 02 "课程的情况(不存在时显示为 null ) 

   ```sql
   SELECT t3.SId,Student.*,t3.score1,t3.score2
   FROM (SELECT t1.SId,t1.score AS score1,t2.score AS score2
         FROM (SELECT SId,score FROM SC WHERE SC.CId='01') AS t1 
               LEFT JOIN (SELECT SId,score FROM SC WHERE SC.CId='02') AS t2
               ON t1.SId=t2.SId) AS t3
   INNER JOIN Student
   ON t3.SId=Student.SId;
   
   +------+------+--------+---------------------+------+--------+--------+
   | SId  | SId  | Sname  | Sage                | Ssex | score1 | score2 |
   +------+------+--------+---------------------+------+--------+--------+
   | 01   | 01   | 赵雷   | 1990-01-01 00:00:00 | 男   |   80.0 |   90.0 |
   | 02   | 02   | 钱电   | 1990-12-21 00:00:00 | 男   |   70.0 |   60.0 |
   | 03   | 03   | 孙风   | 1990-05-20 00:00:00 | 男   |   80.0 |   80.0 |
   | 04   | 04   | 李云   | 1990-08-06 00:00:00 | 男   |   50.0 |   30.0 |
   | 05   | 05   | 周梅   | 1991-12-01 00:00:00 | 女   |   76.0 |   87.0 |
   | 06   | 06   | 吴兰   | 1992-03-01 00:00:00 | 女   |   31.0 |   NULL |
   +------+------+--------+---------------------+------+--------+--------+
   ```



   1.3 查询不存在" 01 "课程但存在" 02 "课程的情况

   ```sql
   SELECT Student.*, SC.score
   FROM SC
   INNER JOIN Student
   ON Student.SId=SC.SId
   WHERE SC.SId NOT IN (SELECT SId FROM SC WHERE SC.CId='01')
   AND SC.CId='02';
   
   +------+--------+---------------------+------+-------+
   | SId  | Sname  | Sage                | Ssex | score |
   +------+--------+---------------------+------+-------+
   | 07   | 郑竹   | 1989-07-01 00:00:00 | 女   |  89.0 |
   +------+--------+---------------------+------+-------+
   ```

2. 查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩

   ```sql
   SELECT Student.Sname, t1.AvgScore
   FROM (SELECT SId, AVG(score) AS AvgScore
         FROM SC
         GROUP BY SId) AS t1
   INNER JOIN Student
   ON Student.SId=t1.SId
   WHERE t1.AvgScore>60;
   
   +--------+----------+
   | Sname  | score    |
   +--------+----------+
   | 赵雷   | 89.66667 |
   | 钱电   | 70.00000 |
   | 孙风   | 80.00000 |
   | 周梅   | 81.50000 |
   | 郑竹   | 93.50000 |
   +--------+----------+
   ```

3. 查询在 SC 表存在成绩的学生信息

   ```sql
   SELECT DISTINCT Student.*
   FROM Student
   INNER JOIN SC
   WHERE Student.SId=SC.SId;
   
   +------+--------+---------------------+------+
   | SId  | Sname  | Sage                | Ssex |
   +------+--------+---------------------+------+
   | 01   | 赵雷   | 1990-01-01 00:00:00 | 男   |
   | 02   | 钱电   | 1990-12-21 00:00:00 | 男   |
   | 03   | 孙风   | 1990-05-20 00:00:00 | 男   |
   | 04   | 李云   | 1990-08-06 00:00:00 | 男   |
   | 05   | 周梅   | 1991-12-01 00:00:00 | 女   |
   | 06   | 吴兰   | 1992-03-01 00:00:00 | 女   |
   | 07   | 郑竹   | 1989-07-01 00:00:00 | 女   |
   +------+--------+---------------------+------+
   ```

4. 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩(没成绩的显示为 null ) 

   ```sql
   SELECT Student.*, t1.courseNum, t1.scoreSum
   FROM (SELECT SId, COUNT(CId) AS courseNum, SUM(score) AS scoreSum
         FROM SC
         GROUP BY SId) as t1
   INNER JOIN Student
   ON Student.SId=t1.SId;
   
   +------+--------+---------------------+------+-----------+----------+
   | SId  | Sname  | Sage                | Ssex | courseNum | scoreSum |
   +------+--------+---------------------+------+-----------+----------+
   | 01   | 赵雷   | 1990-01-01 00:00:00 | 男   |         3 |    269.0 |
   | 02   | 钱电   | 1990-12-21 00:00:00 | 男   |         3 |    210.0 |
   | 03   | 孙风   | 1990-05-20 00:00:00 | 男   |         3 |    240.0 |
   | 04   | 李云   | 1990-08-06 00:00:00 | 男   |         3 |    100.0 |
   | 05   | 周梅   | 1991-12-01 00:00:00 | 女   |         2 |    163.0 |
   | 06   | 吴兰   | 1992-03-01 00:00:00 | 女   |         2 |     65.0 |
   | 07   | 郑竹   | 1989-07-01 00:00:00 | 女   |         2 |    187.0 |
   +------+--------+---------------------+------+-----------+----------+
   ```

   4.1 查有成绩的学生信息(查询逻辑与3不同)

   ```sql
   SELECT *
   FROM Student
   WHERE EXISTS(SELECT * FROM SC WHERE STUDENT.SId=SC.SId);
   
   +------+--------+---------------------+------+
   | SId  | Sname  | Sage                | Ssex |
   +------+--------+---------------------+------+
   | 01   | 赵雷   | 1990-01-01 00:00:00 | 男   |
   | 02   | 钱电   | 1990-12-21 00:00:00 | 男   |
   | 03   | 孙风   | 1990-05-20 00:00:00 | 男   |
   | 04   | 李云   | 1990-08-06 00:00:00 | 男   |
   | 05   | 周梅   | 1991-12-01 00:00:00 | 女   |
   | 06   | 吴兰   | 1992-03-01 00:00:00 | 女   |
   | 07   | 郑竹   | 1989-07-01 00:00:00 | 女   |
   +------+--------+---------------------+------+
   ```

5. 查询「李」姓老师的数量

   ```sql
   SELECT COUNT(*) AS num
   FROM Teacher
   WHERE Tname LIKE '李%';
   
   +-----+
   | num |
   +-----+
   |   1 |
   +-----+
   ```

6. 查询学过「张三」老师授课的同学的信息

   ```sql
   SELECT Student.*
   FROM (SELECT SC.SId
         FROM SC
         WHERE SC.CId IN (SELECT CId
                          FROM Course
                          INNER JOIN Teacher
                          ON Course.TId=Teacher.TId
                          WHERE Teacher.Tname='张三')) AS t1
   INNER JOIN Student
   ON Student.SId=t1.SId;
   
   +------+--------+---------------------+------+
   | SId  | Sname  | Sage                | Ssex |
   +------+--------+---------------------+------+
   | 01   | 赵雷   | 1990-01-01 00:00:00 | 男   |
   | 02   | 钱电   | 1990-12-21 00:00:00 | 男   |
   | 03   | 孙风   | 1990-05-20 00:00:00 | 男   |
   | 04   | 李云   | 1990-08-06 00:00:00 | 男   |
   | 05   | 周梅   | 1991-12-01 00:00:00 | 女   |
   | 07   | 郑竹   | 1989-07-01 00:00:00 | 女   |
   +------+--------+---------------------+------+
   ```

7. 查询没有学全所有课程的同学的信息

   ```sql
   SELECT Student.*
   FROM Student
   INNER JOIN (SELECT SId
               FROM SC
               GROUP BY SId
               HAVING COUNT(*)<(SELECT COUNT(*) FROM Course)) AS t1
   ON Student.SId=t1.SId;
   
   +------+--------+---------------------+------+
   | SId  | Sname  | Sage                | Ssex |
   +------+--------+---------------------+------+
   | 05   | 周梅   | 1991-12-01 00:00:00 | 女   |
   | 06   | 吴兰   | 1992-03-01 00:00:00 | 女   |
   | 07   | 郑竹   | 1989-07-01 00:00:00 | 女   |
   +------+--------+---------------------+------+
   ```

8. 查询至少有一门课与学号为" 01 "的同学所学相同的同学的信

   ```sql
   SELECT DISTINCT Student.*
   FROM Student
   INNER JOIN SC
   ON Student.SId=SC.SId
   WHERE SC.CId IN (SELECT CId FROM SC WHERE SC.SId='01') 
   AND Student.SId!='01';
   
   +------+--------+---------------------+------+
   | SId  | Sname  | Sage                | Ssex |
   +------+--------+---------------------+------+
   | 02   | 钱电   | 1990-12-21 00:00:00 | 男   |
   | 03   | 孙风   | 1990-05-20 00:00:00 | 男   |
   | 04   | 李云   | 1990-08-06 00:00:00 | 男   |
   | 05   | 周梅   | 1991-12-01 00:00:00 | 女   |
   | 06   | 吴兰   | 1992-03-01 00:00:00 | 女   |
   | 07   | 郑竹   | 1989-07-01 00:00:00 | 女   |
   +------+--------+---------------------+------+
   ```

9. 查询和" 01 "号的同学学习的课程 完全相同的其他同学的信息

   ```sql
   SELECT Student.*
   FROM Student
   WHERE s_id IN (SELECT s_id
                  FROM Score
                  GROUP BY s_id
                  HAVING COUNT(s_id)=(SELECT COUNT(c_id)
                                      FROM Score
                                      WHERE s_id='01'))
   AND s_id IN (SELECT s_id
                FROM Score
                WHERE c_id NOT IN (SELECT c_id
                                   FROM Score
                                   WHERE c_id NOT IN (SELECT c_id
                                                      FROM Score
                                                      WHERE s_id='01'))
               GROUP BY s_id)
   AND s_id!='01';
   
   +------+--------+------------+-------+
   | s_id | s_name | s_birth    | s_sex |
   +------+--------+------------+-------+
   | 02   | 钱电   | 1990-12-21 | 男    |
   | 03   | 孙风   | 1990-05-20 | 男    |
   | 04   | 李云   | 1990-08-06 | 男    |
   +------+--------+------------+-------+
   ```

10. 查询没学过"张三"老师讲授的任一门课程的学生姓名

    ```sql
    SELECT s_name
    FROM Student
    WHERE s_id NOT IN (SELECT s_id
                       FROM Score
                       WHERE c_id IN (SELECT c_id
                                      FROM Course
                                      INNER JOIN Teacher
                                      ON Course.t_id=Teacher.t_id
                                      WHERE Teacher.t_name='张三'));
                                      
    +--------+
    | s_name |
    +--------+
    | 吴兰   |
    | 王菊   |
    +--------+
    ```

11. 查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩

    ```sql
    SELECT Student.s_id,Student.s_name,AVG(Score.s_score)
    FROM Student
    LEFT JOIN Score
    ON Student.s_id=Score.s_id
    WHERE Score.s_score<60
    GROUP BY Score.s_id
    HAVING COUNT(Score.s_score)>=2;
    
    +------+--------+--------------------+
    | s_id | s_name | avg(Score.s_score) |
    +------+--------+--------------------+
    | 04   | 李云   |            33.3333 |
    | 06   | 吴兰   |            32.5000 |
    +------+--------+--------------------+
    ```

12. 检索" 01 "课程分数小于 60，按分数降序排列的学生信息

    ```sql
    SELECT Student.*,Score.s_score
    FROM Student
    INNER JOIN Score
    ON Student.s_id=Score.s_id
    WHERE Score.s_score<60 AND Score.c_id='01'
    ORDER BY Score.s_score DESC;
    
    +------+--------+------------+-------+---------+
    | s_id | s_name | s_birth    | s_sex | s_score |
    +------+--------+------------+-------+---------+
    | 04   | 李云   | 1990-08-06 | 男    |      50 |
    | 06   | 吴兰   | 1992-03-01 | 女    |      31 |
    +------+--------+------------+-------+---------+
    ```

13. 按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩

    ```sql
    SELECT Student.s_id, 
           t1.s_score AS '01_score',
           t2.s_score AS '02_score',
           t3.s_score AS '03_score',
    	   AVG(t4.s_score)
    FROM Student
    LEFT JOIN Score AS t1 ON Student.s_id = t1.s_id
    AND t1.c_id = '01'
    LEFT JOIN Score AS t2 ON Student.s_id = t2.s_id
    AND t2.c_id = '02'
    LEFT JOIN Score AS t3 ON Student.s_id = t3.s_id
    AND t3.c_id = '03'
    LEFT JOIN Score AS t4 ON Student.s_id = t4.s_id
    GROUP BY Student.s_id
    ORDER BY AVG(t4.s_score) DESC;
    
    +------+----------+----------+----------+-----------------+
    | s_id | 01_score | 02_score | 03_score | AVG(t4.s_score) |
    +------+----------+----------+----------+-----------------+
    | 07   |     NULL |       89 |       98 |         93.5000 |
    | 01   |       80 |       90 |       99 |         89.6667 |
    | 05   |       76 |       87 |     NULL |         81.5000 |
    | 03   |       80 |       80 |       80 |         80.0000 |
    | 02   |       70 |       60 |       80 |         70.0000 |
    | 04   |       50 |       30 |       20 |         33.3333 |
    | 06   |       31 |     NULL |       34 |         32.5000 |
    | 08   |     NULL |     NULL |     NULL |            NULL |
    +------+----------+----------+----------+-----------------+
    ```

14. 查询各科成绩最高分、最低分和平均分： 以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率 及格为>=60，中等为：70-80，优良为：80-90，优秀为：>=90 要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列

    ```sql
    SELECT Course.c_id AS 课程ID,
           Course.c_name AS 课程名,
           MAX(Score.s_score) AS 最高分,
           MIN(Score.s_score) AS 最低分,
           AVG(Score.s_score) AS 平均分,
           (SUM(CASE
                WHEN Score.s_score>=60
                THEN 1
                ELSE 0
                END))/COUNT(Score.s_score) AS 及格率,
           (SUM(CASE
                WHEN Score.s_score>=70 AND Score.s_score<80
                THEN 1
                ELSE 0
                END))/COUNT(Score.s_score) AS 中等率,
           (SUM(CASE
                WHEN Score.s_score>=80 AND Score.s_score<90
                THEN 1
                ELSE 0
                END))/COUNT(Score.s_score) AS 优良率,
           (SUM(CASE
                WHEN Score.s_score>=90
                THEN 1
                ELSE 0
                END))/COUNT(Score.s_score) AS 优秀率,
           COUNT(Score.s_id) AS 选修人数m
    FROM Course
    LEFT JOIN Score
    ON Course.c_id=Score.c_id
    GROUP BY Course.c_id
    ORDER BY Course.c_id ASC,选修人数 DESC;
    ```

15. 按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺 

    ```sql
    -- RANK() OVER() on MySQL8.0+
    SELECT Score.c_id,Score.s_score,@currank:=@currank+1 AS rank
    FROM Score
    INNER JOIN (SELECT @currank:=0) AS t
    ORDER BY Score.s_score DESC;
    
    +------+---------+------+
    | c_id | s_score | rank |
    +------+---------+------+
    | 03   |      99 |    1 |
    | 03   |      98 |    2 |
    | 02   |      90 |    3 |
    | 02   |      89 |    4 |
    | 02   |      87 |    5 |
    | 03   |      80 |    6 |
    | 01   |      80 |    7 |
    | 02   |      80 |    8 |
    | 03   |      80 |    9 |
    | 01   |      80 |   10 |
    | 01   |      76 |   11 |
    | 01   |      70 |   12 |
    | 02   |      60 |   13 |
    | 01   |      50 |   14 |
    | 03   |      34 |   15 |
    | 01   |      31 |   16 |
    | 02   |      30 |   17 |
    | 03   |      20 |   18 |
    +------+---------+------+
    ```

    15.1 按各科成绩进行排序，并显示排名， Score 重复时合并名次

    ```sql
    -- DENSE RANK() OVER() on MySQL 8.0+
    SELECT Score.c_id,
           Score.s_score,
           (CASE
            WHEN @prevscore=Score.s_score
            THEN @currank
            WHEN @prevscore:=Score.s_score
            THEN @currank:=@currank+1
            END) AS rank
    FROM Score
    INNER JOIN (SELECT @currank:=0,@prevscore:=NULL) AS t
    ORDER BY Score.s_score DESC;
    
    +------+---------+------+
    | c_id | s_score | rank |
    +------+---------+------+
    | 03   |      99 |    1 |
    | 03   |      98 |    2 |
    | 02   |      90 |    3 |
    | 02   |      89 |    4 |
    | 02   |      87 |    5 |
    | 03   |      80 |    6 |
    | 01   |      80 |    6 |
    | 02   |      80 |    6 |
    | 03   |      80 |    6 |
    | 01   |      80 |    6 |
    | 01   |      76 |    7 |
    | 01   |      70 |    8 |
    | 02   |      60 |    9 |
    | 01   |      50 |   10 |
    | 03   |      34 |   11 |
    | 01   |      31 |   12 |
    | 02   |      30 |   13 |
    | 03   |      20 |   14 |
    +------+---------+------+
    ```

16. 查询学生的总成绩，并进行排名，总分重复时保留名次空缺 

    ```sql
    -- FIXME: unable to select students without score
    SELECT Score.s_id,SUM(Score.s_score) AS score_sum,@currank:=@currank+1 AS rank
    FROM Score
    INNER JOIN (SELECT @currank:=0) AS t
    GROUP BY Student.s_id
    ORDER BY score_sum DESC;
    
    +------+-----------+------+
    | s_id | score_sum | rank |
    +------+-----------+------+
    | 01   |       269 |    1 |
    | 03   |       240 |    3 |
    | 02   |       210 |    2 |
    | 07   |       187 |    7 |
    | 05   |       163 |    5 |
    | 04   |       100 |    4 |
    | 06   |        65 |    6 |
    +------+-----------+------+
    ```

    16.1 查询学生的总成绩，并进行排名，总分重复时不保留名次空缺

    ```sql
    SELECT t1.*,
           (CASE
            WHEN @prevscore=t1.score_sum
            THEN @currank
            WHEN @prevscore:=t1.score_sum
            THEN @currank:=@currank+1
            END) AS rank
    FROM (SELECT Score.s_id,SUM(Score.s_score) AS score_sum
          FROM Score
          INNER JOIN (SELECT @currank:=0,@prevscore:=NULL) AS t
          GROUP BY Score.s_id
          ORDER BY SUM(Score.s_score) DESC) AS t1;
    
    +------+-----------+------+
    | s_id | score_sum | rank |
    +------+-----------+------+
    | 01   |       269 |    1 |
    | 03   |       240 |    2 |
    | 02   |       210 |    3 |
    | 07   |       187 |    4 |
    | 05   |       163 |    5 |
    | 04   |       100 |    6 |
    | 06   |        65 |    7 |
    +------+-----------+------+
    ```

17. 统计各科成绩各分数段人数：课程编号，课程名称，[100-85]，[85-70]，[70-60]，[60-0] 及所占百分比

    ```sql
    SELECT Course.c_id,
           Course.c_name,  
           t1.*
    FROM Course
    LEFT JOIN (SELECT c_id,
                      CONCAT(SUM(CASE
                                 WHEN s_score>=85 AND s_score<=100
                                 THEN 1
                                 ELSE 0
                                 END)/COUNT(s_score)*100,'%') AS '[100-85]',
                      CONCAT(SUM(CASE
                                 WHEN s_score>=70 AND s_score<85
                                 THEN 1
                                 ELSE 0
                                 END)/COUNT(s_score)*100,'%') AS '(85-70]',
                      CONCAT(SUM(CASE
                                 WHEN s_score>=60 AND s_score<70
                                 THEN 1
                                 ELSE 0
                                 END)/COUNT(s_score)*100,'%') AS '(70-60]',
                      CONCAT(SUM(CASE
                                 WHEN s_score<=60
                                 THEN 1
                                 ELSE 0
                                 END)/COUNT(s_score)*100,'%') AS '[60-0]'
               FROM Score
               GROUP BY c_id) AS t1
    ON Course.c_id=t1.c_id;
    
    +------+--------+------+----------+----------+----------+----------+
    | c_id | c_name | c_id | [100-85] | (85-70]  | (70-60]  | [60-0]   |
    +------+--------+------+----------+----------+----------+----------+
    | 01   | 语文   | 01   | 0.0000%  | 66.6667% | 0.0000%  | 33.3333% |
    | 02   | 数学   | 02   | 50.0000% | 16.6667% | 16.6667% | 33.3333% |
    | 03   | 英语   | 03   | 33.3333% | 33.3333% | 0.0000%  | 33.3333% |
    +------+--------+------+----------+----------+----------+----------+
    ```

18. 查询各科成绩前三名的记录

    ```sql
    SELECT *
    FROM Score
    WHERE (SELECT COUNT(*)
           FROM Score as Score1
           WHERE Score.c_id=Score1.c_id AND Score.s_score<Score1.s_score)<3
    ORDER BY c_id,s_score DESC;
    
    +------+------+---------+
    | s_id | c_id | s_score |
    +------+------+---------+
    | 01   | 01   |      80 |
    | 03   | 01   |      80 |
    | 05   | 01   |      76 |
    | 01   | 02   |      90 |
    | 07   | 02   |      89 |
    | 05   | 02   |      87 |
    | 01   | 03   |      99 |
    | 07   | 03   |      98 |
    | 02   | 03   |      80 |
    | 03   | 03   |      80 |
    +------+------+---------+
    ```

19. 查询每门课程被选修的学生数

    ```sql
    SELECT COUNT(*) AS 选修人数 
    FROM Score 
    GROUP BY c_id;
    
    +--------------+
    | 选修人数     |
    +--------------+
    |            6 |
    |            6 |
    |            6 |
    +--------------+
    ```

20. 查询出只选修两门课程的学生学号和姓名

    ```sql
    SELECT s_id,s_name
    FROM Student
    WHERE s_id IN (SELECT s_id
                   FROM Score
                   GROUP BY s_id
                   HAVING COUNT(*)=2);
     
    +------+--------+
    | s_id | s_name |
    +------+--------+
    | 05   | 周梅   |
    | 06   | 吴兰   |
    | 07   | 郑竹   |
    +------+--------+
    ```

21. 查询男生、女生人数

    ```sql
    SELECT s_sex, COUNT(*) AS number
    FROM Student
    GROUP BY s_sex;
    
    +-------+--------+
    | s_sex | number |
    +-------+--------+
    | 女    |      4 |
    | 男    |      4 |
    +-------+--------+
    ```

22. 查询名字中含有「风」字的学生信息

    ```	sql
    SELECT *
    FROM Student
    WHERE s_name LIKE '%风%';
    
    +------+--------+------------+-------+
    | s_id | s_name | s_birth    | s_sex |
    +------+--------+------------+-------+
    | 03   | 孙风   | 1990-05-20 | 男    |
    +------+--------+------------+-------+
    ```

23. 查询同名同性学生名单，并统计同名人数

    ```sql
    SELECT *
    FROM Student
    LEFT JOIN (SELECT s_name,s_sex,COUNT(*) AS 同名人数
               FROM Student
               GROUP BY s_name,s_sex) AS t1
    ON Student.s_name=t1.s_name AND Student.s_sex=t1.s_sex
    WHERE t1.同名人数>1;
    ```

24. 查询 1990 年出生的学生名单

    ```sql
    SELECT *
    FROM Student
    WHERE YEAR(s_birth)=1990;
    
    +------+--------+------------+-------+
    | s_id | s_name | s_birth    | s_sex |
    +------+--------+------------+-------+
    | 01   | 赵雷   | 1990-01-01 | 男    |
    | 02   | 钱电   | 1990-12-21 | 男    |
    | 03   | 孙风   | 1990-05-20 | 男    |
    | 04   | 李云   | 1990-08-06 | 男    |
    | 08   | 王菊   | 1990-01-20 | 女    |
    +------+--------+------------+-------+
    ```

25. 查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列

    ```sql
    SELECT c_id,AVG(s_score) AS 平均成绩
    FROM Score
    GROUP BY c_id
    ORDER BY 平均成绩 DESC,c_id ASC;
    
    +------+--------------+
    | c_id | 平均成绩     |
    +------+--------------+
    | 02   |      72.6667 |
    | 03   |      68.5000 |
    | 01   |      64.5000 |
    +------+--------------+
    ```

26. 查询平均成绩大于等于 85 的所有学生的学号、姓名和平均成绩

    ```sql
    SELECT Student.s_id,Student.s_name,t1.平均成绩
    FROM Student
    INNER JOIN (SELECT s_id,AVG(Score.s_score) AS 平均成绩
               FROM Score
               GROUP BY s_id
               HAVING 平均成绩>85) AS t1
    ON Student.s_id=t1.s_id;
    
    +------+--------+--------------+
    | s_id | s_name | 平均成绩     |
    +------+--------+--------------+
    | 01   | 赵雷   |      89.6667 |
    | 07   | 郑竹   |      93.5000 |
    +------+--------+--------------+
    ```

27. 查询课程名称为「数学」，且分数低于 60 的学生姓名和分数

    ```sql
    SELECT Student.s_name,Score.s_score
    FROM Student
    LEFT JOIN Score
    ON Student.s_id=Score.s_id
    WHERE Score.c_id=(SELECT c_id
                      FROM Course
                      WHERE c_name='数学')
    AND Score.s_score<60;
    
    +--------+---------+
    | s_name | s_score |
    +--------+---------+
    | 李云   |      30 |
    +--------+---------+
    ```

28. 查询所有学生的课程及分数情况（存在学生没成绩，没选课的情况）

    ```sql
    SELECT Student.s_id,Score.c_id,Score.s_score
    FROM Student
    LEFT JOIN Score
    ON Student.s_id=Score.s_id;
    +------+------+---------+
    | s_id | c_id | s_score |
    +------+------+---------+
    | 01   | 01   |      80 |
    | 01   | 02   |      90 |
    | 01   | 03   |      99 |
    | 02   | 01   |      70 |
    | 02   | 02   |      60 |
    | 02   | 03   |      80 |
    | 03   | 01   |      80 |
    | 03   | 02   |      80 |
    | 03   | 03   |      80 |
    | 04   | 01   |      50 |
    | 04   | 02   |      30 |
    | 04   | 03   |      20 |
    | 05   | 01   |      76 |
    | 05   | 02   |      87 |
    | 06   | 01   |      31 |
    | 06   | 03   |      34 |
    | 07   | 02   |      89 |
    | 07   | 03   |      98 |
    | 08   | NULL |    NULL |
    +------+------+---------+
    
    ```

29. 查询任何一门课程成绩在 70 分以上的姓名、课程名称和分数

    ```sql
    SELECT Student.s_name,Course.c_id,Score.s_score
    FROM Student
    LEFT JOIN Score
    ON Student.s_id=Score.s_id
    LEFT JOIN Course
    ON Score.c_id=Course.c_id
    WHERE Score.s_score>70;
    
    +--------+------+---------+
    | s_name | c_id | s_score |
    +--------+------+---------+
    | 赵雷   | 01   |      80 |
    | 赵雷   | 02   |      90 |
    | 赵雷   | 03   |      99 |
    | 钱电   | 03   |      80 |
    | 孙风   | 01   |      80 |
    | 孙风   | 02   |      80 |
    | 孙风   | 03   |      80 |
    | 周梅   | 01   |      76 |
    | 周梅   | 02   |      87 |
    | 郑竹   | 02   |      89 |
    | 郑竹   | 03   |      98 |
    +--------+------+---------+
    ```

30. 查询不及格的课程

    ```sql
    SELECT Course.c_id,Score.s_id,Score.s_score
    FROM Score
    LEFT JOIN Course
    ON Score.c_id=Course.c_id
    WHERE Score.s_score<60;
    
    +------+------+---------+
    | c_id | s_id | s_score |
    +------+------+---------+
    | 01   | 04   |      50 |
    | 02   | 04   |      30 |
    | 03   | 04   |      20 |
    | 01   | 06   |      31 |
    | 03   | 06   |      34 |
    +------+------+---------+
    ```

31. 查询课程编号为 01 且课程成绩在 80 分以上的学生的学号和姓名

    ```sql
    SELECT Student.s_id,Student.s_name,Score.s_score
    FROM Student
    INNER JOIN Score
    ON Student.s_id=Score.s_id
    WHERE Score.c_id='01' AND Score.s_score>80;
    ```

32. 求每门课程的学生人数

    ```sql
    SELECT Score.c_id,COUNT(Score.s_id) AS 学生人数
    FROM Score
    GROUP BY Score.c_id;
    
    +------+--------------+
    | c_id | 学生人数     |
    +------+--------------+
    | 01   |            6 |
    | 02   |            6 |
    | 03   |            6 |
    +------+--------------+
    ```

33. 成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩

    ```sql
    SELECT Student.*,t1.s_score
    FROM Student
    INNER JOIN (SELECT s_id,s_score
                FROM Score
                WHERE c_id=(SELECT c_id
                           FROM Teacher
                           WHERE Teacher.t_name='张三')
                ORDER BY s_score DESC
                LIMIT 1) AS t1
    ON Student.s_id=t1.s_id;	
    
    +------+--------+------------+-------+---------+
    | s_id | s_name | s_birth    | s_sex | s_score |
    +------+--------+------------+-------+---------+
    | 01   | 赵雷   | 1990-01-01 | 男    |      80 |
    +------+--------+------------+-------+---------+
    ```

34. 成绩有重复的情况下，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩

    ```sql
    SELECT Student.*,t1.s_score
    FROM Student
    INNER JOIN (SELECT s_id,s_score
                FROM Score
                WHERE c_id=(SELECT c_id
                            FROM Teacher
                            WHERE Teacher.t_name='张三')
                AND s_score=(SELECT MAX(s_score)
                             FROM Score
                             WHERE c_id=(SELECT c_id
                                         FROM Teacher
                                         WHERE Teacher.t_name='张三'))) AS t1
    ON Student.s_id=t1.s_id;	
    
    +------+--------+------------+-------+---------+
    | s_id | s_name | s_birth    | s_sex | s_score |
    +------+--------+------------+-------+---------+
    | 01   | 赵雷   | 1990-01-01 | 男    |      99 |
    +------+--------+------------+-------+---------+
    ```

35. 查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩

    ```sql
    SELECT *
    FROM Score AS t1
    WHERE EXISTS(SELECT * 
                 FROM Score AS t2
                 WHERE t1.s_id=t2.s_id
                 AND t1.c_id!=t2.c_id
                 AND t1.s_score=t2.s_score);
                 
    +------+------+---------+
    | s_id | c_id | s_score |
    +------+------+---------+
    | 03   | 01   |      80 |
    | 03   | 02   |      80 |
    | 03   | 03   |      80 |
    +------+------+---------+
    ```

36. 查询每门功成绩最好的前两名

    ```sql
    SELECT *
    FROM Score
    WHERE (SELECT COUNT(*)
           FROM Score as Score1
           WHERE Score.c_id=Score1.c_id AND Score.s_score<Score1.s_score)<2
    ORDER BY c_id,s_score DESC;
    
    +------+------+---------+
    | s_id | c_id | s_score |
    +------+------+---------+
    | 01   | 01   |      80 |
    | 03   | 01   |      80 |
    | 01   | 02   |      90 |
    | 07   | 02   |      89 |
    | 01   | 03   |      99 |
    | 07   | 03   |      98 |
    +------+------+---------+
    ```

37. 统计每门课程的学生选修人数（超过 5 人的课程才统计）。

    ```sql
    SELECT Score.c_id,COUNT(Score.s_id) AS 学生人数
    FROM Score
    GROUP BY Score.c_id
    HAVING 学生人数>5;
    
    +------+--------------+
    | c_id | 学生人数     |
    +------+--------------+
    | 01   |            6 |
    | 02   |            6 |
    | 03   |            6 |
    +------+--------------+
    ```

38. 检索至少选修两门课程的学生学号

    ```sql
    SELECT s_id
    FROM Score
    GROUP BY s_id
    HAVING COUNT(c_id)>=2;
    
    +------+
    | s_id |
    +------+
    | 01   |
    | 02   |
    | 03   |
    | 04   |
    | 05   |
    | 06   |
    | 07   |
    +------+
    ```

39. 查询选修了全部课程的学生信息

    ```sql
    SELECT *
    FROM Student
    WHERE NOT EXISTS(SELECT *
                     FROM Course
                     WHERE NOT EXISTS(SELECT *
                                      FROM Score
                                      WHERE s_id=Student.s_id
                                      AND c_id=Course.c_id));
                                      
    +------+--------+------------+-------+
    | s_id | s_name | s_birth    | s_sex |
    +------+--------+------------+-------+
    | 01   | 赵雷   | 1990-01-01 | 男    |
    | 02   | 钱电   | 1990-12-21 | 男    |
    | 03   | 孙风   | 1990-05-20 | 男    |
    | 04   | 李云   | 1990-08-06 | 男    |
    +------+--------+------------+-------+
    ```

40. 查询各学生的年龄，只按年份来算

    ```sql
    SELECT Student.*,YEAR(NOW())-YEAR(s_birth) AS age
    FROM Student;
    
    +------+--------+------------+-------+------+
    | s_id | s_name | s_birth    | s_sex | age  |
    +------+--------+------------+-------+------+
    | 01   | 赵雷   | 1990-01-01 | 男    |   28 |
    | 02   | 钱电   | 1990-12-21 | 男    |   28 |
    | 03   | 孙风   | 1990-05-20 | 男    |   28 |
    | 04   | 李云   | 1990-08-06 | 男    |   28 |
    | 05   | 周梅   | 1991-12-01 | 女    |   27 |
    | 06   | 吴兰   | 1992-03-01 | 女    |   26 |
    | 07   | 郑竹   | 1989-07-01 | 女    |   29 |
    | 08   | 王菊   | 1990-01-20 | 女    |   28 |
    +------+--------+------------+-------+------+
    ```

41. 按照出生日期来算，当前月日 < 出生年月的月日则，年龄减一

    ```sql
    SELECT Student.*,
           (CASE
            WHEN MONTH(NOW())<MONTH(s_birth)
            THEN YEAR(NOW())-YEAR(s_birth)-1
            ELSE YEAR(NOW())-YEAR(s_birth)
            END) AS age
    FROM Student;
    
    +------+--------+------------+-------+------+
    | s_id | s_name | s_birth    | s_sex | age  |
    +------+--------+------------+-------+------+
    | 01   | 赵雷   | 1990-01-01 | 男    |   28 |
    | 02   | 钱电   | 1990-12-21 | 男    |   27 |
    | 03   | 孙风   | 1990-05-20 | 男    |   28 |
    | 04   | 李云   | 1990-08-06 | 男    |   28 |
    | 05   | 周梅   | 1991-12-01 | 女    |   26 |
    | 06   | 吴兰   | 1992-03-01 | 女    |   26 |
    | 07   | 郑竹   | 1989-07-01 | 女    |   29 |
    | 08   | 王菊   | 1990-01-20 | 女    |   28 |
    +------+--------+------------+-------+------+
    ```

42. 查询本周过生日的学生

    ```sql
    SELECT Student.*
    FROM Student
    WHERE WEEKOFYEAR(s_birth)=WEEKOFYEAR(CURDATE());
    ```

43. 查询下周过生日的学生

    ```sql
    SELECT Student.*
    FROM Student
    WHERE WEEKOFYEAR(s_birth)=WEEKOFYEAR(CURDATE())+1;
    ```

44. 过生日查询本月过生日的学生

    ```sql
    SELECT Student.*
    FROM Student
    WHERE MONTH(NOW())=MONTH(s_birth);
    ```

45. 查询下月过生日的学生

    ```sql
    SELECT Student.*
    FROM Student
    WHERE MONTH(s_birth)-MONTH(NOW())=1;
    ```
