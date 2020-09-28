# SQL

## 数据库配置
```sql
# 查看数据库编码配置
show variables like 'character%';
# 修改数据库客户端编码配置
set character_set_client='utf8mb4';
# 修改数据库连接器编码配置
set character_set_connection='utf8mb4';
# 修改数据库编码配置
set character_set_database='utf8mb4';
# 修改数据库结果编码配置
set character_set_results='utf8mb4';
# 修改数据库服务器编码配置
set character_set_server='utf8mb4';
```

## 查询语句
```sql
# 从数据表中检索单个字段
select <FIELD_NAME> from <TABLE_NAME>;
# 从数据表中检索多个字段
select <FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME>;
# 从数据表中检索所有字段
select * from <TABLE_NAME>;
# 从数据表中检索单个字段并只返回不同记录
select distinct <FIELD_NAME> from <TABLE_NAME>;
# 从数据表中检索单个字段并只返回前N条记录
SQL Server && Access : select top <N> <FIELD_NAME> from <TABLE_NAME>;
DB2 : select <FIELD_NAME> from <TABLE_NAME> fetch first <N> rows only;
Oracle : select <FIELD_NAME> from <TABLE_NAME> where rownum = '<N>';
MySQL && MariaDB && PostgreSQL && SQLite : select <FIELD_NAME> from <TABLE_NAME> limit <N>;
# 从数据表中检索从N1行到N2行的记录
select <FIELD_NAME> from <TABLE_NAME> limit <N1> offset <N2>;
```

##排序检索数据
```sql
# 从数据表中检索单个字段并按照值排序
select <FIELD_NAME> from <TABLE_NAME> order by <FIELD_NAME>;
# 从数据表中检索多个字段并按照值排序
select <FIELD_NAME>,<FIELD_NAME>,<FIELD_NAME>, from <TABLE_NAME> order by <FIELD_NAME>,<FIELD_NAME>;
# 从数据表中检索多个字段并按字段排序
select <FIELD_NAME>,<FIELD_NAME>,<FIELD_NAME>, from <TABLE_NAME> order by 2,3;
# 从数据表中检索多个字段并按值降序排序
select <FIELD_NAME>,<FIELD_NAME>,<FIELD_NAME>, from <TABLE_NAME> order by <FIELD_NAME> desc;
select <FIELD_NAME>,<FIELD_NAME>,<FIELD_NAME>, from <TABLE_NAME> order by <FIELD_NAME> desc <FIELD_NAME>;
```

## 过滤数据
```sql
# 从数据表中检索多个字段并返回符合过滤条件的记录
select <FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME> where <FIELD_NAME> >!=< <CONDITION>;
# 从数据表中检索多个字段并返回在N1,N2之间的记录
select <FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME> between <N1> and <N2>;
# 从数据表中检索单个字段并返回值不为空的记录
select <FIELD_NAME> from <TABLE_NAME> where <FIELD_NAME> is null;
# 从数据表中检索多个字段并返回符合多重过滤条件的记录
select <FIELD_NAME>,<FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME> where <FIELD_NAME> >!=< '<CONDITION>' and <FIELD_NAME> <!=> '<CONDITION>';
# 从数据表中检索多个字段并返回符合多重过滤条件中其中一个条件的记录
select <FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME> where <FIELD_NAME> >!=< '<CONDITION>' or <FIELD_NAME> >!=< '<CONDITION>';
# 从数据表中检索多个字段并返回符合多重过滤条件中其中一个条件且按值排序的记录
select <FIELD_NAME>,<FIELD_NAME> fron <TABLE_NAME> where <FIELD_NAME> in ('<CONDITION>','<CONDITION>') order by <FIELD_NAME>;
# 从数据表中检索多个字段并返回不符合过滤条件且按值排序的记录
select <FIELD_NAME>,<FIELD_NAME> fron <TABLE_NAME> where not <FIELD_NAME> = '<CONDITION>' order by <FIELD_NAME>;
# 从数据表中检索多个字段并返回符合通配符过滤条件的记录 # _单个字符 %多个字符 []符合其中某一个值
select <FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME> where <FIELD_NAME> like '<CONDITION>%'
select <FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME> where <FIELD_NAME> like '_<CONDITION>'
select <FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME> where <FIELD_NAME> like '[<CONDITION>]%'
```

## 拼接计算
```sql
# 从数据表中检索多个字段并返回拼接后且按值排序的记录
select <FIELD_NAME> + '(' + <FIELD_NAME> + ')' from <TABLE_NAME> order by <FIELD_NAME>;
# 从数据表中检索多个字段并返回拼接后去除空格且按值排序的记录
select <FIELD_NAME> + '(' + trim(<FIELD_NAME>) + ')' from <TABLE_NAME> order by <FIELD_NAME>;
# 将拼接字段设置别名
select <FIELD_NAME> + '(' + <FIELD_NAME> + ')' as <ALIAS> from <TABLE_NAME> order by <FIELD_NAME>;
# 从数据表中检索多个字段并返回计算后且符合过滤条件的记录
select <FIELD_NAME>,<FIELD_NAME>,<FIELD_NAME>*<FIELD_NAME> as <ALIAS> from <TABLE_NAME> where <FIELD_NAME> <!=> <CONDITION>;
```

