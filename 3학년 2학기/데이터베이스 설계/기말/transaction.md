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
	- DBMS는 DML 실행 전에 항상 log를 남긴다
	- log만 제대로 남아있다면 failure가 생겨도 rollback 가능
	- ACID의 A와 D는 stability에 전부 달려있다해도 과언x

- RAID(Redundant Array of Inexpensive Disks)
	- 비싸지 않은 여러 개의 디스크를 배열하여 속도, 안정성, 효율성, 가용성 등의 증대를 위해 사용
	- RAID 0 : 성능
		- 모든 드라이브에 디스크 스트라이핑을 제공
		- 데이터 중복성은 제공하지 않지만 최적의 성능을 제공
	- RAID 1 : 데이터 보호
		- mirroring mode -> 용량을 절반으로 이용
		- 절반은 데이터 보관에 이용, 절반은 사본 복제에 이용
	- **RAID 5 : 데이터 보호 및 속도
		- 드라이브가 3개 이상인 시스템에서 권장
		- 모든 드라이브에 걸쳐 데이터를 스트라이핑해 성능을 높임
		- 각 드라이브의 일부를 내고장성에 할애(parity), 나머지 부분은 데이터 저장공간으로 남겨둠으로 데이터 보호 성능 극대화
		- parity block은 xor 연산을 수행
	- RAID 10 : 높은 안정성 및 성능
		- 드라이브 4개 이상에서 권장
		- 드라이브 2개, 2개를 각각 RAID1로 묶고, 그 묶음 2개를 RAID0로 묶는다
		- 세그먼트를 스트라이핑하여 매우 높은 입출력 속도
		- 용량은 절반
		- 최대 성능 및 높은 내결함성이 중요한 업무 (예: **데이터 베이스 관리 솔루션**)에 사용

- transaction abort
	- abort가 날 시 transaction이 일으킨 모든 변화는 undo후 rollback
	- commit 후에는 abort되었어도 undo할 수 없다
	- commit 후 undo하는 유일한 방법은 반대되는 transaction인 compensating transaction 실행 뿐
	- compensating transaction의 실행 책임은 사용자에게 

- transaction state
	- Active
	- Partially committed
	- Failed
	- Aborted
	- Committed

- **concurrent execution
	- 여러 transaction이 concurrent 하게 시스템에서 run될시 장점은
		- 프로세서와 디스크 활용빈도가 높아져 throughput 상승
		- 평균 응답 시간 감소
	- **concurrency control schemes 
		- ACID의 I기능 담당
		- concurrent 한 transaction 사이의 consistency 깨지지 않게 manage 
	- schedule
		- concurrent transaction의 시간적인 순서를 결정하는 정보의 연
		- 개별적인 transaction의 순서를 유지해야함
		- serial schedule 
		  ![[Pasted image 20231109153143.png]]
		column은 transaction
		가로줄은 시간 순서 
		- 
