[[Attached Files/a833c651e16446a680e84433c3a73726_MD5.jpeg|Open: Pasted image 20231102091833.png]]
![[Attached Files/a833c651e16446a680e84433c3a73726_MD5.jpeg]]위  테이블의 문제점
- branch name, city, assets 반복
- 업데이트를 복잡하게 만들며 asset의 일관성 문제 발생 가능
- loan이 존재하지 않을 경우 데이터를 저장할 방법이 없음
- null을 사용할 수는 있지만 다루기 어려움

발생하는 Anomaly
- insertion anomaly