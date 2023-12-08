- **Transaction** 
	- <font color="#00b0f0">단일 논리적 작업을 형성하는 연산들의 모음</font>
	- 송금등의 행위는 atomicity하게 행해져야함 (avoids inconsistency)
	- failure에 대해서는 <u>all or nothing</u>을 적용해야함 
	  -> 양쪽 모두에 행해지지 않으면 아예 행하지 말아야함
	- insert, delete, update 등 테이블을 수정하는 행위는 transaction을 잘 관리해야 함

- ACID
	- **Atomicity(원자성)**
		-  <u>DBMS의 책임</u> → <font color="#00b0f0">recovery system</font>
		- 트랜잭션의 작업들이 모두 수행되거나 전혀 수행되지 않아야함. 일부만 수행된 상태가 되어서는 안됨
		- 부분적으로 업데이트된 데이터가 DB에 반영되지 않도록 all or nothing을 원자적으로 수행
	- **Consistency(일관성)**
		- <u>개발자의 책임</u>
		- 트랜잭션의 수행 이후에도 데이터는 항상 일관되고 무결성이 유지된 상태에 있어야함 (integrity 보장)
		- explicit
			- PK, FK등의 지정으로 constraint 지정
		- implicit
			- 예를 들어, 송금할 때 <u>송금자의 잔액과 수금자의 잔액의 합은 늘 일정</u>해야한다.
			- 좀더 논리적인 개념이다
		- DB의 statement를 변화시킬 수 있는 것은 transaction 뿐이다. 
		- 초기 statement는 문제가 없어야 함
		- transaction 중 문제가 생긴다면 기존 statement로 회귀한다
		- transaction 성공 시, DB는 consistent 한 상황이다
		  ->**erroneous transaction logic** 작성시 inconsistency 가 발생하고, 이건 DBMS의 책임이 아닌 개발자의 책임이다.
	- **Isolation(고립성)**
		- <u>DBMS의 책임</u> → <font color="#00b0f0">concurrency control system</font>
		- 각 트랜잭션은 다른 트랜잭션의 수행에 영향을 끼치지않아야 함
		- **Serializability: 각 트랜잭션을 따로 수행한 결과와 동일하여야 함(트랜잭션 간의 순서는 무관함)**
		- 각 tran은 격리되어 수행되어야 한다[[Attached Files/b4500ab8ff9f281743de2b16b3af41be_MD5.jpeg|]]
![[Attached Files/b4500ab8ff9f281743de2b16b3af41be_MD5.jpeg]]
			- Q)T1 -> T2 가 올바른가 T2 -> T1이 올바른가?
			  A) 둘다 올바르다
			- 만약 꼭 송금후에 잔액이 출력되는게 올바르다! 라고 주장하고 싶으면 T1 -> T2의 순서만을 고집하는게 아닌 T1의 뒤에 T2를 붙여 하나의 transaction으로 만들어야한다.
			  -> transaction은 isolation하기 때문에 순서는 상관없어야한다
		- **Durability(지속성)**
			- <u>DBMS의 책임</u> → <font color="#00b0f0">recovery system</font>
			- 한번 Commit된 트랜잭션의 결과는 계속적으로 유지되어야 함
			- transaction이 성공적으로 끝마치면, failure가 일어나더라도 그 결과는 영속적으로 DB에 유지되어야 한다

- System Crash로부터의 안전성
	- Atomicity 
		- 문제점 : 재고는 줄어 들었는데 판매 내역은 없다
		- All or nothing : 판매기록도 있고 재고도 줄던지, 판매기록도 없고 재고도 그대로던지
	- Durability 
		- 문제점 : 정상적 구매완료 됐는데, 정전으로 구매 안된것으로 나옴
		- 지속성 : 한번 완료된 트랜잭션은 영원히 완료된 것으로 있어야 함

