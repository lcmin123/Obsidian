[[BCNF]] 보다 약한 노말 폼으로써, BCNF가 의존성을 유지하지 못하거나, 업데이트시 FD 위반이 확인 될 경우 쓰기 좋다

- **중복을 허용(-) :: [[anomaly]] 
- 개별 관계에서 FD 확인 가능
- 조인 연산 없이 FD 확인 가능
- **항상 lossless [[decomposition]] 
- **항상 dependency preserving

조건
- a -> b가 자명
- a가 R의 수퍼키
- (new) b - a의 모든 속성이 캔디드 키의 일부일 경우

예시
  dept_advisor (s_ID, i_ID, dept)
  F = {s_ID, dept -> i_ID,  i_ID -> dept
  two candidate keys: (s_ID, dept), (i_ID, s_ID)
  1. s_ID, dept -> i_iD 
     s_ID, dept가 수퍼키이므로 만족
2. i_ID -> dept
   b - a = dept가 candidate key의 일부이므로 만족
   (BCNF는 성립하지 않음)

다른 정의:
2NF에서 pk에 대해 연속 종속이 아닌 column들만 있을때