**Aggregate Function(집계함수)**

여러행으로부터하나의결과값을반환

종류
–AVG 
–COUNT 
	•COUNT( * ): number of rows in a table (NULL도count된다)
	•COUNT(expr): non-null value (NULL은빠진다)
	•COUNT(DISTINCT expr): distinct non-null
–MAX
–MIN  
–SUM
–STDDEV 
–VARIANCE

**일반적인오류**

주의
**SELECT deptno, AVG(sal) FROM emp;**
–집계함수의결과는한row만남게된다
–deptno는하나의row에표현될수없다
–부서별과같은내용이필요할때는**Group by**절사용

->
**SELECT deptno, AVG(sal) 
FROM emp
GROUP BY deptno**
**ORDER BY deptno;**

**HAVING 절**

**Aggregation 결과에대해다시Condition을적용**할때사용

일반적인오류

–평균월급이2000 이상인부서는?

**SELECT deptno, AVG(sal) 
FROM emp
GROUP BY deptno**
**HAVING AVG(sal) > 2000;

주의
–WHERE 절은Aggregation 이전, HAVING 절은Aggregation 이후의filtering
–Having절에는Group by에참여한컬럼이나Aggregate 함수만사용가능




