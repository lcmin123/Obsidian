- RFC 4949
보안정책에 따라 시스템 자원 사용을 규제하고,
인가된 존재(사용자, 프로그램, 다른 시스템)에게 사용을 허가. 정리하면,
	- 허가되지 않은 사용자의 자원 접근 막고
	- 정당한 사용자라도 허가되지 않은 방법으로 자원사용 막고
	- 정당한 사용자가 허가된 방식으로만 자원에 접근하도록

![[Pasted image 20231115115720.png]]

- Access control components 
	- Authentication function
		- 사용자가 시스템에 접근할 수 있는지 결정
		- 로그인, 패턴, 지문 등등
	- Access control function
		- 사용자의 요청이 허용되는지 결정
	- Security admin (서버 관리자)
		- 사용자의 요청이 자원에 어떻게 접근하는지를 명시한 권한 DB를 관리
		- 권한DB란 어떤 사용자가, 어떤 객체에, 어떻게 접근할 수 있다는 권한(**접근 제어 정책**)을 적어놓는 DB
	- Audit (감사)
		- 사용자가 시스템 자원에 접근하는 것을 기록, 감시
		- 전체 과정을 log로 남겨놓고, 필요시 감사

- Access Control Policy (접근 제어 정책)
	- 어떤 상황에서, 누구에게 어떤 종류의 접근이 허용되는지를 결정
	- 예를 들어, 긴급전화는 패턴을 풀지 않고도 할 수 있다
	  → 상황에 따라 접근 허용 범위 달라짐
	- 권한 DB에 저장되며 보안 admin이 관리 (SA)
	- 종류
		- [[DAC]] (Discretionary Access Control)
		- MAC (Mandatory Access Control)
		- [[RBAC]] (Role-Based Access Control)
		- [[ABAC]] (Attribute-Based Access Control)
