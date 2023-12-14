- **Concurrency control**
	- 트랜잭션의 기본 속성중 하나인 <font color="#00b0f0">isolation</font>에 초점
	- 여러 트랜잭션이 동시에 실행될 때, 데이터베이스에서 격리 속성이 보장되지 않을 수 있음
	- 격리를 보장하기 위해 DBMS는 동시 실행되는 트랜잭션 간 상호작용을 제어해야 함
	  → <font color="#00b0f0">concurrency control schemes</font>
	- 자주 사용되는 방법은 <font color="#00b0f0">Two phase lock protocol과 snapshot isolation</font>
- Lock based protocol
	- **lock**은 데이터 항목에 대한 동시 액세스를 제어하는 매커니즘
	- 데이터는 <font color="#00b0f0">배타적모드(X)</font>와 <font color="#00b0f0">공유모드(S)</font>로 잠길 수 있음
	- 배타적 모드는 읽기, 쓰기가 모두 가능한 상태
	- 공유모드는 데이터를 읽기만 가능한 상태
	- 트랜잭션은 concurrency contol 관리자에게 <font color="#00b0f0">락 요청</font>을 하고, 요청이 <font color="#00b0f0">승인</font>된 후에만 트랜잭션 진행
- Lock-compability matrix
	- 항목에 대해 배타적lock을 보유할 경우 다른 항목에 대해 어떤 lock도 보유 불가
	  → 둘다 read only일 경우에만 더블 락 가능, read중 write 시도시 fail
	- lock을 부여할 수 없는 경우, 요청한 트랜잭션은 다른 lock이 모두 해제될 때 까지 대기
	- 예시![[Pasted image 20231214135417.png]]
		- read, write할때는 x락, read만 할땐 s락
	- 