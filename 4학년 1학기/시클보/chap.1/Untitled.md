- Two - Tier Approach
	- 운영체제에서 세밀한 접근 제어를 구현하는 것은 바람직하지 않음
	- 그래서 접근 제어를 위해 특권 프로그램 이용
	  ex) passwd 프로그램 → /etc/shadow 파일에 대한 세밀한 접근 제어 로직 구현
	- 이러한 방식으로 운영 체제의 부담은 줄이고, 접근 제어를 안전하게 처리 가능
- 특권 프로그램의 종류
	- Daemons
		- background에서 돌아가는 프로그램
		- 특권을 가진 사용자에 의해 실행되어야 함
	- Set-UID Programs
		- UNIX 시스템에서 널리 사용
		- 특별한 bit로 mark 되어있다
- Set-UID 개념
	- 사용자가 특정 프로그램 실행 시, 그 프로그램을 소유한 사용자의 특권을 임시로 얻을 수 있는 메커니즘
	- Set-UID 프로그램을 일반 권한으로 실행해도, 소유자 권한으로 실행 됨
	- 각 프로세스는 두개의 ID 소유
		- RUID : 프로세스의 실제 소유자 식별 ID
		- EUID : 프로세스 특권 ID
	- 일반 프로세스 실행 시, RUID = EUID
	- Set-UID 프로세스 실행 시 RUID  ≠ EUID (seed에서 실행 시 RUID = root, EUID = seed)
	- 일반 사용자가 상승된 특권을 가짐
- Set-UID 프로그램으로 전환하기
```shell
cp /bin/cat ./mycat cat 프로그램 복사
sudo chown root mycat owner seed -> root
```