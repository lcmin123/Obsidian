공개키를 쓰려면 PKI가 필요

대칭키를 쓰려면 둘이 나눠 가져야하는 문제점
-> KDC를 만들어 운영해야하는데, PKI랑 큰 차이 X
--> 대칭키를 얼굴 안보고 나누는 방법 필요 : [[DiffieHellman]]

- 아이디어 
	- 임의의 큰 정수 p,q 를 Alice와 Bob에게 공개
	- Alice만 아는 값 : a, A = q<sup>a</sup>mod p 계산
	- Bob만 아는 값 : b, B = q<sup>b</sup>mod p 계산
	- 공개 값 4개 (p,q,A,B), 비밀 값 2개 (a,b)
	- Alice와 Bob은 K = q<sup>ab</sup>mod p 계산 -> 대칭키
	- 해커는 공개 값 (p, q, A, )