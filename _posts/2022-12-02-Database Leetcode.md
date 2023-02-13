---
layout: post
title: Database Leetcode
date: 2022-12-02
Author: Yancy
tags: [Database]
comments: true
toc: true
---
## Basic

### Leetcode 1757

``` 
select product_id
from Products
where low_fats = 'Y' and recyclable = 'Y'
```

### Leetcode 620

``` 
select *
from Cinema
where description <> 'boring'
and id %2 = 1
order by rating desc
```

## JOIN

### Leetcode 181

join

``` 
# Write your MySQL query statement below
select clerk.name Employee
from Employee clerk left join Employee manager on clerk.managerId=manager.id
where clerk.salary>manager.salary;
```

### Leetcode 175

join

``` 
# Write your MySQL query statement below
select a.firstname,a.lastname, b.city, b.state
from Person a left join Address b
on a.personId = b.personId;
```

### Leetcode 577

``` 
select e.name, b.bonus
from Employee e left join Bonus b using(empId)
where b.bonus is null or b.bonus<1000
```

### *Leecode 603

sequential: use distinct and abs(), 

``` 
select distinct a.seat_id
from Cinema a left join Cinema b on abs(a.seat_id - b.seat_id) = 1
where a.free = 1 and b.free = 1
order by a.seat_id asc;
```

### *Leetcode 612

``` 
select round(min(sqrt((POW(a.x - b.x, 2) + POW(a.y - b.y, 2)))),2) shortest
from Point2D a, Point2D b 
where a.x <> b.x or a.y<> b.y
```

### Leetcode 613

``` 
select min(abs(a.x-b.x)) shortest
from point a, point b
where a.x<>b.x
```

### *Leetcode 180

``` 
SELECT DISTINCT
    l1.Num AS ConsecutiveNums
FROM
    Logs l1,
    Logs l2,
    Logs l3
WHERE
    l1.Id = l2.Id - 1
    AND l2.Id = l3.Id - 1
    AND l1.Num = l2.Num
    AND l2.Num = l3.Num
```

``` 
select distinct sub.lnum ConsecutiveNums
from(
    select l.id id, l.num lnum
    from logs l join logs s on l.id = s.id-1
    where l.num = s.num
)sub join logs t on sub.id = t.id-2
where sub.lnum = t.num
```

### Leetcode 262

``` 
select total.day Day, ifnull(round(cancel.can/total.tot,2),0.00) 'Cancellation Rate'
from (
    select t.request_at day, count(t.id) tot 
    from Trips t, Users u1 , Users u2 
    where t.client_id = u1.users_id and t.driver_id = u2.users_id
    and u1.banned = 'No' and u2.banned = 'No'
    and t.request_at>= "2013-10-01" and t.request_at <= "2013-10-03"
    group by t.request_at
) total left join (
    select t.request_at day, count(t.id) can
    from Trips t, Users u1 , Users u2 
    where t.client_id = u1.users_id and t.driver_id = u2.users_id
    and u1.banned = 'No' and u2.banned = 'No'
    and t.request_at>= "2013-10-01" and t.request_at <= "2013-10-03"
    and t.status like "cancelled%" 
    group by t.request_at
)cancel on total.day = cancel.day
```



## Duplicate

### Leetcode 169

delete duplicate

``` 
# Please write a DELETE statement and DO NOT write a SELECT statement.
# Write your MySQL query statement below
delete p1 from Person p1, Person p2
where p1.email = p2.email and p1.id > p2.id;
```

### Leetcode 182

select duplicate--self join

``` 
select distinct a.email 
from Person a ,Person b 
where a.email = b.email and a.id<>b.id;
```

or: group by and having

``` 
select distinct a.email 
from Person a 
group by email having count(*)>1;
```

## Date

### Leetcode 197

subdate: reduce one day`SUBDATE(*date*, INTERVAL *value unit*)/SUBDATE(date, days)`

``` 
select a.id
from Weather a left join Weather b on b.recordDate = subdate(a.recordDate,1)
where a.temperature> b.temperature;
```

### *Leetcode 1141

datediff( date, date)

``` 
select activity_date day, count(distinct user_id) active_users
from Activity
where datediff('2019-07-27', activity_date) >= 0
and datediff('2019-07-27', activity_date) < 30
group by activity_date 
```

## Aggregate