## 函数
```sql
# 将文本转为大写格式
upper()
# 将文本转为小写格式
lower()
# 返回字符串左边的字符
left()
# 返回字符串左边的字符
right()
# 返回字符串的长度
length()
# 去掉字符串左边的空格
ltrim()
# 去掉字符串右边的空格
rtrim()
# 返回字符串的soundex的值
soundex()
# 返回字符串中的年份
year()
# 返回一个数的绝对值
abs()
# 返回一个角度的余弦
cos()
# 返回一个数的指数值
exp()
# 返回圆周率
pi()
# 返回一个角度的正弦
sin()
# 返回一个数的平方根
sqrt()
# 返回一个角度的正切
tan()
# 返回某列的平均值
avg()
# 返回某列的行数
count()
# 返回某列的最大值
max()
# 返回某列的最小值
min()
# 返回某列值之和
sum()

```

## 分组
```sql
# 从数据表中检索多个字段并设置别名且分组输出
select <FIELD_NAME> count(*) as <ALIAS> from <TABLE_NAME> group by <FIELD_NAME>;
# 从数据表中检索多个字段并设置别名且过滤分组后输出
# where是分组前过滤 having是分组后过滤
select <FIELD_NAME>,count(*) as <ALIAS> from <TABLE_NAME> group by <FIELD_NAME> having count(*) <!=> <N>;
```

## 联结
```sql
# 从多张数据表中检索多个字段并返回符合过滤条件的记录 （内联结）
select <FIELD_NAME>,<FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME> <TABLE_NAME> where <TABLE_NAME.FIELD_NAME> = <TABLE_NAME.FIELD_NAME>
# 从多张数据表中检索多个字段并返回符合过滤条件的记录 （自联结）
select <TABLE_NAME.FIELD_NAME>,<TABLE_NAME.FIELD_NAME>,<TABLE_NAME.FIELD_NAME> from <TABLE_NAME> as <ALIAS>,<TABLE_NAME> as <ALIAS> where <TABLE_NAME.FIELD_NAME> = <TABLE_NAME.FIELD_NAME> and <TABLE_NAME.FIELD_NAME> = '';
# 从多张数据表中检索多个字段并返回符合过滤条件的记录 （外联结）
select <TABLE_NAME.FIELD_NAME>,<TABLE_NAME.FIELD_NAME> from <TABLE_NAME> left outer join <TABLE_NAME> on <TABLE_NAME.FIELD_NAME> = <TABLE_NAME.FIELD_NAME>
```

## 组合查询
```sql
# 从单张数据表中组合查询多个字段并返回符合过滤条件的记录
select <FIELD_NAME>,<FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME> where <FIELD_NAME> in ('<CONDITION>','<CONDITION>') union select <FIELD_NAME>,<FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME> where <FIELD_NAME> = '<CONDITION>';
# 从单张数据表中组合查询多个字段并返回符合过滤条件且包含重复的记录
select <FIELD_NAME>,<FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME> where <FIELD_NAME> in ('<CONDITION>','<CONDITION>') union all select <FIELD_NAME>,<FIELD_NAME>,<FIELD_NAME> from <TABLE_NAME> where <FIELD_NAME> = '<CONDITION>';

```

## 增删改
```sql
# 向数据表中插入数据
insert into <TABLE_NAME>(<FIELD_NAME>) values(<VALUE>);
insert into <TABLE_NAME> values(<VALUE>);
# 向数据表中插入检索出的数据
insert into <TABLE_NAME>(<FIELD_NAME>) select <FIELD_NAME> from <TABLE_NAME>;
# 重命名数据表
rename table <TABLE_NAME> to <TABLE_NAME>;
# 将数据从已存在的数据表中复制到新创的数据表中
select * into <TABLE_NAME> from <TABLE_NAME>;
# 将数据从已存在的数据表中复制到新创的数据表中
create table <TABLE_NAME> as select * from <TABLE_NAME>;
# 更新数据表中已有的记录
update <TABLE_NAME> set <FIELD_NAME> = '<CONDITION>' where <FIELD_NAME> = '<CONDITION>';
# 删除数据表中已有的记录
delete from <TABLE_NAME> where <FIELD_NAME> = '<CONDITION>';
```

## 创建数据表
```sql
# 创建数据表
create table <TABLE_NAME>(<FIELD_NAME> <TYPE(SIZE)> <RESTRICTIONS>);
# 删除数据表
drop table <TABLE_NAME>;
# 向已存在的数据表中添加新的字段
alter table <TABLE_NAME> add <FIELD_NAME> <TYPE(SIZE) <RESTRICTIONS>;
# 向已存在的数据表中删除已存在的字段
alter table <TABLE_NAME> drop column <FIELD_NAME>;
```
## 视图
```sql
# 创建视图，简化语句类似于代码片段
create view <VIEW_NAME> as select <FIELD_NAME> from <TABLE_NAME>;
# 使用视图查询
select <FIELD_NAME>,<FIELD_NAME> from <VIEW_NAME> where <FIELD_NAME> = '<CONDITION>';
```