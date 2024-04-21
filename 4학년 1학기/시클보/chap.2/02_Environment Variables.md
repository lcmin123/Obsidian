- 환경변수
	- 동적으로 명명된 값의 집합
	- 어떤 프로세스가 실행되는 운영 환경의 일부
	- 실행 중인 프로세스의 동작에 영향을 줌
	- PATH 변수가 있으며, 프로그램 실행 전체 경로가 제공되지 않으면 쉘 프로세스가 프로그램의 위치를 찾기 위해 PATH 사용
	- 프로세스는 fork() 시스템 호출을 사용해 새 프로세스가 생성될 때 부모의 환경 변수를 상속하거나, execve() 시스템 호출을 사용해 메모리 공간을 새 프로그램 데이터로 덮어 씌우며 환경변수를 초기화함
	- execve() 호출시 환경변수 전달 가능
	- 환경변수는 초기에 스택에 저장되며, main() 호출전에 스택에 푸시됨
	- envp와 environ은 초기에 동일 위치를 가리키지만, envp는 main() 함수 내에서만 접근 가능하며 environ은 전역변수임
	- 환경변수에 대한 메모리 위치 변경 시 environ 변경 가능
	- 쉘 변수는 쉘에서 사용되는 내부 변수, 사용자가 할당 및 삭제 가능
- 환경 변수 접근 방법
	- main() 파라미터 통해 접근 (char* envp[])
	- 전역변수 사용해 접근 (extern char** environ)
- 프로세스가 환경 변수를 얻는 방법
	- fork() 시스템 호출을 사용해 자식은 부모의 환경 변수를 상속받음
	- 프로세스가 자체적으로 새 프로그램을 실행 할 때, execve()를 이용해 시스템 호출. 이때 프로세스 메모리 공간이 새 프로그램 데이터로 덮어 씌워지며 이전 환경변수 손실. 
	  execve()는 int execve(const char* filename, char* const argv[], <<char* const envp[]>>)를 이용해 환경 변수 전달 가능
- 환경 변수의 메모리 할당
	- 환경 변수는 초기에 메모리의 stack에 저장
	- heap은 malloc, new등으로 동적 메모리 관리
	- main() 호출 전에, 환경 변수들은 스택에 푸시됨
	- 환경 변수들은 스택에 저장된 포인터 배열을 이용해 실제 환경변수 문자열 가리킴
	- main() 함수의 스택 프레임에 위치한 환경 변수들은 main() 함수 내에서 사용됨
	- envp와 environ은 초기에 동일한 위치를 가리킴
	- envp는 main() 함수 내에서만 접근 가능한 지역변수 이며, environ은 전역변수
	- 환경 변수가 변경 되어 환경변수 저장 위치가 heap으로 이동할 때, environ은 가변, envp는 불변
	  → 최신의 환경변수에 접근 위해선 envp보단 environ 포인터 사용이 바람직
- 쉘 변수와 환경 변수
	- 쉘 변수
		- 쉘 내부에서 사용되는 내부 변수
		- 사용자가 생성, 할당 및 삭제 가능
	- 환경 변수
		- 프로세스의 실행 환경에 영향을 주는 동적으로 명명된 값의 집합
		- 프로세스의 동작 방식을 조절
		- 프로그램 실행 시 중요한 역할
- /proc file system
	- /proc 파일 시스템은 커널 및 프로세스 정보 실행중인 프로세스에 대한 데$이터가 저장) 제공
	- strings /proc/ $ /environ 명령어를 통해 현재 프로세스의 환경 변수 출력 가능
- 쉘 변수와 환경 변수 - 이어서
	- 쉘 변수와 환경 변수는 다르다
	- 쉘 프로그램 실행 시 ,쉘 변수로 환경 변수가 복사된다. 쉘 변수 변경은 환경 변수에 영향을 주지 않는다.
```shell 
$ strings /proc/&&/environ | grep LOGNAME
LOGNAME = seed -> 환경변수
$ ehco $LOGNAME
seed -> 쉘변수
$ LOGNAME = bob -> 환경변수 LOGNAME 변경
$ echo $LOGNAME
bob
$ strings /proc/$$/environ | grep LOGNAME
LOGNAME = seed -> 환경변수는 영향없음
$ unset LOGNAME -> 쉘변수 초기화
$ echo $LOGNAME

$ strings /proc/$$/environ | grep LOGNAME
LOGNAME = seed -> 환경변수는 영향없음
```
- 쉘 변수가 자식 프로세스의 환경 변수에 미치는 영향
	- 부모 프로세스의 쉘 변수 중 부모의 환경변수 사본 쉘 변수와, exported shell 변수가 자식으로 전달된다.
	- not exported shell 변수와 predefined shell 변수는 자식에게 전달되지 않는다.
```shell
$ strings /proc/$$/environ | grep LOGNAME
LOGNAME = seed
$ LOGNAME2 = alice
$ export LOGNAME3 = bob 
-> exported shell variable
$ env | grep LOGNAME 
-> 현재 환경변수 보여주는 명령어: 쉘은 새로운 자식 프로세스를 생성해 환경 변수 출력
LOGNAME = seed
LOGNAME3 = bob
$ unset LOGNAME 
-> LOGNAME 쉘 변수 할당 해제
$ env | grep LOGNAME
LOGNAME3 = bob
```
- 환경 변수로 인한 공격 표면
	- 환경 변수의 숨겨진 사용은 위험
	- 사용자가 환경 변수를 설정할 수 있으므로 ,환경변수는 SUID 프로그램에서의 공격표면이 된다
	- 외부 라이브러리 함수 코드를 로딩하는 Linker, Library, External Program, Application Code등이 공격 표면이 될 수 있음
- Linker를 이용한 공격
	- 외부 라이브러리를 로드하는 과정에서 공격 발생가능
	- Dynamic Linking
		- 프로그램 실행 시 linking
		- gcc -o hello hello.c
		- 환경 변수를 사용하며, 그것이 공격표면화
	- Static Linking
		- 컴파일시 linking
		- gcc -static -o hello hello.c
		- 링커는 프로그램 코드와 라이브러리