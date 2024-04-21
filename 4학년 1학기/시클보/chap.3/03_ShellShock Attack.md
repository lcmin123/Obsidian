- 쉘쇼크 공격 
	- 배쉬 쉘의 취약점을 이용하는 형태의 공격
	- 쉘 프로그램은 운영 체제의 명령 줄 해석기로 사용자와 운영 체제 간의 인터페이스를 제공합니다.
	- 쉘 함수와 관련된 쉘쇼크 취약점이 소개됩니다.
	- 부모 쉘에서 함수를 정의하고 내보내면 자식 프로세스에서 해당 함수를 사용할 수 있습니다.
	- 환경 변수를 사용하여 함수 정의를 자식 bash 프로세스에 전달하는 방법이 소개됩니다.
	- 쉘쇼크 취약점은 2014년 9월 24일에 CVE-2014-6271로 공개되었으며, bash가 환경 변수를 함수 정의로 변환할 때 발생하는 오류를 악용합니다.
	- 2014년 웹서버에서 사용자로부터 들어온 cgi 리퀘스트를 처리해주는 과정에서 배시 쉘이 실행 됐고, 배시쉘의 실행 과정에서 이 쉘샥 취약점으로 인해 결국에 공격자가 웹서버 상에서 자신이 원하는 다양한 커맨드를 실행하는 것이 가능했었음
- bash 소스 코드의 실수
	- 쉘쇼크 버그는 bash 소스 코드의 variables.c 파일에서 시작됩니다.
	- 환경 변수의 값이 "() {"로 시작하는지 확인하고, 이를 함수 정의로 변환하는 과정에서 발생하는 오류가 설명됩니다.
	- parse_and_execute() 함수가 다른 쉘 명령어도 구문 분석할 수 있어서 보안 문제가 발생할 수 있음을 설명합니다.
- 취약한 Set-UID 프로그램에 대한 쉘쇼크 공격 
	- Set-UID root 프로그램이 /bin/ls 명령어를 실행할 때 쉘쇼크 취약점을 악용하여 권한이 없는 명령어가 실행될 수 있음을 설명합니다.
	- Set-UID root 프로그램이 system() 함수를 사용하여 /bin/ls 명령어를 실행하고, 이를 통해 공격자가 설정한 환경 변수에 의해 무단 명령어가 실행될 수 있음을 보여줍니다.

- 배경 : 쉘 함수
	- 쉘 프로그램은 운영 체제의 명령 줄 해석기
	- 사용자와 운영 체제 간의 인터페이스 제공
	- sh, bash, zsh 등 이 존재
	- bash 쉘은 가장 인기있는 linux 쉘 중 하나
	- 쉘 쇼크 취약점은 쉘 함수와 관련 있음
	- 예시
```shell
 $ foo() { echo "Inside Func"; }
 -> foo라는 함수를 정의
 $ declare -f foo
foo () 
{ 
    echo "Inside Func"
}
-> 함수의 정의를 확인
 $ foo
Inside Func
-> 함수 호출
 $ unset -f foo
 -> 함수 삭제
 $ declare -f foo
 -> 함수의 정의를 확인하면 아무것도 없다
```

- 자식 프로세스에 쉘 함수 전달하기
	- Approach 1 : 부모 쉘에서 함수를 정의하고 내보내면, 자식 프로세스에서 해당 함수 사용 가능
	**- 부모 프로세스도 쉘 프로세스여야 함**
	- 부모가 쉘 함수를 전달한다는 것은 자식 프로세스의 **환경 변수로써** 쉘 함수를 전달하는 것
	- ps $ x 2 로 현재 pid 확인 가능
	- 예시
```shell
$ foo() { echo "Inside Func"; }
 -> foo라는 함수를 정의
$ declare -f foo
foo () 
{ 
    echo "Inside Func"
}
-> 함수의 정의를 확인
$ foo
Inside Func
-> 함수 호출
$ export -f foo
-> 내보내기
$ bash 
-> 자식 프로세스 생성
$ ps $$
-> 자식 프로세스의 pid 나옴
$ declare -f foo
foo (){
	echo "Inside Func"
}
$ foo
Inside Func
```

- 부모 프로세스의 foo()를 unset 한다면 자식 프로세스의 foo()도 해제된다.
- Approach 2 : 환경 변수를 정의한다. 그것이 곧 차일드 배쉬 프로세스의 함수 정의가 된다.
	**- 부모 프로세스가 쉘 프로세스가 아니더라도 환경 변수만 가지고 있으면 된다**
```shell
$ foo = '() { echo "hello"; }'
$ echo $foo
() { echo "hello"; }
$ declare -f foo
-> 아직 쉘 변수 상태이므로 declare 안먹힘
$ export foo
-> export를 통해 child의 환경변수에 추가하고, shell function으로 변환
$ bash_shellshock
-> 취약한 버전의 bash를 차일드에서 실행
-------------------child------------------
$ echo $foo
-> foo는 더이상 shell 변수가 아닌, shell function이다
$ delcare -f foo
foo(){
	echo "hello"
}
$ foo
hello
```

- 두 approach 비교
	- 둘다 환경변수를 사용한다는 점에서 비슷하다
	- approach 1은 부모 쉘이 새 프로세스를 생성했을 때, 각 exported function definition을 환경변수로써 전달한다
	- 자식 프로세스가 bash를 실행하면 bash는 환경 변수를 함수 정의로 다시 해석 =. approach 2
	- approach 2는 부모 프로세스가 쉘 프로세스일 필요가 없다 → 환경변수만 사용 가능하면 됨

- shellshock vulnerability
	- 부모 프로세스는 환경 변수를 통해 자식 쉘 프로세스에 함수 정의 전달
	- 이때 구문 분석 논리 버그로 인해 bash는 변수에 포함된 일부 명령 실행
```shell
$ foo='() {echo "hello";}; echo "attack";'
$ echo $foo
() { echo "hello world"; }; echo "attack";
$ export foo
$ bash_shellshock
-> run bash (vulnerable ver)
attack
-> extra command gets executed!
-----------------child--------------------
$ echo $foo
-> shell function이므로 무응답
$ declare -f foo
foo (){
	echo "hello"
}
```

- bash code의 문제점
``` c
void initialize_shell_variables (char** env,int privmode) {

	if (privmode == 0 && read_but_dont_execute ==0 && STREQN ("() (",string,4)))
}
-> parsing하며
foo = () {echo "hello";}; echo "extra";를
foo () {echo "hello";}; echo "extra";로
코드를 전환한다 (등호 없애기)

parse_and_execute(temp_string, name, SEVAL_NONINT | SEVAL_NONIST);
-> 전환된 코드에서 세미콜론을 기준으로 foo(){echo "hello";} 실행 후 echo "extra"를 실행한다
```



