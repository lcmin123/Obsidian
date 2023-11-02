- 모든 [[functional dependency]] (a -> b)가 f에 존재할 때, 다음중 하나를 만족해야함.
1. a -> b 가 자명한 경우
2. a가 R의 수퍼키
3. 모든 비자명한 함수 종속에서 결정자가 key인 경우
 
 - BCNF아닌 예
inst_dept(ID, name, salary, dept_name,building, budget)
dept_name->building, budget 일때,
dept_name이 수퍼키가 아니므로 bcnf가 아니다

- bcnf로 decomping 하기
	- 비자명한 의존관계 a -> b가 bcnf를 위반한다면 R을 
	   a U b 와 (R - ( b - a))로 decomping 한다
	- 위의 예
		- R1 (dept_name, building, budget)과 
		  R2 (Id, name, salary, dept_name)으로 나눈다면
		  R1 : a가 R1의 수퍼키이므로 [[BCNF]] 
		  R2 : 비자명한 함수 종속에서 ID가 Key이므로 [[BCNF]] 