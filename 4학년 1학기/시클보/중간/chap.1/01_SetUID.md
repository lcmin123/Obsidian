- Two - Tier Approach
	- 운영체제에 의한 접근제어인 generic access, 특권 프로그램에 의한 fine-grained access로 나뉜다
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
cp /bin/cat ./mycat //cat 프로그램 복사

ls -l mycat
>>> -rwxr-xr-x 1 seed seed

sudo chown root mycat //owner seed -> root

ls -l mycat
>>> -rwxr-xr-x root seed // owner changed

mycat /etc/shadow
>>> permission denied

sudo chmod 4755 mycat // change to Set-UID

ls -l mycat
>>> -rwsr-xr-x root seed

mycat /etc/shadow
>>> 정상 출력
```
- Set-UID 작동 방식
	- SUID 프로그램은 특별한 비트가 설정 되어 있다
	  → 이 프로그램이 특정 사용자의 권한으로 실행될 수 있음을 나타냄
	- passwd같은 SUID프로그램이 root 소유이고 SUID 비트 설정되어 있다면 seed는 root권한을 얻어 /etc/shadow 접근 가능
- SUID 보안
	- 일반 사용자에게 특권 부여시, SUID 프로그램을 통해 특권을 일시 획득하는 것 뿐이지 직접 특권 부여하는 것이 아님
	- 그러므로 특정 권한을 얻더라도 프로그램은 제한된 동작만을 수행 가능
	- 모든 프로그램을 SUID로 변경하는 것은 안전하지 않음 → /bin/sh나 vi 주의
- SUID 공격 표면
	- 사용자 입력을 통한 공격
		- 명시적 입력 
			- 버퍼 오버플로우 : 사용자 입력 데이터가 프로그램의 버퍼를 넘어 메모리 영역 침범 → 악의적 코드 실행 가능
			- 형식 문자열 취약점 : 사용자 입력을 문자열로 해석하는 프로그램에서 발생하는 취약점. 프로그램 동작 조작 가능
		- Change Shell(CHSH)과 같은 SUID 프로그램
			- 특권으로 실행하므로 입력을 필터링하지 않으면 보안 문제 발생 가능
			- 쉘 변경 명령 실행 시 입력 검증이 이뤄지지 않으면 새로운 루트 계정을 만들거나 다른 보안문제 초래 가능
	- 시스템 입력을 통한 공격
		- Race Condition : 특권 파일로부터의 심볼릭 링크를 통해 일반 파일에서 특권 파일로 영향 끼칠 수 있음
		- /tmp 폴더같은 world-writable 디렉토리에 쓰기를 통해 프로그램에 영향 가능
	- 환경변수를 통한 공격
		- 환경변수를 설정해 프로그램 동작 조작 가능
		- 사용자가 프로그램 실행 전 설정한 환경 변수를 통해 프로그램 동작에 영향 가능
		- PATH 환경변수
			- 쉘 프로그램에서 명령 실행 시 경로 찾는 변수
			- /bin/sh는 ls 실행 시 PATH를 이용해 실행 파일을 찾음
			- PATH 변수 조작으로 ls 명령 찾기 제어 가능
- Capability Leaking
	- 특정 경우에 특권 프로그램은 실행 중 자신의 권한을 낮추는 경우가 있음
	- su 프로그램은 다른 사용자로 전환하도록 허용하며, 특권을 낮추는 과정에서 특권 누출 가능
	- 특권 하향 전에 특권 기능 미정리시, 특권 누출 가능
	- Capability Leaking을 통한 공격
		- 파일 디스크립터를 닫지 않고 특권이 낮아지면 파일 디스크립터가 여전히 유효할 수 있음
		- 특권이 낮아지기 전에 파일 디스크립터를 삭제해야 취약점 해결 가능
```c 
cap_leak.c

fd = open("etc/zzz", O_RDWR | O_APPEND);
if (fd == -1){
	printf("Cannot open /etc/zzz");
	exit(0);
}
-> file descripter 생성(root owned SUID program), /etc/zzz는 root에 의해서만 쓰기 가능

printf("fd is %d\n", fd);

!close(fd);!

setuid(getuid());
-> 특권 하향

v[0] = "/bin/sh";
v[1] = 0;
execve(v[0], v, 0);
```

```shell
sudo chown root cap_leak
sudo chmod 4755 cap_leak
-> change to root-owned SUID program

ls -l cap_leak
>>> -rwsr-xr-x root seed

cat /etc/zzz
>>> bbb

echo aaa > /etc/zzz
>>> permission denied
-> file에 직접 write 불가능

cap_leak
>>> fd is 3

echo ccc >& 3
-> file descripter 값 3에 직접 redirect 시도: seed도 수정이 가능해진다!

exit