- sql에서의 transaction 
	- sql문의 시작부터 commit의 호출까지가 transaction의 한 호흡
	- rollbak 호출 시 트랜잭션 취소
	- autocommit 모드일 경우 sql 문장 하나 단위로 transaction 
		- 한 호흡 단위로 실행 불가. 보통 꺼놓는다
		- transaction은 순서가 상관 없으므로, sql문 단위로 transaction을 생성한다면 sql문의 순서가 뒤죽박죽이 되게 된다.

- oracle에서의 transaction 호흡
	- commit, rollback을 만날때 까지
	- DDL command를 만날때 까지(CREATE, DROP, RENAME) -> DDL의 전후에 각각 commit이 존재하기 때문
	- <font color="#00b0f0">Work는 가독성 향상 목적으로 사용, 보통 생략</font>

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
	- commit 후 undo하는 유일한 방법은 반대되는 transaction인 <u>compensating transaction</u> 실행 뿐
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
	- <font color="#00b0f0">schedule</font>
		- 동시에 실행되는 트랙잭션의 명령어들이 실행되는 시간적인 순서를 지정하는 일련의 명령어들
			- 트랜잭션의 모든 명령어들로 구성되어야 함
			- 개별적인 transaction의 순서를 유지해야 함
		- 성공한 트랜잭션은 마지막 문장으로 <u>commit</u> 명령어를 실행
		- 실패한 트랜잭션은 마지막 문장으로 <u>abort</u> 명령어를 실행
		- serial schedule : t1 -> t2 == t2 -> t1의 결과
		  ![[Pasted image 20231109153143.png|200]]
		column은 transaction
		가로줄은 시간 순서 
		- unserial schedule : 결과는 위 스케줄과 같다 ![[Pasted image 20231208203903.png|200]]
		- 두 스케줄 모두 sum(A+B) preserved
		- 이 스케줄은 consistency 가 지켜지지 않았다.
		  <u>read와 write의 위치에 유의</u>![[Pasted image 20231208204026.png|200]]

- serializability 
	- <u>기본 전제</u> : 각 트랜잭션은 DB Consistency를 유지한다
	  → 트랜잭션 자체에는 오류가 없어야 함
		- atomic하게만 state를 유지해주면 DBMS는 항상 consistent state -> consistent state로 transaction한다.
		- transaction의 오류는 전적으로 개발자 책임
	- 어떤 스케줄이 <font color="#00b0f0">serializable</font>하다는 것은 <u>그 스케줄이 serial schedule과 equivalent하다</u>는 것이다
	- 스케줄 동등성의 다른 형태
		- **Conflict Serializability (충돌 직렬화)**
		- View Serializability (뷰 직렬화)
		  → 계산 복잡성이 높아 실무에서는 사용 x

- Serializable (직렬 가능)
	- T1과 T2가 있을 때 트랜잭션의 올바른 수행 결과는?
	- 트랜잭션을 하나씩 차례로 수행했을 때 나올 수 있는 결과는 모두 올바른것임 (아무거나 선택) → <font color="#00b0f0">모두 올바른 것이라고 가정할 수 밖에 없음!</font>
	- 꼽으면 T1→T2, T2→T1 등으로 하나의 트랜잭션화 시켜야함

- 충돌하는 명령어 
	- 두개의 명령어 l<sub>i</sub>와 l<sub>j</sub>가 각각 트랜잭션 T<sub>i</sub>와 T<sub>j</sub>에 속한다고 가정할 때, 두 명령어가 충돌하는 경우는 l<sub>i</sub> = read, l<sub>j</sub> = read인 경우를 제외하고 모두이다
	  → 둘중 하나라도 write면 conflict 
	- 충돌하는 두 명령어 사이에는 (논리적인) 시간 순서가 강제
	- 즉, l<sub>i</sub>와 l<sub>j</sub>가 스케줄에서 연속적으로 나타나고 충돌하지 않는 경우, 스케줄에서 위치를 바꿔도 결과는 동일하게 유지![[Pasted image 20231208205237.png]]
	  → write(A)와 read(B)는 상관 없으므로 충돌x, 교체가능

