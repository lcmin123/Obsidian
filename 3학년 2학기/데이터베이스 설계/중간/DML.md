  

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

**UPDATE**

조건을만족하는레코드를변경
–10번부서원의월급100인상& 수수료0으로변경
**UPDATE emp SET sal = sal + 100, comm = 0**
**WHERE deptno = 10;**

WHERE 절이생략되면모든레코드에적용
–모든직원의월급10%인상
**UPDATE emp SET sal = sal * 1.1;**

Subquery를이용한변경
–담당업무가'SCOTT'과같은사람들의월급을부서최고액으로변경
**UPDATE emp SET sal = (SELECT MAX(sal) FROM emp)**
**WHERE job = (SELECT job FROM emp WHERE ename='SCOTT');**

**DELETE**

조건을만족하는레코드삭제
–이름이'SCOTT'인사원삭제
**DELETE FROM emp WHERE ename = 'SCOTT';**

조건이없으면모든레코드삭제(주의!)
–모든직원정보삭제
**DELETE FROM emp;**

Subquery를이용한DELETE
–'SALES'부서의직원모두삭제
**DELETE FROM emp WHERE deptno = (SELECT deptno FROM dept WHERE dname = 'SALES');**
  


  






  




