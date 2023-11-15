모든 사용자에게 p=11, q=2가 공개되어 있고, Alice가9를 Bob이 4을 임의로 생성하였을 때 공통으로 가지게 되는 비밀키는 무엇이 되는지 보이시오.

1. alice는 9를 선택하고 x를 계산
   x = q^a mod p = 2^9 mod 11 
   = 6
2. bob은 4를 선택하고 y를 계산
   y = q^b mod p = 2^4 mod 11 
   = 5
3. alice는 k를 계산
   k = y^a mod p = 5^9 mod 11
   5 mod 11 = 5
   5^2 mod 11 = 3
   5^4 mod 11 = 9
   5^9 mod 11 = 405 mod 11 
   = 9
4. bob은 k를 계산
   k  = x^b mod p = 6^4 mod 11
   6 mod 11 = 6
   6^2 mod 11 = 3
   6^4 mod 11 
   = 9
   