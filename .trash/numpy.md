- import numpy as np
- np.zeros(5)
	- array([0,0,0,0,0])
- np.zeros((3,4)) 
	- array([[0,0,0,0],
	  [0,0,0,0],
	  [0,0,0,0]])
- a.ndim
	- 차원 수 리턴
	- len(a.shape)와 같다
- a.size
	- 행 x 열 리턴
- a.shape
	- (3,4)
- np.ones(3,4)
	- 1로 채워진 3행 4열 배열 
- np.full((3,4),np.pi)
	- pi로 채워진 3행 4열 배열
- np.empty(2,3)
	- 초기화 되지 않은(garbage 값) 2행 3열 배열
- np.array([[1,2,3],[10,20,30]])
	- 1행 123, 2행 102030인 배열 생성
- np.arrange(1,5)
	- array([1,2,3,4])
- np.arrange(1,3,0.5)
	- array(1,1.5,2,2.5)
- np.linspace(0,1.5,4)
	- [0, 0.5, 1, 1.5]
	- 0부터 1.5포함 4등분
- np.random.rand(3,4)
	- 0~1 난수로 채워진 3행 4열 배열
- np.random.randn(3,4)
	- 평균0, 분산1 정규분포 난수로 채워진 3행 4열
- def my_function(z,y,x):
	  return x + 10 * y + 100 * z
	  
  np.fromfunction(my_function,(3,2,10))
	- 2행 10열 3개 3차원 배열 생성. 값은 my_func
- a.dtype
	- 배열 자료형 반환
	- int64, float64, complex64(복소수)
- type(np.zeros((3,4)))
	- numpy.ndarray
- a.itemsize
	- 할당 메모리 공간
- a.data
	- 할당 메모리 주소
- a.shape = (6,4)
	- 6행 4열의 배열로 reshape
	- 원본이랑 원소 개수 같아야 함
	- 별개의 메모리 공간을 사용하는 배열
- a.reshape(4,6)
	- 기존 array에 대해 다른 view를 리턴
	- **원본과 같은 메모리 공간 사용**
- a[1,2] = 999
	- a의 1행 2열 값 업데이트
- a.ravel()
	- a의 값을 갖는 1-dimensional view 생성
- **broadcasting**
	- 두 피연산자 배열의 크기가 다를 때 element wise를 위해 shape을 일치 시키려는 시도
	- rank가 다를 때
		- 차원에 1을 붙여서 차원수 맞춘다
		- np.arange(5).reshape(1,1,5)
	- rank는 같지만 shape이 다를 때
		- 알잘딱
	- rank와 shape이 모두 다를때
		- 오류
- upcasting
	- unit8 + 소수 = float64 형변환(dtype)
- conditional operator
	- [1,2,3] < [0,0,5]
	  array([False, False, True])
- a.mean()
	- 배열 내 평균값 리턴
- 3차원 배열일때
	- a.sum(axis = 0)
		- z축 방향으로 sum 수행
	- a.sum(axis = 1)
		- y축 방향으로 sum 수행
	- a.sum(axis=(0,2))
		- x,z축 방향으로 sum
- np.square(a)
	- 배열내 원소들 제곱해서 각각 리턴
- slice
	- a[2:5]
		- 2번째부터 5번째 전까지 리턴
	- a[2:-1]
		- 2부터 마지막원소 -1 전까지 리턴
	- a[:2] = a[0:2]
	- a[2 : : 2]
		- 2번째 원소부터 2스텝으로 리턴
	- a[: : -1]
		- 배열 리버스
	- 슬라이스후 원본 값 업데이트 가능, 삭제(del) 불가능
	- 원본 업데이트해도 슬라이스 반영
	- slice에 copy() 할당시 원본 반영 안됨
	- 크기 다르면 업데이트 불가능
	- a_slice = a[2:6]
	  a_slice[1] = a[3]
- a[1, :]
	- 1행 리턴
- a[:,1]
	- 1열 리턴
- a[(0,2), 2:5]
	- 0행, 2행 2~4원소
- a[:, (-1,2)]
	- 모든 행, -1열과 2열
- a[(1,2),(1,2)]
	- 1행1열, 2행2열 원소 리턴
- a[1, …]
	- 3차원이면 1차원의 모든행열 리턴
- a[1,2,…]
	- 1차원 2열의 모든 행 리턴
- boolean indexing
	- 