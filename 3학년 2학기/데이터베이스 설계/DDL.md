- 요약
	CREATE TABLE: 테이블생성
		  Create a table with the same schema as an existing table:
		**create table** temp_account **like** account
			account와 동일한 temp_account 생성
	ALTER TABLE: 테이블관련변경 
	컬럼추가
	–ALTER TABLE book**ADD**(pubsVARCHAR2(50));
	
	컬럼수정
	
	–ALTER TABLE book**MODIFY**(titleVARCHAR2(100));
	
	컬럼삭제
	
	–ALTER TABLE book**DROP COLUMN**author;
	
	UNUSED 컬럼
	
	–ALTER TABLE book**SET UNUSED** (author);ALTER TABLE book**DROP UNUSED COLUMNS**;

테이블삭제

–**DROPTABLE**book;

데이터삭제

–**TRUNCATETABLE**book;

Comment

–**COMMENT ON TABLE** book **IS**‘this is comment’;

RENAME

–**RENAME**book **TO**article;

주의:
–**ROLLBACK의대상이아님!**

- 기본 데이터 타입
	- varchar2(size): 가변 길이 문자열
	- char(size): 고정 길이 문자열
	- number(p, s): 가변 길이 숫자 
	  전체 p 자리중 소수점 이하 s자리
	- date: 고정길이 날짜+시간