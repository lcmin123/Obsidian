 - Number 하나의 타입이 양수, 음수, 정수, 소수 모두 담을 수 있다
	- 여러 연산 기호 사용 가능. 연산 순서는 PEMDAS
		  Parentheses (괄호) :()
		  Exponenet (지수) : **
		  Multiplication (곱하기) : *
		  Division (나누기) : /
		  Addition (더하기) : +
		  Substraction (빼기) : -
		  <Modular (나머지) : %>
	- NAN : 숫자가 아님을 나타내는 Number 

- 변수
	- `let 변수이름 = 변수값;`
		- 변수 이름을 불러올때 마다 변수 값 return
		- let A = B + C에서 B나 C를 업데이트해도 A의 값을 변하지 않는다
		- **변수는 본질적으로 연결된게 아니라 그 순간을 스냅샷으로 찍은 것**
	- `const 변수이름 = 변수값;`
		- 값을 변경할 수 없는 상수 변수 선언
		- array, object에서 유용하게 사용
	- `var 변수이름 - 변수값;`
		- legacy