cat /etc/zzz
>>> bbb
>>> ccc
```
- 
	- 위의 예시는 파일을 닫기 전에 fd가 여전히 사용가능해서 발생하는 문제 : Capability Leak이다.
	- 고치려면? 특권 하향 전에 file descripter를 파괴해야한다(close the file)
- Capability Leaking in OS X
	- 동적 링커 dyld에 DYLD_PRINT_TO_FILE 환경 변수를 추가함으로 공격
	- SUID 프로그램에서 root 권한 프로그램이 dyld 호출 시 dyld의 EUID 또한 root가 됨을 이용
- Invoking other programs
	- 일반적으로 SUID 프로그램이 실행하는 외부 명령어는 사용자의 input을 받지 않음
	- 공격
		- 사용자는 종종 외부 명령어에 입력 데이터를 제공하도록 요청받는데,
		- 외부 명령어가 올바르게 호출되지 않으면 사용자의 입력 데이터가 명령어 이름으로 변환될 수 있음 → 위험
	- 안전하지 않은 접근
		- 외부 명령어를 호출하는 가장 쉬운 방법은 system() 함수
		- 아래의 프로그램은 /bin/cat 프로그램을 실행해야 함
		- root 소유 SUID 프로그램이므로 프로그램은 모든 파일을 볼 수는 있지만 쓸 수는 없다
		- 이 프로그램을 사용해 다른 명령어를 root 권한으로 실행해보자
```c
catall.c

int main (int argc, char *argv[]) {
	char *cat = "/bin/cat";
	
	if (argc < 2) {
		priintf("please type a file name");
		return 1;
	}

	char *command = malloc(strlen(cat) + strlen(argv[1]) + 2);
	sprintf(command, "%s %s", cat, argv[1]); 
	-> command 버퍼에 저장된 cat의 문자열 "/bin/cat" 출력, argv[1] 문자열 출력
	system(command); 
	-> /bin/cat
	return 0;
}
```

```shell
$ gcc -o catall catcall.c
$ sudo chown root catall
$ sudo chmod 4755 catall
$ ls -l catall
> -rwsr-xr-x root seed
$ catall /etc/shadow
> 정상 출력
$ catall "aa;/bin/sh" 
-> root 권한으로 system(command) => bin/cat aa;bin/sh 실행한것과 동일.
root 권한의 catall로 부터 bin/sh가 root 권한으로 실행되어 EUID root의 shell 실행.
```
- 
	- 방지하기 위해서..
		- system() 대신 execve() 함수를 이용해 외부 명령어를 호출하면 안전
		- 외부 명령어를 실행할 때 코드와 데이터를 분리해 사용자 input이 코드로 변환되는 것을 방지
		- execve(v[0], v, 0)
			- v[0] : 실행할 프로그램 경로
			- v : 사용자 input data
			- 0 : 환경변수 배열
	- 안전한 코드
```c
catall.c

int main (int argc, char *argv[]) {
	char *v[3];
	
	if (argc < 2) {
		priintf("please type a file name");
		return 1;
	}

	v[0] = "/bin/cat";
	v[1] = argv[1];
	v[2] = 0;

	execve(v[0], v, 0);
	
	return 0;
}
```
- 추가적인 논의
	- execlp(), execvp(), execvpe()등은 PATH 환경변수 공격을 받을 수 있다
- 보안 원칙
	- 코드와 데이터는 반드시 분리되어야 함
	- 공격 표면
		- system()
		- cross site scripting
		- sql injection
		- buffer overfolw attack
- 최소 권한 원칙
	- 특권 프로그램은 필요한 작업만 수행할 수 있는 최하의 권한을 부여받아야 함
	- 특권 프로그램이 해당 권한이 더 이상 필요하지 않을 때 권한을 비활성화 (일시적 혹은 영구적) 해야함
	- Linux에서는 seteuid(), setuid()를 이용해 권한 통제 가능
**- 요약**
	- Set-UID Privileged Programs
		- 특권 프로그램의 필요성, Set-UID 메커니즘 작동, 보안 결함, 공격 표면, 특권 프로그램 보안 강화 방법 등에 대해 다룸
	- Capability Leaking
		- 특권 프로그램이 실행 중 권한을 낮추면서 발생하는 능력 누출에 대한 사례와 취약점 설명
	- Invoking Other Programs
		- 외부 명령어 호출에 대한 안전한 방법과 위험한 접근 방식, 그리고 다른 언어에서의 외부 명령어 호출에 대한 고려 사항을 다룸
	- Principle of Code/Data Isolation
		- 코드와 데이터의 분리를 통한 보안성 강화 방법과 위반 시 발생하는 공격에 대해 설명
	- Principle of Least Privilege
		- 특권 프로그램에 최소한의 권한을 부여하고 필요 없는 권한은 비활성화하는 원칙과 이를 구현하는 방법에 대해 다룸
