- 디피 헬먼 키 교환 기반
- 이산대수의 어려움을 이용
  이산대수: A = B<sup>C</sup>, C 찾기
- [[RSA]] 보다 느림
- 대칭키 세션 키 교환, 세션 키 생성에 사용
- 메시지 암호화에는 대칭키 암호화를 함께 사용해야 함
- k를 임의로 선택하기 때문에 동일한 M에 대해 매번 다르게 암호화됨
- CipherText는 원래 M 길이의 2배가 됨

- 공개키 : y, p,q
- 비밀키 : x

### 키생성
1. 큰 소수 p 결정
2. p보다 작은 임의의 q, x 선택
3. y = q^x mod p

### 암호화 / 복호화
- 암호화 
<u>p-1</u>와 서로소인 임의의 k 결정
a = q<sup>k</sup>mod p
b = Y<sup>k</sup>M mod p

- 복호화
  b/a<sup>x</sup>mod p = b * a <sup>p-1-x</sup> mod p = M
