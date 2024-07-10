
## 1.
> 사원(==이름==, 부서, 재직년도, 주소, 기본급)
> 자격증(==이름==, 분야 경력)

**자격증을 2 개 이상 가진 사원의 이름을 검색**
``` SQL
SELECT 이름
FROM 자격증
GROUP BY 이름
HAVING COUNT(*) >= 2;
```
## 2.
> 학생(==학번==, 주민등록번호, 이름, 학년, 전화번호, 주소)

**학생 테이블에서 3학년 학생에 대한 모든 속성을 추출한 <3학년학생> 뷰를 정의하는 SQL문을 작성하시오. 단, 갱신이나 삽입 연산시 뷰 정의 조건을 따라야한다.**
```SQL
CREATE VIEW 3학년학생
AS SELECT *
FROM 학생
WHERE 학년=3
WITH CHECK OPTION; -- 갱신이나 삽입 연산 시 뷰의 정의 조건을 위배하면 실행을 거절한다.
```
## 3.
> 강좌(==강좌번호==, 강좌명, 학점, 수강인원, 강의실, 학기, 연도, 교수번호)
> 교수(==교수번호==, 주민등록번호, 이름, 직위, 재직년도)

**교수번호가 같은 튜플을 조인하여 강좌명, 강의실, 수강제한인원, 교수이름 속성을 갖는 뷰를 정의하는 SQL문은?**
```SQL
CREATE VIEW 강좌교수(강좌명, 강의실, 수강제한인원, 교수이름)
AS SELECT 강좌명, 강의실, 수강제한인원, 이름
FROM 강좌 JOIN 교수 USING (교수번호);
```
혹은, 
```SQL
CREATE VIEW 강좌교수(강좌명, 강의실, 수강제한인원, 교수이름)
AS SELECT 강좌명, 강의실, 수강제한인원, 이름
FROM 강좌, 교수
WHERE 강좌.교수번호 = 교수.교수번호;
```
## 4.
> 거래내역(상호, 금액, 세액, 총액)

**총액이 가장 큰 거래처의 '상호'와 '총액'을 검색하는 SQL문**
```SQL
SELECT 상호, 총액 FROM 거래내역 WHERE 총액 IN (SELECT MAX(총액) FROM 거래내역);
```
## 5.
> 장학금(학과, 이름, 장학내역, 장학금)
### 5-1. 장학내역별 장학금에 대한 순위
```SQL
SELECT 장학내역, 장학금,
	RANK() OVER (PARTITION BY 장학내역 ORDER BY 장학금 DESC) AS 장학금순위
FROM 장학금
WHERE 장학내역 = '우수장학';
```
- `WHERE 장학내역= '우수장학'`에 의해, '우수장학'에 해당하는 행들만 선택된다. 
- SQL 쿼리의 논리적 실행 순서는 일반적으로 다음과 같다:
	1. FROM
	2. WHERE
	3. GROUP BY
	4. HAVING
	5. SELECT
	6. ORDER BY
	**RANK는 SELECT 절 안에 있으므로 테이블에서 WHERE에 의해 필터링이 된 후 실행된다.**
### 5-2. 장학금 테이블에 대해 각 조합에 대한 통계를 출력
```SQL
SELECT 장학내역, 학과, AVG(장학금) AS 장학금평균
FROM 장학금
GROUP BY CUBE(장학내역, 학과);
```
'장학내역', '학과'를 기준으로 그룹을 지정하여 그룹 평균과 전체 평균을 구한다.
## 6.
다음 두 테이블에서 주어진 SQL의 실행 결과는?

![이미지](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%218025&authkey=%21ANubbenP4iXLXV0&width=293&height=226)

```SQL
SELECT COUNT(*) CNT FROM A CROSS JOIN B WHERE A.NAME LIKE B.RULE;
```
1. FROM 절에서 [[21. 집합 연산과 관계 연산#^ce60a1|CROSS JOIN ]]이 발생: 교차 조인은 가능한 모든 조합으로 조인한다.

| NAME  | RULE |
| ----- | ---- |
| Smith | S%   |
| Smith | %t%  |
| Allen | S%   |
| Allen | %t%  |
| Scott | S%   |
| Scott | %t%  |
2. WHERE 조건절에 들어서 A의 이름과 B의 규칙이 맞는 것들이 쿼리된다.
	- Smith 2개, Scott 2개.
3. 그 숫자를 COUNT 하였으므로 4가 select 되어 출력된다.