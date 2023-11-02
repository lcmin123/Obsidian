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
- 비용 높음 
  - > <u>nr x bs + br</u> ( block transfer ) + <u>nr + br </u>(seek)
  - n은 튜플개수
  - **작은 관계가 인메모리 핏하다면, 그걸 이너 릴레이션으로 사용 -> bs + br transfer + 2 seek으로 비용 감소

예시
[[Attached Files/bcfadf0664c458034c5fa318d6572093_MD5.jpeg|Open: Pasted image 20231102142331.png]]
![[Attached Files/bcfadf0664c458034c5fa318d6572093_MD5.jpeg]]
1. student가 outer 
   