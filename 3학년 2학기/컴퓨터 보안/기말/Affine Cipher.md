- 두 정수 a, b 를 선택한 후 평문 M을 C = am + b mod 26으로 암호화 (단, a는 26과 서로소)
- a가 26과 서로소이므로 da = 1 mod 26인 d가 존재
- 암호문 c를 d(c-b) = d(am) = (da)m = m mod 26으로 [[복호화]] 
- 예) a = 1, b = 3이면 [[Shift Cipher]]