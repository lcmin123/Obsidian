stable storage
	- 정보가 절대 사라지지 않는 스토리지(이론상)
	- data를 여러 비휘발성 스토리지에 2중 3중으로 백업
	- DBMS는 DML 실행 전에 항상 log를 남긴다
	- log만 제대로 남아있다면 failure가 생겨도 rollback 가능
	- ACID의 A와 D는 stability에 전부 달려있다해도 과언x