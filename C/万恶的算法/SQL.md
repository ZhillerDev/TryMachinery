## 牛客刷题

<br>

### 非技术快速入门

#### 基础查询

查询所有列 `select * from xxx`

查询多列 `select id,age from xxx`

查询结果去重 `select distinct id from xxx`

查询结果限制返回行 `SELECT device_id FROM user_profile ORDER BY id LIMIT 2`

查询后列重新命名 `select device_id as user_infos_example from user_profile limit 2`

<br>

#### 条件查询

从用户信息表中取出满足条件的数据，结果返回设备 id 和学校  
`Select device_id,university FROM user_profile where university = "北京大学" and device_id = user_profile.device_id;`

查询年龄大于 24 岁的用户信息  
`SELECT device_id, gender, age, university FROM user_profile WHERE age is not null and age > 24`

查询某一范围的数据  
`SELECT device_id,gender,age FROM user_profile WHERE age BETWEEN 20 AND 23`

查询排除某一数据在外的所有数据  
`SELECT device_id,gender,age,university FROM user_profile WHERE university NOT IN ('复旦大学')`

where 过滤空值 `select device_id,gender,age,university from user_profile where age is not NULL;`

操作符练习 `SELECT device_id,gender,age,university,gpa FROM user_profile WHERE gender = 'male' AND gpa > 3.5`

where in && not in  
`select device_id,gender,age,university,gpa from user_profile where university in('北京大学','复旦大学','山东大学');`

操作符混用

```sql
select device_id,gender,age,university,gpa from user_profile
where (university='山东大学' and gpa>3.5 )
or (university="复旦大学" and gpa>3.8);
```

<br>

模糊查询与字符匹配

```sql
SELECT device_id,age,university FROM user_profile
WHERE university LIKE '%北京%'
```

<br>

#### 高级查询

找到 GPA 最大的复旦学生

```sql
select max(gpa) as gpa
from user_profile
where university='复旦大学';
```

<br>

sum && average

```sql
select
    count(gender) as male_num,
    round(avg(gpa), 1) as avg_gpa
from
    user_profile
where
    gender = "male";
```

<br>

分组 group by 实现过滤的基础使用

```sql
select
    gender,
    university,
    count(device_id) as user_num,
    avg(active_days_within_30) as avg_active_days,
    avg(question_cnt) as avg_question_cnt
from
    user_profile
group by
    gender,
    university
```

<br>

#### 多表查询

给定两个表 question_practice_detail（不包含 university 属性） 以及 user_profile，筛选出 user_profile 表中所有浙大学生，并从 question_practice_detail 表中取出对应浙大学生的数据  
最后根据 question_id 升序排列

```sql
select
    qpd.device_id,
    qpd.question_id,
    qpd.result
from
    question_practice_detail as qpd
    inner join user_profile as up on up.device_id = qpd.device_id and up.university = '浙江大学'
order by
    question_id
```

<br>

使用 SQL 查找每个学校用户的平均答题数目(说明：某学校用户平均答题数量计算方式为该学校用户答题总次数除以答过题的不同用户个数)根据示例，结果按照 university 升序排序

```sql
select
    university,
    count(question_id) / count(distinct qpd.device_id) as avg_answer_cnt
from
    question_practice_detail as qpd
    inner join user_profile as up on qpd.device_id = up.device_id
group by
    university
```

<br>

联合查询  
收集两个表，然后使用 `union all` 把这两个表拼接起来

```sql
select
    device_id,
    gender,
    age,
    gpa
from
    user_profile
where
    university = '山东大学'
union all
select
    device_id,
    gender,
    age,
    gpa
from
    user_profile
where
    gender = 'male'
```

<br>

#### 必会常用函数

统计 user_profile 表内 age 小于 25 岁的，以及大于 25 岁的各自有多少个人，插入到新表里面

```sql
SELECT
    -- CASE END 实现类似于switch选择的效果，记住结尾要加上END
    CASE
        WHEN age < 25
        OR age IS NULL THEN '25岁以下'
        WHEN age >= 25 THEN '25岁及以上'
    END age_cut,
    COUNT(*) number
FROM
    user_profile
GROUP BY
    age_cut
```

<br>

日期格式化函数的使用  
需要取出指定日期，小明写了多少道题，限定必须在 2021 年的八月份内的所有日期才有效

```sql
select
    day (date) as day,
    count(question_id) as question_cnt
from
    question_practice_detail
where
    month (date) = 8
    and year (date) = 2021
group by
    date
```

<br>

字符串操作函数  
通过使用 SUBSTRING_INDEX 函数，使用逗号分隔字符串，并取出倒数第一个字符串

请注意！SUBSTRING_INDEX 取出的是一段字符串，而不是指定序号位置的字符串！！！  
比如 `SUBSTRING_INDEX (profile, ",", 3)` 就表示取出前三个序号分隔字符串

```sql
SELECT
    SUBSTRING_INDEX (profile, ",", -1) as gender,
    COUNT(*) as number
FROM
    user_submit
GROUP BY
    gender;
```

<br>

#### 综合大题

<br>

### SQL 必知必会
