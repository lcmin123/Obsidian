n = 주사위 갯수
k = 목표 숫자
temp = [0…n-1] 배열

dice (k,n,temp)
	if (k = 0 && n = 0)
		temp 출력
	else
		if(check(k,n))
			for( i = 1; i ≤ 6; i++)
				temp
check(k,n)
	if(k<n or k>n)
		return false
	return true