- **Conflict Serializability (충돌 직렬성)**
	- 일정 S가 비충돌 명령어의 교환으로 일정 S’로 변환될 수 있다면, S와 S’는 **conflict equivalent(충돌 등가)** 이다
	- 일정 S가 <font color="#00b0f0">Serial schedule</font> 과 충돌 등가 하다면, S는 **충돌 직렬성**을 가진다고 한다
	- conflict serializable의 예![[Pasted image 20231208210048.png]]
	- not conflict serializable 의 예![[Pasted image 20231208234205.png]]
	- 결과는 같지만, 충돌등가하지 않은 예![[Pasted image 20231208234305.png]]

- 선행 그래프
	- 직렬 가능성의 테스트
		- <font color="#00b0f0">선형 그래프</font>는 트랜잭션이 있는 직접 그래프
		- 두 트랜잭션 충돌시 먼저 액세스한 T → 나중에 액세스한 T로 화살표 그림
		- ![[Pasted image 20231209010308.png]]<font color="#00b0f0"> 그래프가 순환된다면 스케줄은 충돌 직렬성이 없다</font>

- 충돌 직렬화 가능성
	- <font color="#00b0f0">그래프가 비순환적일 경우에만 충돌직렬 가능</font>
	- 선형 그래프 순환 여부 파악 위해 순환 감지 알고리즘 사용
	- 알고리즘은 정점 수에 비례하는 시간 소요
	- 선형그래프가 비순환적이라면, <font color="#00b0f0">위상 정렬</font>을 통해 직렬화 순서 얻을 수 있음
		- 스케줄 a 의 직렬화 순서는 b와 c 같이 그래프 부분 순서와 일치하는 선형 순서가 될 수 있음![[Pasted image 20231209010735.png|200]]

- **Recoverable Schedule**
	- T<sub>j</sub>가 이전에 T<sub>i</sub>가 작성한 데이터 항목을 읽는 경우, T<sub>i</sub>의 커밋 작업이 T<sub>j</sub>의 커밋 작업보다 먼저 나타나야 한다는 것을 의미
	- 아래 스케줄은 T<sub>9</sub>가 read(A)작업 후에 즉시 커밋했으므로 복구 가능하지 않음![[Pasted image 20231209011038.png|300]]
		- T<sub>8</sub>이 중단되어야 하는 경우, T<sub>9</sub>는 일관성이 보장되지 않는 데이터베이스 상태를 읽었을 수 있으므로, 데이터베이스는 일정이 복구 가능하도록 보장해야함

- <font color="#00b0f0">Cascading Rollback</font>
	- 단일 트랜잭션 실패가 연속적인 트랜잭션 롤백을 일으키는 것
	- 만일 T10이 실패한다면, T11과 T12도 롤백되어야 하고, 이는 상당량의 작업을 되돌릴 수 있음![[Pasted image 20231209011412.png]]

- <font color="#00b0f0">Cascadeless Schedule</font> 
	- T<sub>j</sub>가 T<sub>i</sub>가 이전에 작성한 데이터 항목을 읽을 때, T<sub>i</sub>의 커밋이 T<sub>j</sub>의 읽기보다 먼저 나타야함을 의미
	- 모든 Cascadeless 스케줄은 복구 가능 
	  → 모든 스케줄을 Cascadeless 하게 제한하는게 바람직

- 약한 수준의 일관성
	- 일부 응용 프로그램은 직렬화 되지 않은 스케줄을 허용하는 약한 일관성 수준을 허용함
		- 모든 계정의 근사 총 잔액을 얻고자 하는 읽기 전용 [[트랜잭션]] 
		- 쿼리 최적화를 위해 계산된 데이터베이스 통계
		- 위와 같은 트랜잭션은 다른 트랙션들과 같이 직렬화될 필요 없음
	- 성능을 위한 정확성의 희생
	- Sql에서의 일관성 수준
		- Serializable : 디폴트
		- Repetable read : 커밋된 레코드만 읽기 가능. 도

- Isolation level
	- 높은 수준의 isolation level일수록 <u>concurrency</u>는 떨어지지만(성능) 더 높은 수준의 <u>consistency</u>를 보장한다(정확성)