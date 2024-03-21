---
title: Postgres - Grouping data by date
description: Useful postgres snippets for grouping data by date
date: 2016-03-27
draft: false
toc: false
tags:
  - postgres
---

### Easy ways to group counts of data by days, weeks, months or years.

~~~ sql
-- Group by Day of the week
>> select count(*) bugs_count,to_char(created_on, 'DAY') as day_of_week from BUGS group by day_of_week order by bugs_count desc;
 bugs_count     | day_of_week
----------------+-----------
              8 | THURSDAY
              5 | MONDAY
              4 | WEDNESDAY
              4 | SATURDAY
              3 | FRIDAY
              2 | TUESDAY
~~~

~~~ sql
-- Group by Day of the month
>> select count(*) as bugs_count,to_char(created_on, 'DD') as day_of_month from BUGS group by day_of_month order by bugs_count desc;
 bugs_count     | day_of_month
----------------+--------------
              5 | 16
              3 | 03
              3 | 06
              2 | 17
              2 | 07
              2 | 01
              2 | 10
              2 | 08
              1 | 31
              1 | 28
              1 | 04
              1 | 19
              1 | 22
~~~

~~~ sql
-- Group by week
>> select count(*) as bugs_count,to_char(created_on, 'YYYY-WW') as year_week from BUGS group by year_week order by year_week;
 bugs_count     | year_week
----------------+-----------
              3 | 2015-29
              4 | 2015-32
              1 | 2015-43
              2 | 2015-44
              1 | 2015-48
              5 | 2016-06
              2 | 2016-08
              4 | 2016-09
              4 | 2016-11
~~~

~~~ sql
-- group by month+year
>> select count(*) as bugs_count,to_char(created_on, 'YYYY-MM') as year_month from BUGS group by year_month order by year_month;
 bugs_count     | year_month
----------------+------------
              3 | 2015-07
              4 | 2015-08
              2 | 2015-10
              1 | 2015-11
              1 | 2015-12
              7 | 2016-02
              8 | 2016-03
~~~

~~~ sql
-- group by year
>> select count(*) as bugs_count,to_char(created_on, 'YYYY') as year from BUGS group by year order by year;
 bugs_count     | year
----------------+------
             11 | 2015
             15 | 2016
~~~
