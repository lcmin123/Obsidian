- var, let, const의 차이
	- var
		- 함수 레벨 스코프
		- 선언된 함수 내에서만 접근 가능
		- 재선언, 재할당 가능
		- 호이스팅 되어 선언전에 참조 가능
	- let
		- 블록 레벨 스코프
		- 선언된 블록 내에서만 접근 가능
		- 재선언 불가능, 재할당 가능
		- 선언전에 참조시 에러
	- const
		- 블록 레벨 스코프
		- 재선언, 재할당 불가
		- 한번 선언되면 값이 불변
		- 선언전에 참조시 에러
	- 즉, 변수 선언에는 기본적으로 const를 이용하고, 재할당이 필요한 경우에 한정해 let을 사용
- 반복문 (for in, for of)
	- for in
		- 객체의 모든 열거 가능한 속성에 대해 반복
		- 배열의 인덱스를 반환
	- for of
		- 객체의 요소에 대해 반복
		- 배열의 값을 반환
		- 배열과 같은 반복 가능한 객체에 대해 반복하는 데에 적합
- 배열의 딥카피와 쉘로우 카피
	- 딥카피
		- 배열의 실제 값을 새로운 메모리공간에 복사
		- 복사한 배열을 수정해도 원 배열은 불변
		- spread 연산자![[Pasted image 20240113180214.png]]
		- Object.assign()![[Pasted image 20240113180247.png]]
		- JSON.parse(), JSON.stringfy()![[Pasted image 20240113180318.png]]
	- 쉘로우 카피
		- 배열의 주소값만 복사
		- 복사한 배열 수정시 원 배열 수정![[Pasted image 20240113180357.png]]
- 람다함수와 콜백함수
	- 람다 함수
		- 익명 함수를 지칭
		- 함수를 한번만 사용하거나, 함수를 인자로 전달하는 경우 사용
		- 콜백 함수로 사용됨
	- 콜백 함수
		- 어떤 함수의 파라미터로써 함수를 넘기고, 어떤 행위나 태스크가 완료된 직후에 호출하는 함수
		- 비동기 작업 처리시 사용 : 스크립트 로딩이 끝나 후 실행될 함수를 콜백 함수로 지정
		- 익명 함수![[Pasted image 20240113180851.png]]
		- 화살표 함수![[Pasted image 20240113180905.png]]
- import와 require
	- js에서 외부 파일이나 라이브러리를 불러올 때 사용하는 모듈 키워드
	- require
		- node.js에서 사용되는 commonJS의 키워드
		- 프로그램의 어느지점에서나 호출 가능
		- 동기적으로 모듈 로드
	- import
		- ES2015에서 도입된 키워드
		- 파일의 시작부분에서만 실행 가능
		- 비동기 문법으로 파일 중간에 모듈 불러오기 가능
		- 사용자가 필요한 모듈 부분만 선택하고 로드할수 있어 선호됨
		- require보다 성능이 우수하며 메모리 절약