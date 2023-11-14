`let stringName = "stringValue"`

- 문자열을 자릿수마다 숫자로 인덱싱 되어있음
- 문자열[숫자]로 접근 가능
- 문자열.length로 문자열 길이 return
- 문자열 + 문자열로 strcat 가능

- 메서드
	- 특정 문자열을 통해 수행할 수 있는 기능
	- 문자열.method()로 구현
		- toUpperCase() → msg를 대문자로 변환
		- trim() → 문자열 앞뒤의 공백 제거
		  문자열의 핵심만 뽑고 싶을 때
	- 연달아 수행 가능 → 문자열.toUpperCase().trim() …
	- 메서드 괄호 안에 파라메터 받을 수 있음
		- indexOf(args) → 문자열안의 args의 인덱스 return, 없으면 -1 return
		- 
	