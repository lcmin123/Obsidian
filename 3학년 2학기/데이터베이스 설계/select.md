**SELECT**

데이터베이스에서원하는데이터를검색, 추출

Syntax
–**SELECT** ALL | DISTINCT 열_리스트
**FROM** 테이블_리스트
**WHERE** 조건
**GROUPBY** 열_리스트 HAVING 조건
**ORDERBY** 열_리스트 ASC | DESC

기능
–Projection: 원하는컬럼선택
–Selection: 원하는튜플선택
–Join: 두개의테이블결합
–기타: 각종계산, 정렬, 집계(Aggregation)

**기본SELECT**

형식
–SELECT *|{[DISTINCT] column|expression [alias],...}FROM table;

내용
–* : 모든컬럼반환
–DISTINCT: 중복된결과제거
–SELECT 컬럼명: Projection
–FROM: 대상테이블
–ALIAS: 컬럼이름변경(as)
–Expression: 기본적인연산및함수사용가능