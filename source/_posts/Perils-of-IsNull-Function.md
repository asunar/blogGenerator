---
title: Perils of IsNull Function
date: 2018-12-10 08:13:48
tags:
---
Did you know that SQL Server’s IsNull() function does an implicit conversion?

```sql
ISNULL ( check_expression , replacement_value)
```
The documentation explicitly states that

The value of check_expression is returned if it is not NULL; otherwise, replacement_value is returned after **it is implicitly converted to the type of check_expression, if the types are different. replacement_value can be truncated if replacement_value is longer than check_expression.** (emphasis mine)

```sql
DECLARE @TestIsNull table (  
 EmpID varchar(10)  
)  
  
insert into @TestIsNull  
select '1234567890'  
  
DECLARE @empIdFilter varchar(9) = null  
  
select *  
from   
 @TestIsNull   
where  
 empId = ISNULL(@empIdFilter,empid)  
  
EmpID  
----------  
  
(0 rows affected)  

```

See the problem? Data type of EmpId column on line 2 is varchar(10) and the data type of empIdFilter is varchar(9).

Therefore, ISNULL will convert varchar(10) to varchar(9) resulting in truncation. Now imagine this in a stored procedure where the EmpId column datatype is not obvious and filter variable is declared somewhere far away from the SQL statement. It becomes a pretty insidious bug.

In order to avoid this, you can write your SQL statement like this.

```sql
DECLARE @TestIsNull table (  
 EmpID varchar(10)  
)  
  
insert into @TestIsNull  
select '1234567890'  
  
DECLARE @empIdFilter varchar(9) = null  
  
select *  
from   
 @TestIsNull   
where   
 @empIdFilter is null or empid = @empIdFilter  
  
EmpID  
----------  
1234567890  
  
(1 row affected)  

```