### *Leetcode 534

``` 
select player_id, event_date, sum(games_played) over(partition by player_id order by event_date asc) games_played_so_far
from Activity
```

### *Leetcode 1795

aggregate the column, union 

``` 
# Write your MySQL query statement below
select product_id, 'store1' store, store1 price from Products where store1 is not null
union 
select product_id, 'store2' store, store2 price from Products where store2 is not null
union
select product_id, 'store3' store, store3 price from Products where store3 is not null
```

### Leetcode 1581

group by and distinct: when have the aggregation function, use group by 

``` 
select customer_id, count(customer_id) count_no_trans
from Visits a left join Transactions b
on a.visit_id = b.visit_id
where b.visit_id is null
group by customer_id
```

### Leetcode 1126

``` 
with cte as(
    select event_type, avg(occurences) ave
    from Events
    group by event_type 
)
select business_id 
from Events left join cte using(event_type)
where occurences>ave
group by business_id 
having count(event_type)>1
```

## Case

### Leetcode 610

``` 
select x, y, z, case when ((x+y>z) and (x+z>y) and (y+z>x) ) then 'Yes' else 'No' end triangle
from Triangle
```

### *Leetcode 1873

use case and like

``` 
select employee_id, 
case when employee_id % 2 = 1 and name not like 'M%' then salary
else 0 
end
as bonus
from Employees
order by employee_id;
```

### Leetcode 627

``` 
update Salary
set sex = case sex when 'm' then 'f'
else 'm'
end;
```

### *Leetcode 626

``` 
with cte as(
    select *, count(*) over() cnt from seat 
)

select (
    case when (id % 2 = 1 and cnt <> id) then id + 1 
    when (id % 2 = 1 and cnt = id) then id
    else id - 1
    end
    ) id, student
from cte
order by id
```

### Leetcode 608

``` 
select id,
    case when p_id is null then 'Root' 
    when id in (select p_id from tree ) then 'Inner'
    else 'Leaf'
    end type
from tree
order by id
```

### *Leetcode 1127

``` 
# Write your MySQL query statement below
with cte as(
    select user_id,spend_date, 
        sum(case when platform ='mobile' then amount else 0 end) mobile_a,
        sum(case when platform ='desktop' then amount else 0 end) desktop_a
    from Spending
    group by spend_date, user_id
)
select spend_date, 'desktop' platform,
    sum(case when mobile_a=0 and desktop_a>0 then desktop_a else 0 end) total_amount, 
    sum(case when mobile_a=0 and desktop_a>0 then 1 else 0 end) total_users
from cte
group by spend_date
union
select spend_date, 'mobile' platform,
    sum(case when mobile_a>0 and desktop_a=0 then mobile_a else 0 end) total_amount, 
    sum(case when mobile_a>0 and desktop_a=0 then 1 else 0 end) total_users
from cte
group by spend_date
union
select spend_date, 'both' platform,
    sum(case when mobile_a>0 and desktop_a>0 then mobile_a+desktop_a else 0 end) total_amount, 
    sum(case when mobile_a>0 and desktop_a>0 then 1 else 0 end) total_users
from cte
group by spend_date
```



## Group by

### Leetcode 178

``` 
select score, dense_rank() over(order by score desc) 'rank'
from Scores
```

### Leetcode 570

``` 
select e.name 
from(
    select count(name) cnt, managerId
    from Employee
    where managerId <>'None'
    group by managerId
)sub join Employee e on sub.managerId = e.id
where sub.cnt>=5
```

``` 
SELECT
    Name
FROM
    Employee AS t1 JOIN
    (SELECT
        ManagerId
    FROM
        Employee
    GROUP BY ManagerId
    HAVING COUNT(ManagerId) >= 5) AS t2
    ON t1.Id = t2.ManagerId
```

### Leetcode 1729

``` 
select user_id, count(user_id) followers_count
from Followers
group by user_id
order by user_id;
```

### Leetcode 1075

attention: round() 

``` 
select p.project_id, round(avg(experience_years),2) average_years
from project p join employee e using(employee_id)
group by p.project_id
```



### Leetcode 1303

group by the column not selected: window function

``` 
select employee_id, count(team_id) over (partition by team_id )team_size
from Employee
```

### Leetcode 596

group by... having....

``` 
# Write your MySQL query statement below
select class
from Courses
group by class having count(class)>=5;
```

