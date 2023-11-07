[[Attached Files/a833c651e16446a680e84433c3a73726_MD5.jpeg|Open: Pasted image 20231102091833.png]]
![[Attached Files/a833c651e16446a680e84433c3a73726_MD5.jpeg]]위  테이블의 문제점
- branch name, city, assets 반복
- 업데이트를 복잡하게 만들며 asset의 일관성 문제 발생 가능
- loan이 존재하지 않을 경우 데이터를 저장할 방법이 없음
- null을 사용할 수는 있지만 다루기 어려움

발생하는 Anomaly
- insertion anomaly 
	- loan-number 없이 branch-name 등을 insert 불가
- deletion anomaly 
	- 어떤 branch의 마지막 loan을 delete하면 branch 자체가 사라짐
- update anomaly 
	- 어떤 특정한 branch의 정보(downtown 의 asset 등)을 update하면 해당 튜플을 모두 업데이트해야함 
- 원인
	- 정보의 중복
	- 여러 entity가 하나의 table에 합쳐짐
- 해결책
	- [[decomposition]]