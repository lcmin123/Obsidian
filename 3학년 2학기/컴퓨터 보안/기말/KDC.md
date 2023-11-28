
유의 개념 : [[CA]]
- Key Distribution Centre
	- for symmetric key cryptography
	- how to determine a shared secret key
	- ![[Pasted image 20231128102843.png]]
	- 중앙의 신뢰할 수 있는 키관리, 분배 책임 주체
	- 사용자들은 KDC 등록 신청 하면, KDC가 각 사용자들과 비밀 키 공유
	- 장점
		- 빠르다. 대칭키를 효율적으로 관리 (**CA와의 차이점**)
	- 단점
		- 단일 실패 지점 (SPoF)
			- KDC가 고장나면 전혀 서비스 불가
			- 해결책 : mirror server 가동
			  →서버와 미러서버 간의 consistency 문제 발생 가능