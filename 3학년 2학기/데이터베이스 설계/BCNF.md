모든 [[functional dependency]] (a -> b)가 f에 존재할 때, 다음중 하나를 만족해야함.
1. a -> b 가 자명한 경우
2. a가 R의 수퍼키
3. 모든 비자명한 함수 종속에서 결정자가 key인 경우

BCNF아닌 예
inst_dept(ID, name, salary, dept_name,building, budget )