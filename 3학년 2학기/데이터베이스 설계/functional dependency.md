a -> b
- a determines b
- a값이 같은데 b값이 다를수는 없다
- relation의 모든 instance에서 규칙이 성립해야함
- 자명한 관계는 trivial

- 기능들
	- 오직 K만 R을 결정할때 K는 수퍼키
	- 오직 K만 R을 결정하고, K에 속하는 임의의 원소는 R을 결정하지 못할때 K는 캔디드키

- lossless [[decomposition]] 의 조건
	- R1, R2가 R의 [[decomposition]] 일때, 
	  **R1 n R2 -> R1 or R1 n R2 -> R2**
	  라면 , 즉 분해한 두개의 schema중 하나가 다른 하나의 key를 포함하면 lossless 
	- 하지만 의존성의 보존은 보장하지 않는다