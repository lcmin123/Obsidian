- 리액트는 virtual dom 개념을 사용해 변화가 있는 부분만 브라우저에 알려 dom을 업데이트 함
- virtual dom은 실제 dom의 복사본
- dom의 구조를 자바스크립트 객체로 나타낸 것
- 메모리상에만 존재
- 업데이트 전 상태, 업데이트 되어야 할 상탤 ㅗ존재
- 동작 과정
	- 데이터를 업데이트 하면 전체 UI를 virtual dom에 리렌더링
	- 이전 virtual dom에 있던 내용과 비교
	- 바뀐 부분만 dom에 적용
- 전체 페이지를 새로 불러오는것 보다 변경된 부분만 업데이트 하므로 퍼포먼스 적으로 뛰어남