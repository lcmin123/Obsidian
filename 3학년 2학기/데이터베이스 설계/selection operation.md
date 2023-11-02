- A1 
	- linear search
	- cost = tT x br + ts
	- if selection on key = tT x (br/2) + ts
- A2 
	- primary index, equality on key
	- retrieve single record
	- cost = (hi + 1) x (tT + ts)
	  b+tree 높이 + 1 x 총 시간
- A3 
	- primary index, equality on nonkey
	- retrieve multiple record 
	- cost = hi x (tT + ts) + tT x b + ts
	  b+tree 높이 x 총시간 + 블록 전체 transfer시간 + 한번seek
- A4 
	- secondary index, equality 
	- retrieve single record if key is candidate
	- cost = (hi + 1) x(tT + ts) 
	  **== A2**
	- retrieve multiple record if key is not candidate 
	- cost = (hi + n) x (tT + ts) -> 트리높이 + 탐색 횟수 x 총시간 (비쌀수잇음)
- A5 
	- primary index, comparison
	- 크기 비교
	- cost = (hi ) x (tT + ts) + tT x b + ts
	  **== A3**
- A6 - secondary index, comparison 



	- cost = (hi + n) x (tT + ts)
	  **== A4**