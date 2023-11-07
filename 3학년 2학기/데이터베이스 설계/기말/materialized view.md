**create view** department_total_salary(dept_name, total_salary) as
**select** dept_name, sum(salary)
**from** instructor
**group** by dept_name

group by는 sorting이 들어갈 수 밖에 없음. -> 오버헤드
그래서 매번 group by 하지 않고, view 자체를 하나의 테이블로 작성해버리는 것이 바로 [[materialized view]]

- materialized  view maintenance
	- incremental view maintenance : 테이블 update시 즉시 materialized view도 update (immediate)
	- deferreed view maintenance 
- 