### *Leetcode 1484

group_concat()

``` 
select sell_date, count(distinct product) num_sold, group_concat(distinct product order by  product asc) products
from Activities
group by sell_date
```

### Leetcode 511

min, and group by

``` 
select player_id, min(event_date) first_login
from Activity
group by player_id 
```

### *Leetcode 512

the use of first_value() over(partition by .... order by......)

``` 
# Write your MySQL query statement below
select distinct player_id, first_value(device_id) over (PARTITION BY player_id
        ORDER BY event_date) device_id
from Activity
```

### Leetcode 550

``` 
select round(
    (
        select count(*)
        from(
            select player_id, min(event_date) min_date
            from Activity
            group by player_id
        )sub1, Activity a
        where sub1.min_date = a.event_date-1 and sub1.player_id = a.player_id
    )
    /
    (
        select count(distinct player_id)
        from Activity
    )
,2) fraction
```

### Leetcode 580

``` 
with ct1 as (
    select dept_id, count(student_id) student_number
    from Student 
    group by dept_id
)
select dept_name, ifnull(student_number,0) student_number
from Department left join ct1 using(dept_id)
order by student_number desc, dept_name asc
```



### *Leetcode 597

ifnull, distinct two columns

``` 
select ifnull(
    round(
        (select count(distinct requester_id, accepter_id) from RequestAccepted)
        /
        (select count(distinct sender_id,send_to_id) from FriendRequest )
       ,
    2),
0) accept_rate 
```

### *Leetcode 578

``` 
with ct1 as(
    select question_id, count(id) cnt_show
    from SurveyLog
    where action = 'show'
    group by question_id
),
ct2 as(
    select question_id, count(id) cnt_answer
    from SurveyLog
    where action = 'answer'
    group by question_id
)
select question_id survey_log
from ct1 left join ct2 using(question_id)
order by (cnt_answer/cnt_show) desc, question_id asc
limit 1
```

``` 
select question_id as survey_log
from surveylog
group by question_id
order by sum(case when action = 'answer' then 1 else 0 end) 
  / 
  sum(case when action = 'show' then 1 else 0 end) desc,
  question_id asc
limit 1
```



### Leetcode 1407

ifnull and order by two column

``` 
select name, ifnull(sum(distance),0) travelled_distance
from Users left join Rides on Rides.user_id = Users.id
group by user_id
order by travelled_distance desc, name asc
```

### *Leetcode 1107

``` 
select login_date, count(user_id) user_count
from(
    select user_id, min(activity_date) login_date
    from traffic 
    where activity = 'login'
    group by user_id
) subtable
where datediff('2019-06-30',login_date) <= 90
group by login_date

```

### Leetcode 1113

``` 
select extra report_reason, count(distinct post_id) report_count
from Actions
where action_date = '2019-07-04'
and action = 'report'
group by extra
```

### Leetcode 619

count()

``` 
select max(o_num) num
from(select num o_num ,count(num) c_num from MyNumbers group by num) t
where c_num = 1;
```

### *Leetcode 602

``` 
with cte as(
    select requester_id id1, accepter_id id2
    from RequestAccepted
    union
    select accepter_id id1, requester_id id2 
    from RequestAccepted
)
select id1 id, count(id2) num
from cte
group by id1
order by count(id2) desc
limit 1
```

### Leetcode 614

``` 
with cte1 as(
    select followee name, count(*) ee
    from follow 
    group by followee
)
,
cte2 as(
    select follower name, count(*) er
    from follow
    group by follower
)
select name follower, cte1.ee num
from cte1 left join cte2 using(name)
where cte1.ee>=1 and cte2.er>=1
order by name asc
```

### Leetcode 615

date_format()

``` 
with cte as(
    select *, 
        avg(amount) over(partition by date_format(pay_date, '%Y-%m')) total, 
        avg(amount) over(partition by date_format(pay_date, '%Y-%m'),e.department_id) dept
    from salary s left join employee e using(employee_id)
)
select distinct date_format(pay_date, '%Y-%m') pay_month,
    department_id, 
    (case when total>dept then 'lower'
    when total < dept then 'higher'
    else 'same'
    end) comparison
from cte
```



### *Leetcode 618

