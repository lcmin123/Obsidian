- STL Vector
	- [[(Iterator)]]
	- 예제 [[Vector example]]
	- 메서드 
		- O(1)
			- push_back() : 벡터 뒤에 요소를 추가
			- pop_back() : 벡터 마지막 요소 제거
		- O(N)
			- push_front() : 벡터 처음에 요소 추가
			- pop_front() : 벡터 처음 요소 제거
		- 그 외
			- size() : 저장된 요소 개수 반환
			  **unsigned int 반환하므로 for 문에서 size()-1 금지**
			- empty() : 벡터가 비어있는지 여부 반환
			- clear() : 벡터 모든 요소 제거
			- resize() : 벡터 크기 변경
			- at(): 특정 인덱스 요소 접근
			- insert() : 특정 위치에 요소 삽입
			- erase() : 특정 위치나 범위 요소 제거
	- =를 사용하면 deep copy 발생
	- range-based for loop
	  ```c++
	  vector<int> v1 = {1,2,3};
	  for(int e : v1)
	  -> v1 모든 원소 순회
	  for(int& e : v1)
	  ->v1 모든 원소 순회 및 레퍼런스 : 
	  for 문 내에서 v1 수정시 원본 v1도 수정
	  ```
	  