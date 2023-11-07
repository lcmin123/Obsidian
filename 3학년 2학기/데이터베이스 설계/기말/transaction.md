- 송금등의 행위는 atomicity하게 행해져야함 (avoids inconsistency)
	- failure에 대해서는 all or nothing을 적용해야함 
	  -> 양쪽 모두에 행해지지 않으면 아예 행하지 말아야함
	- insert, delete, update 등 테이블을 수정하는 행위는 transaction을 잘 관리해야 함

- ACID
	- Atomicity(원자성)
		-   트랜잭션의 작업들이 모두 수행되거나 전혀 수행되지 않아야함. 일부만 수행된 상태가 되어서는 안됨
		- 부분적으로 업데이트된 데이터가 DB에 반영되지 않도록 all or nothing을 원자적으로 수행
	- Consistency(일관성)
		- 트랜잭션의 수행 이후에도 데이터는 항상 일관되고무결성이 유지된 상태에 있어야함 (integrity 보장)
		- explicit
			- PK, FK등의 지정으로 constraint 지정
		- implicit
			- 예를 들어, 송금할 때 송금자의 잔액과 수금자의 잔액의 합은 늘 일정해야한다.
			- 좀더 논리적인 개념이다
		- DB의 statement를 변화시킬 수 있는 것은 transaction 뿐이다. 
		- 초기 statement는 문제가 없어야 함
		- transaction 중 문제가 생긴다면 기존 statement로 회귀한다
		- transaction 성공 시, DB는 consistent 한 상황이다
		  ->erroneous transaction logic 작성시 inconsistency 가 발생하고, 이건 DBMS의 책임이 아닌 개발자의 책임이다.
	- Isolation(고립성)
		- 각 트랜잭션은 다른 트랜잭션의 수행에 영향을 끼치지않아야 함
		- **Serializability: 각 트랜잭션을 따로 수행한 결과와 동일하여야 함(트랜잭션간의순서는무관함)**
		- 각 tran은 격리되어 수행되어야 한다[[Attached Files/b4500ab8ff9f281743de2b16b3af41be_MD5.jpeg|]]
![[Attached Files/b4500ab8ff9f281743de2b16b3af41be_MD5.jpeg]]
			- Q)T1 -> T2 가 올바른가 T2 -> T1이 올바른가?
			  A) 둘다 올바르다
			- 만약 꼭 송금후에 잔액이 출력되는게 올바르다! 라고 주장하고 싶으면 T1 -> T2의 순서만을 고집하는게 아닌 T1의 뒤에 T2를 붙여 하나의 transaction으로 만들어야한다.
			  -> transaction은 isolation하기 때문에 순서는 상관없어야한다.
			  - concurrency-control system가 담당
		- Durability(지속성)
			- 한번 Commit된 트랜잭션의 결과는 계속적으로 유지되어야 함
			- transaction이 성공적으로 끝마치면, failure가 일어나더라도 그 결과는 영속적으로 DB에 유지되어야 한다
			- recovery system가 담당

- sql에서의 transaction 
	- sql문의 시작부터 commit의 호출까지가 transaction의 한 호흡
	- rollbak 호출 시 트랜잭션 취소
	- autocommit 모드일 경우 sql 문장 하나 단위로 transaction 
		- 한 호흡 단위로 실행 불가. 보통 꺼놓는다
		- transaction은 순서가 상관 없으므로, sql문 단위로 transaction을 생성한다면 sql문의 순서가 뒤죽박죽이 되게 된다.

- oracle에서의 transaction 호흡
	- commit, rollback을 만날때 까지
	- DDL command를 만날때 까지(CREATE, DROP, RENAME) -> DDL의 전후에 각각 commit이 존재하기 때문

- stable storage
	- 정보가 절대 사라지지 않는 스토리지(이론상)
	- data를 여러 비휘발성 스토리지에 2중 3중으로 백업
	- 로그만 제대로 남아있다면 failure가 생겨도 rollback 가능
