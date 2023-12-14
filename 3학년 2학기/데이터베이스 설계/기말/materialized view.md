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
	- deferred view maintenance 

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
	- projection 연산은 중복을 제거하기 때문에 **keep the count**를 해야한다. -> metadata로써 
	- R= (A, B), and r(R) = { (a,2), (a,3)}
	- TTA(r) has a single tuple (a)
	- a는 (a,2), (a,3) 두개의 튜플과 매핑되어 있으므로 a가 매핑되어 있는 튜플의 개수를 계속 추적해야한다.

- [[aggregation]] operation 
	- v = dept.name **G** **count** (ID)(Instructor)
	  여기의 count는 [[aggregation]] 연산으로써의 count 
	  (not metadata)
		- insert시 
			- 기존에 그룹 존재 : 1 증가
			- 기존에 그룹 미존재 : 새로운 그룹 생성 후 1 증가
		- delete 시
			- 기존에 그룹의 [[aggregation]] 값 1 초과 : 1 감소
			- 기존에 그룹의 [[aggregation]] 값 1 이하 : 그룹 삭제 ( [[aggregation]] 값이 0이면 그룹 삭제해야함)
	- v = dept.name **G** **sum** (ID)(Instructor)
		- **keep the count** 필요 -> metadata로써
		- sum이 0이 되더라도 그룹을 삭제할 수 없다
	- v = dept.name **G** **avg** (ID)(Instructor)
		- sum과 count를 유지 후 마지막에 나눠서 구한다
	- v = dept.name **G** **min/max** (ID)(Instructor)
		- 기존의 min/max값을 갱신하는 tuple insert시 해당 tuple의 값으로 update
		- 기존의 min/max값이 갱신되는 tuple delete시 새로운 min/max를 찾아야 하는 부담이 생긴다

- other operation 
	- v = r n s
		- insert시 : r에 튜플 insert시, s에도 존재하는지 체크 후  존재하면 v에 add
		- delete시 : r에서 튜플 delete시 체크과정 없이 v에서 delete 
		  -> table은 중복 데이터 없으므로 한 튜플 delete시 체크없이 바로 교집합에서 제거
		  

- materialized view selection
	- materialized view selection과 index selection은 근본적으로 같은 질문이다
	- view와 index는 maintenance cost가 존재하므로, 중구난방으로 만들 수는 없다.  -> workload에 따라 view나 index를 생성해야한다
	- **workload** : DB의 사용 패턴. 어떤 쿼리가 자주 등장하고, 어떤 튜플이 자주 업데이트되는지.
	- 데모 시나리오의 성능 최적화 고민시 추측에 의한 workload, 혹은 주력으로 두고싶은 서비스에 집중한 workload를 선택할 수 있다

summary
transformation : relation algebra형태로 변환할 때, 하나의 sql이 여러 RA로 번역될 수 있는데(equivalence rule), join되는 tuple의 개수를 따지는 등 가장 최적의 조합을 고려하여 결정해야 하며 [[evaluation]] plan은 cost-based, heuristics등이 있고 materialized view는 