``` 
with cte as(
    select 
        case when continent = 'America' then name end America,
        case when continent = 'Asia' then name end Asia,
        case when continent = 'Europe' then name end Europe,
        row_number() over(partition by continent order by name) As rn
    from Student
)
select max(America) as America ,max(Asia) as Asia, max(Europe) as Europe
from cte
group by rn
```

### Leetcode 1045

``` 
select c.customer_id 
from Customer c
group by c.customer_id 
having count(distinct product_key) = (select count(distinct product_key) from Product)
```

### Leetcode 1050

``` 
select actor_id, director_id
from ActorDirector
group by actor_id, director_id
having count(timestamp) >=3
```

### Leetcode 1097

``` 
# Write your MySQL query statement below
with cte1 as(
    select player_id, min(event_date) install_dt
    from Activity 
    group by player_id

),
cte2 as(
    select install_dt, count(*) retention
    from cte1 c, Activity a
    where c.player_id = a.player_id and a.event_date = c.install_dt+1
    group by install_dt
)


select cte1.install_dt , count(cte1.install_dt) installs, round(ifnull(retention,0)/count(cte1.install_dt),2)  Day1_retention
from cte1 left join cte2 using(install_dt)
group by install_dt
```

### *Leetcode 1132

``` 
# Write your MySQL query statement below
with cte as(
    select post_id, a.action_date, count(distinct r.post_id)/count(distinct a.post_id) per
    from Actions a left join Removals r using(post_id)
    where a.extra = 'spam'
    group by action_date
)
  select round(avg(per)*100,2) average_daily_percent
  from cte
```

## Null

### Leetcode 183

is null

``` 
# Write your MySQL query statement below
select a.name Customers
from  Customers a left join Orders b on a.id=b.customerId
where b.customerId is null;
```

### Leetcode 584

not equal: notice the null!!!!

``` 
select name
from Customer
where referee_id <> 2 or referee_id is null ;
```

## Rank

### Leetcode 586

the largest

``` 
# Write your MySQL query statement below
select customer_number
from Orders
group by customer_number 
order by count(*) desc
limit 1;
```

## Top N problem

### *Leetcode 185

``` 
select d.name Department, j.name Employee, j.Salary Salary
from 
    (select *, dense_rank() over (partition by departmentId order by salary desc) my_rank from Employee e) j
     left join department d on j.departmentId = d.id
where j.my_rank<= 3
```

### Leetcode 574

``` 
SELECT
    name AS 'Name'
FROM
    Candidate
        JOIN
    (SELECT
        Candidateid
    FROM
        Vote
    GROUP BY Candidateid
    ORDER BY COUNT(*) DESC
    LIMIT 1) AS winner
WHERE
    Candidate.id = winner.Candidateid
```

### Leetcode 1077

``` 
select total.proj project_id, total.empl employee_id
from ( 
    select p.project_id proj, p.employee_id empl, rank() over(partition by project_id order by e.experience_years desc) my_rank
    from Project p left join Employee e on p.employee_id = e.employee_id) total
where total.my_rank =1
```

### *Leetcode 1082

``` 
select seller_id
from sales
group by seller_id 
having sum(price) = (
    select sum(price) 
    from Sales 
    group by seller_id
    order by sum(price) desc
    limit 1)
```

### Leetcode 1341

``` 
select name results
from (
    select u.name, count(user_id) max_count
    from MovieRating m join Users u using (user_id)
    group by user_id 
    order by count(user_id) desc, name asc
    limit 1
) sub1
union 

select title 
from(
    select title, avg(rating)
    from MovieRating m join movies using(movie_id)
    where month(created_at)=2 and year(created_at)=2020
    group by movie_id
    order by avg(rating) desc, title asc
    limit 1
    )
    sub2 
```

### *Leetcode 1369

``` 
select username, activity, startDate, endDate
from (
    select *,
    count( activity) over(partition by username) cnt, 
    rank() over(partition by username order by enddate desc ) rnk
    from UserActivity
) sub1
where cnt = 1 or rnk =2
```

### Leetcode 1532

``` 
select c.name customer_name, c.customer_id customer_id, o.order_id order_id,o.order_date order_date 
from 
(select *,
count(order_id) over (partition by customer_id) cnt, 
row_number() over (partition by customer_id order by order_date desc) rnk
from Orders ) o join customers c using(customer_id)
where o.rnk <= 3 or o.cnt<3
order by customer_name asc, customer_id asc, order_date desc
```

