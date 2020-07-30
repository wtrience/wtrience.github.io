---
layout:     post
title:      "C++ interview"
subtitle:   " \"coding everyday\""
date:       2019-10-28 12:00:00
author:     "Tian"
header-img: "img/post-bg-2015.jpg"
tags:
    - C++
---
> “Thinking”

    查找最晚入职员工的所有信息
    select * from employees order by hire_date desc limit 1;

    查找入职员工时间排名倒数第三的员工所有信息
    select * from employees order by hire_date desc limit 1 offset 2;

    查找各个部门当前(to_date='9999-01-01')领导当前薪水详情以及其对应部门编号dept_no
    select salaries.emp_no,salaries.salary,salaries.from_date,salaries.to_date,dept_manager.dept_no from salaries, dept_manager 
    where salaries.emp_no = dept_manager.emp_no
    and salaries.to_date = "9999-01-01"
    and dept_manager.to_date = "9999-01-01";

    查找所有已经分配部门的员工的last_name和first_name
    select es.last_name,es.first_name, dp.dept_no
    from employees as es,dept_emp as dp
    where es.emp_no = dp.emp_no


    查找所有员工的last_name和first_name以及对应部门编号dept_no，也包括展示没有分配具体部门的员工
    select employees.last_name,employees.first_name,dept_emp.dept_no
    from employees left join dept_emp
    on employees.emp_no=dept_emp.emp_no

    查找所有员工入职时候的薪水情况，给出emp_no以及salary， 并按照emp_no进行逆序
    select employees.emp_no,salaries.salary
    from employees,salaries
    where employees.emp_no=salaries.emp_no and employees.hire_date=salaries.from_date
    order by employees.emp_no desc

    查找薪水涨幅超过15次的员工号emp_no以及其对应的涨幅次数t
    select emp_no,count(emp_no) as t
    from salaries group by emp_no having count(emp_no)>15

    
