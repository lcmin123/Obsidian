
- <font color="#00b0f0">Cascading Rollback</font>
	- 단일 트랜잭션 실패가 연속적인 트랜잭션 롤백을 일으키는 것
	- 만일 T10이 실패한다면, T11과 T12도 롤백되어야 하고, 이는 상당량의 작업을 되돌릴 수 있음![[Pasted image 20231209011412.png]]

- <font color="#00b0f0">Cascadeless Schedule</font> 
	- T<sub>j</sub>가 T<sub>i</sub>가 이전에 작성한 데이터 항목을 읽을 때, T<sub>i</sub>의 커밋이 T<sub>j</sub>의 읽기보다 먼저 나타야함을 의미
	- 모든 Cascadeless 스케줄은 복구 가능 
	  → 모든 스케줄을 Cascadeless 하게 제한하는게 바람직