### Leecode 1549

``` 
select product_name, product_id, order_id, order_date
from(
    select *, rank() over (partition by product_id order by order_date desc) num
    from orders o join Products p using(product_id)
    
) sub1
where num = 1
order by product_name asc,product_id asc,order_id asc

```

### *Leetcode 579

``` 
with ct1 as (
    select *, max(month) over(partition by id order by month desc) max_month
    from Employee
)

select a.id, a.month, a.salary + ifnull(b.salary,0) + ifnull(c.salary,0) salary
from ct1 a left join Employee b on a.month = b.month +1 and a.id =b.id
left join Employee c  on a.month = c.month+2 and a.id = c.id
where a.max_month <> a.month
```



## Screen the condition

### *Leetcode 1083

``` 
SELECT s.buyer_id
FROM Sales AS s INNER JOIN Product AS p
ON s.product_id = p.product_id
GROUP BY s.buyer_id
HAVING SUM(CASE WHEN p.product_name = 'S8' THEN 1 ELSE 0 END) > 0
AND SUM(CASE WHEN p.product_name = 'iPhone' THEN 1 ELSE 0 END) = 0;
```

### Leetcode 1084

``` 
select s.product_id, p.product_name
from sales s join product p using(product_id)
group by s.product_id 
having (min(sale_date)>='2019-01-01') and (max(sale_date)<='2019-03-31')
```

## Limit 

### *Leetcode 176

``` 
select ifnull(
    (select distinct salary 
    from Employee
    order by salary desc
    limit 1 offset 1), null)
SecondHighestSalary
```

### *Leetcode 177

``` 
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    SET N = N - 1;
  RETURN (
      # Write your MySQL query statement below.
    select distinct salary 
    from Employee
    order by salary desc
    limit 1 offset N 

  );
END
```

## Mid number

### *Leetcode 569

``` 
with cte as(
    select *,row_number ()over (partition by company order by salary asc) row_id,
        count(id) over(partition by company) as cnt
    from employee
)
select id,company, salary
from cte
where row_id between cnt/2 and cnt/2+1
```

### *Leetcode 571

``` 
# Write your MySQL query statement below
with cte as (
    select *,sum(frequency) over (order by num asc) sm, sum(frequency) over()  total
    from numbers
)
select round(avg(num),1) median
from cte
where total/2 between sm-frequency and sm
```

## Concecutive

### *Leetcode 601

``` 
with t as ( select t1.id , t1.visit_date , t1.people , 
            id - row_number() OVER(ORDER BY id) as grp
       from stadium t1
       where people >= 100 
          )
select t.id, t.visit_date ,t.people
from t 
where grp in ( select grp from t group by grp having count(*) >=3  )
```

``` 
#attention: don't forget the last one to join!!!! and order it
with ct1 as(select * from Stadium where people>=100)
select a.*
from ct1 a join ct1 b on a.id = b.id-1
join ct1 c on a.id = c.id-2
union 
select a.*
from ct1 a join ct1 b on a.id = b.id+1
join ct1 c on a.id = c.id+2
union 
select a.*
from ct1 a join ct1 b on a.id = b.id+1
join ct1 c on a.id = c.id-1

order by id
```

## IN

### Leetcode 585

``` 
select round(sum(tiv_2016 ),2) tiv_2016
from Insurance
where tiv_2015 in (
    select tiv_2015
    from Insurance
    group by tiv_2015 having count(pid) >1
)
and (lat,lon) in 
(
    select lat, lon
    from Insurance
    group by concat(lat,lon) having count(pid) = 1
)
```

### Leetcode 607

``` 
select name
from SalesPerson 
where sales_id not in 
(select o.sales_id
from orders o join company c using(com_id)
where c.name = 'RED')
```

### *Leetcode 1112

``` 
select student_id, min(course_id) course_id, grade
from Enrollments
where (student_id, grade) in
(select student_id, max(grade) grade
from Enrollments
group by student_id)
group by student_id
order by student_id asc
```

### Leetcode 1098

``` 
select book_id, name
from Books
where book_id not in (select book_id
    from Orders
    where dispatch_date between '2018-06-23' and '2019-06-23'
    group by book_id having sum(quantity)>=10)
and available_from<'2019-05-23'
```

