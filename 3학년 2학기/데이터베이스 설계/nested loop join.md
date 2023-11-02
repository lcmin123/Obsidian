cartesian product와 같은 역할

**for each**tuple tr**in** r**do begin
	for each tuple** ts**in** s**do begin**
		test pair (tr , ts) to see if they satisfy the join condition 
		if they do, add tr• tsto the result.
	**end
end**

r : outer [[join]] 
s : inner join

- 인덱스 필요없음
- 비용 높음 - > nr x bs + br ( block transfer ) + (nr + br)