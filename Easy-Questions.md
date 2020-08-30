---ERD Diagram : https://sqlzoo.net/wiki/Help_Desk

### 1. There are three issues that include the words "index" and "Oracle". Find the call_date for each of them

```SQL
select call_date, call_ref
from Issue 
where Detail like '%index%' and Detail like '%Oracle%'
```

### 2. Samantha Hall made three calls on 2017-08-14. Show the date and time for each

```SQL
select i.call_date, ca.first_name, ca.last_name
from Issue i
inner join Caller ca on i.caller_id = ca.caller_id
where ca.first_name = 'Samantha' and ca.last_name = 'Hall'
and date(call_date) = '2017-08-14'
```

### 3. There are 500 calls in the system (roughly). Write a query that shows the number that have each status.

```SQL
select i.status, count(*) as Volume
from Issue i
group by 1
```

### 4. Calls are not normally assigned to a manager but it does happen. How many calls have been assigned to staff who are at Manager Level?

```SQL
select count(i.call_ref) as mlcc
from Issue i
inner join (
select staff_code
from Staff
where level_code > 3) as s
on i.assigned_to = s.staff_code
```

### 5. Show the manager for each shift. Your output should include the shift date and type; also the first and last name of the manager.

```SQL
select s.shift_date, s.shift_type, st.first_name, st.last_name
from Shift s
inner join Staff st
on s.manager = st.staff_code
order by 1
```
