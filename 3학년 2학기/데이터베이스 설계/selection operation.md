- A1 - linear search
	- cost = tT x br + ts
	- if selection on key = tT x (br/2) + ts
- A2 - primary index, equality on key
	- retrieve single record
	- cost = (hi + 1) x (tT + ts)
	  b+tree 높이 + 1 x 총 시간
- A3 - primary index, equality on nonkey
	- retrieve multiple record 
	- cost = hi x (tT + ts) + tT x b + ts
	  b+tree 높이 x 총시간 + 블록 전체 트랜스퍼시간 + 한번