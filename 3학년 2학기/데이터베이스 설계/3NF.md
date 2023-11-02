[[BCNF]] 보다 약한 노말 폼으로써, BCNF가 의존성을 유지하지 목하거나, 업데이트시 FD 위반이 확인 될 경우 쓰기 좋다

- 중복을 허용(-)
- 개별 관계에서 FD 확인 가능
- 조인 연산 없이 FD 확인 가능
- 항상 lossless
- 항상 dependency preserving

조건
- a -> b가 자명
- a가 R의 수퍼키
- (new) 