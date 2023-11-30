- 네트워크 5계층 ![[Pasted image 20231130102629.png|300]]
- SSL
	- 웹기반 전자 상거래를 위해 TCP Layer에서 메시지의 무결성과 기밀성 제공
		- Server/Client Authentication
			- Server - RSA, X.509
			- Client - browser에서 제공
		- Confidentiality
			- DES, 3DES, IDEA, RC2, RC4
		- Integrity 
			- MD5
		- 부인방지 기능은 제공 안됨 → 전자서명 사용해야
	- Web-server와 Web-browser간 보안 (https)
		- 단순히 서버와 브라우저간의 모든 데이터를 암호화하는 것이 안전한건 아니다
		- Reply Attack 발생 가능
	- Protocol 구성
		- Handshake
			- 암호 spec, session 관리
			- Client, Server간 인증
			- 암호 알고리즘 합의
			- 키교환
		- record protocols
			- 암호, 무결성 등 보안 서비스
			- 데이터를 인증, 암호화처리하고 메시지 송수신
	- 인증 수행 → 세션키 분배 → 암호화 메시지 송수신 순서
- [[Handshake Protocol]]
- Record Protocol 
	- 단계 
	  1. 메시지를 최대 16KB 단위로 fragment
	  2. 압축(OPT)
	  3. MAC 추가
	  4. Encryption
	  5. SSL Record Header 추가
