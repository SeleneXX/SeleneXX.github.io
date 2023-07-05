# MySQL

SQL语句在除了具体字段值以外（某个表某一列中的某个具体的值）的部分，不区分大小写。保持大小写风格

```sql
-- 是注释
```

## 1. DQL（查询）语句

```sql
select 列名, 列名, ..., 列名 from 表名 where 条件 and/or 条件 ... group by 列 Having 分组条件 Order by 排序;
```

### 1.1 普通查询

#### 设置别名

```sql
select ename, sal from emp;
select ename 姓名, sal 月薪 from emp;
```

#### 查询时直接计算

```sql
select (sal+comm) * 12 年薪 from emp;
```

#### 查询结果拼接

```sql
select CONCAT(ename, "的月薪是:", sal) 员工月薪 from emp;
```

### 1.2 条件查询

#### 不等于

```sql
select * from emp where job != 'SALESMAN';
select * from emp where job <> 'SALESMAN';
```

#### 在两值之间（包含两端）

```sql
select * from emp where sal BETWEEN 1000 and 3000 ;
```

#### 在集合内

```sql
select * from emp where empno in (7499, 7566, 7782);
select * from emp where empno not in (7499, 7566, 7782);
```

#### 空值

```sql
select * from emp where comm is null ;
```

#### 模糊查询

%代表任意字符出现任意次数

```sql
select * from emp where ename like '%M'; -- 最后一位是M
select * from emp where ename like 'M%'; -- 第一位是M
select * from emp where ename like '%M%'; -- 包含M
```

_代表任意字符出现一次

```sql
select * from emp where ename like '_M'; -- 第二位是M
```

出现特殊字符需要用\进行转译

```sql
select * from emp where ename like '%\%%'; -- 包含%
```

### 1.3 排序

数据默认按主键排序。null默认为最小值

```sql
select * from emp order by sal; -- 按照月薪，升序排序
select * from emp order by job, sal; -- 先按照职位，再按照薪资，升序排序
select ename, sal from emp order by 1; -- 按照查询的第一个目标（ename），升序排序
select ename, job, sal from emp order by job, sal Desc; -- 查询ename，job，sal，先按照job升序，然后按照sal降序
```

### 1.4 分页

数据太多，限制显示的条数

```sql
select ename, job, sal from emp limit 3; -- 显示前3条数据
select ename, job, sal from emp limit 0,3; -- 显示从0开始的后3条数据
```

### 1.5 函数

#### 字符串函数

```sql
select
	lower(ename), -- 全变小写
	length(ename), -- 长度
	lpad(ename, 8, '#'), -- 长度不足8位，则在左侧补充#
	rpad(ename, 8, '#'), -- 长度不足8位，则在右侧补充#
	ltrim("   123   "), -- 左侧去空格
	rtrim("   123   "), -- 右侧去空格
	replace(ename, 'S', '@'), -- 替换S为@
	SUBSTR(ename, 1, 3), -- 截取字符串，从第1位开始，共截取3位
from
	emp;
```

#### 数值函数

```sql
select
	abs(-10), -- 绝对值
	ceil(3.2), -- 向上取整
	floor(3.2), -- 向下取整
	round(3.2), -- 四舍五入
	power(3,2), -- 指数
	mod(3,2), -- 取余
```

#### 日期函数

