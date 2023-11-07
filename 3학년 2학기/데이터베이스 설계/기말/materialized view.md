**create view** department_total_salary(dept_name, total_salary) as
**select** dept_name, sum(salary)
**from** instructor
**group** by dept_name

group by는 sorting이 들어갈 수 밖에 없음. -> 오버헤드
그래서 매번 group by 하지 않고, view 자체를 하나의 테이블로 작성해버리는 것이 바로 [[materialized view]]

- materialized  view maintenance
	- incremental view maintenance : 테이블 update시 즉시 materialized view도 update (immediate)
		- update 시 delete 후 insert하는 방식으로 구현 : differential
		- delete 되는 tuple : d<sub>r</sub>
		- insert 되는 tuple : i<sub>r</sub>
	- deferreed view maintenance 

- [[join operation]] 
	- r에 새로운 tuple이 insert 된 상황 가정
	- r<sup>new</sup> = r<sup>old</sup> U i<sub>r</sub> 
	  -> 기존 테이블과 새로운 튜플의 합집합
	- [[Attached Files/8ea9a5c7d7732f61759453b1d7f16963_MD5.jpeg|Open: Pasted image 20231107152045.png]]
![[Attached Files/8ea9a5c7d7732f61759453b1d7f16963_MD5.jpeg]]
	- r<sup>new</sup>과 s의 join 연산을 다시 계산할 필요 없이, 새로운 튜플만 가지고 join해서 연산을 수행하면 된다

- [[selection operation]] 
	- [[Attached Files/03d4897ab199789a6ef33ef66b1cfe69_MD5.jpeg|Open: Pasted image 20231107152317.png]]
![[Attached Files/03d4897ab199789a6ef33ef66b1cfe69_MD5.jpeg]]

- projection operation 
	- 
