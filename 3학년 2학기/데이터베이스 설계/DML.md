  

종류
–add new row(s)
	•**INSERT INTO테이블이름[(컬럼리스트)] VALUES ( 값리스트) ;**
–modify existing rows
	•**UPDATE 테이블이름SET 변경내용[WHERE 조건];**
–remove existing rows
	•**DELETEFROM테이블이름[WHERE 조건];**

**트랜잭션**의대상
–트랜잭션은DML의집합으로이루어짐

**INSERT**
묵시적방법: 컬럼이름, 순서지정하지않음. 테이블생성시정의한순서에따라값지정
**INSERT INTO dept(dname, deptno)VALUES ('MARKETING', 777);**

명시적방법: 컬럼이름명시적사용. 지정되지않은컬럼NULL 자동입력
**INSERT INTO dept VALUES (777, 'MARKETING', NULL);**

Subquery이용: 타테이블로부터데이터복사(테이블은이미존재해야함)
**INSERT INTO deptusaSELECT deptno, dname FROM dept WHERE country = 'USA';**

–참고: CREATE TABLE AS SELECT는없는테이블을생성& 데이터복사

  




