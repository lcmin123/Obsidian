dependancy preserve 되지 않은 BCNF를 사용하거나,
중복을 감안하고 3NF를 쓰려 할때
우리는 성능을 위해 non-normalized schema를 사용할 수 있다

1. 정규화 되지 않은 관계 사용
	1. 빠른 조회(+)
	2. 업데이트를 위한 추가 공간 및 시간(-)
	3. 추가 코딩 작업 오류 가능성(-)
2. materialized view 사용
	1. 단점은 동일
	2. 추가코딩 없음 (DBMS가 해줌)
	3. 가능한 오류 회피