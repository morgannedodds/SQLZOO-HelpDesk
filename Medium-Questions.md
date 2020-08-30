---ERD Diagram : https://sqlzoo.net/wiki/Help_Desk

### 6. List the Company name and the number of calls for those companies with more than 18 calls.

select c.company_name, count(*) as CC
from Customer c
inner join Caller cc
on c.company_ref = cc.company_ref
inner join Issue i
on cc.caller_id = i.caller_id
group by c.company_name
having count(*) > 18


### 7. Find the callers who have never made a call. Show first name and last name

select c.first_name, c.last_name
from Caller c
left join Issue i
on c.caller_id = i.caller_id
where call_ref is NULL


### 8. For each customer show: Company name, contact name, number of calls where the number of calls is fewer than 5

select cu.company_name,ca2.first_name,ca2.last_name,count(*) as nc
from Customer cu
inner join Caller ca on ca.company_ref = cu.company_ref 
inner join Issue i on i.caller_id = ca.caller_id
inner join Caller ca2 on ca2.caller_id = cu.contact_id
group by cu.company_name,ca2.first_name,ca2.last_name
having nc < 5


### 9. For each shift show the number of staff assigned. Beware that some roles may be NULL and that the same person might have been assigned to multiple roles
(The roles are 'Manager', 'Operator', 'Engineer1', 'Engineer2').

with shift_union as
(select s.shift_date, s.shift_type, manager as role
from Shift s
UNION ALL
select s.shift_date, s.shift_type, operator as role
from Shift s
UNION ALL
select s.shift_date, s.shift_type, Engineer1 as role
from Shift s
UNION ALL
select s.shift_date, s.shift_type, Engineer2 as role
from Shift s 
)
select shift_date, shift_type, count(distinct role) as cw
from Shift_Union
group by 1, 2

### 10. Caller 'Harry' claims that the operator who took his most recent call was abusive and insulting. Find out who took the call (full name) and when.

select s.first_name, s.last_name, i.call_date
from Issue i
inner join Caller ca on i.caller_id = ca.caller_id
inner join Staff s on i.taken_by = s.staff_code
where ca.first_name = 'Harry'
order by i.call_date desc
limit 1
