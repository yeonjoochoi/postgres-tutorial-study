
|목차|내용|
|------|---|
|1|[섹션 1. 데이터 쿼리](#섹션1.데이터쿼리)|
|2|[섹션 2. 데이터 필터링](#섹션2.데이터필터링)|
|3|[섹션 3. 여러 테이블 조인](#섹션3.여러테이블조인)|
|4|[섹션 4. 데이터 그룹화](#섹션4.데이터그룹화)|
|5|[섹션 5. 집합 연산](#섹션5.집합연산)|
|6|[섹션 6. 그룹 세트, 큐브 및 롤업](#섹션6.그룹세트,큐브및롤업)|
|7|[섹션 7. 서브쿼리](#섹션7.서브쿼리)|

# 섹션 1. 데이터 쿼리

**Select**

- **용도**: 데이터베이스에서 정보를 조회하는 가장 기본적인 기능
- **정의**: SELECT 문은 테이블에서 데이터를 선택하고 검색하는 데 사용
- **예시**:  (employees 테이블에서 모든 열을 선택)
- **유의할 점**: 데이터베이스에서 많은 열을 선택하는 것은 성능에 부담을 줄 수 있으므로 필요한 열만 선택하여 쿼리의 성능을 최적화

```sql
SELECT * FROM employees;
```

**Column aliases**

- **용도**: 쿼리 결과의 열에 임시 이름을 지정하여 가독성을 높이거나 계산된 열에 이름을 부여
- **정의**: SELECT 문에서 AS 키워드를 사용하여 열 별칭을 지정
- **예시**:  (employees 테이블에서 employee_id를 id로, employee_name을 name으로 선택)
- **유의할 점**:열 별칭을 사용할 때는 의미 있는 이름을 지정해야 쿼리 결과를 이해하고 유지 관리하기 쉬움

```sql
SELECT employee_id AS id, employee_name AS name 
FROM employees;
```

**Order By**

- **용도**: 결과 집합을 정렬하여 데이터를 쉽게 읽을 수 있도록 함
- **정의**: ORDER BY 절을 사용하여 정렬하고자 하는 열을 지정
- **예시**: (employees 테이블의 hire_date 열을 내림차순으로 정렬하여 선택)
- **유의할 점**: 정렬 작업은 성능에 영향을 미칠 수 있으므로  정렬된 결과가 필요한 경우에만 ORDER BY를 사용.

```sql
SELECT * FROM employees 
ORDER BY hire_date DESC;
```

**Select Distinct**

- **용도**: 중복된 행을 제거하여 결과 집합에서 고유한 값을 선택
- **정의**: DISTINCT 키워드를 사용하여 중복을 제거
- **예시**: (employees 테이블의 department_id 열에서 중복을 제거하여 선택)
- **유의할 점**: DISTINCT를 사용하여 중복을 제거할 때 결과 집합이 많은 행을 포함하는 경우 성능에 영향을 줄 수 있기에 DISTINCT 대신 그룹화를 고려

```sql
SELECT DISTINCT department_id 
FROM employees;
```

# 섹션 2. 데이터 필터링

**Where**

- **용도**: 지정된 조건에 따라 행을 필터링
- **정의**: WHERE 절을 사용하여 조건을 지정
- **예시**: (employees 테이블에서 department_id가 100인 행을 선택)
- **유의할 점**: WHERE 절에서는  인덱스가 없는 열에 대해 조건을 지정하는 것은 성능 저하를 가져올 수  있기에 적절한 인덱스를 사용하여 검색을 최적화

```sql
SELECT * FROM employees 
WHERE department_id = 100;
```

**AND operator**

- **용도**: 여러 개의 조건을 모두 만족하는 행을 반환
- **정의**: WHERE 절에서 두 조건을 AND로 결합
- **예시**: (employees 테이블에서 나이가 30 이상이고 직책이 'Manager'인 행을 선택)

```sql
SELECT * FROM employees 
WHERE age >= 30 AND job_title = 'Manager';
```

**OR operator**

- **용도**: 여러 개의 조건 중 하나 이상을 만족하는 행을 반환
- **정의**: WHERE 절에서 두 조건을 OR로 결합
- **예시**: (employees 테이블에서 department_id가 200 또는 300인 행을 선택)

```sql
SELECT * FROM employees 
WHERE department_id = 200 OR department_id = 300;
```

**LIMIT**

- **용도**: 반환되는 행의 수를 제한
- **정의**: LIMIT 절을 사용하여 반환되는 행의 수를 제
- **예시**:  (employees 테이블에서 최대 10개의 행을 선택)

```sql
SELECT * FROM employees 
LIMIT 10;
```

**Fetch**

- **용도**: FETCH 절을 사용하여 반환되는 행의 수를 제한하고 오프셋을 설정하여 일정 수의 행을 건너뜀
- **정의**: FETCH FIRST N ROWS ONLY를 사용하여 N개의 행을 반환
- **예시**: (employees 테이블에서 처음부터 5개의 행을 선택)

```sql
SELECT * FROM employees 
FETCH FIRST 5 ROWS ONLY;
```

**IN**

- **용도**: 값 목록에서 일치하는 데이터를 선택
- **정의**: WHERE 절에서 IN을 사용하여 값 목록에서 데이터를 선택
- 예시: (employees 테이블에서 department_id가 100 또는 200인 행을 선택)

```sql
SELECT * FROM employees 
WHERE department_id IN (100, 200);
```

**Between**

- **용도**: 지정된 범위 내의 데이터를 선택
- **정의**: WHERE 절에서 BETWEEN을 사용하여 데이터의 범위를 선택
- **예시**: (employees 테이블에서 나이가 25에서 35 사이인 행을 선택)

```sql
SELECT * FROM employees 
WHERE age BETWEEN 25 AND 35;
```

**Like**

- **용도**: 패턴에 따라 데이터를 필터링
- **정의**: WHERE 절에서 LIKE를 사용하여 패턴을 지정
- **예시**:  (employees 테이블에서 이름이 'John'으로 시작하는 행을 선택)

```sql
SELECT * FROM employees 
WHERE employee_name LIKE 'John%';
```

**IS NULL**

- **용도**: 값이 null인지 아닌지를 확인
- **정의**: WHERE 절에서 IS NULL 또는 IS NOT NULL을 사용하여 null 값을 확인
- **예시**: (employees 테이블에서 email 열이 null인 행을 선택)

```sql
SELECT * FROM employees 
WHERE email IS NULL;
```

# 섹션 3. 여러 테이블 조인

**JOINS**

- **용도**: 여러 테이블에서 데이터를 결합
- **정의**: JOIN을 사용하여 테이블을 결합
- **예시**:  (employees 테이블과 departments 테이블을 department_id로 조인하여 선택)
- **주의할 점:** 조인할 때는 . 작은 테이블을 기준으로 조인하고 인덱스를 활용하는 것이 성능 향상에 도움되므로 테이블의 크기와 인덱스를 고려하여 조인 방법을 선택

```sql
SELECT * FROM employees 
JOIN departments ON employees.department_id = departments.department_id;
```

**Table aliases**

- **용도**: 테이블 별칭을 사용하여 쿼리를 단순화하고 이름 충돌을 피함
- **정의**: AS를 사용하여 테이블 별칭을 지정
- **예시**: (employees 테이블과 departments 테이블을 별칭을 사용하여 조인)

```sql
SELECT e.employee_name, d.department_name 
FROM employees AS e 
JOIN departments AS d ON e.department_id = d.department_id;
```

**Inner Join**

- **용도**: 다른 테이블에서 일치하는 행을 가진 행을 선택
- **정의**: INNER JOIN을 사용하여 일치하는 행을 선택
- **예시**:  (employees 테이블과 departments 테이블을 department_id로 내부 조인)

```sql
SELECT * FROM employees 
INNER JOIN departments ON employees.department_id = departments.department_id;
```

**Left Join**

- **용도**: 왼쪽 테이블의 모든 행과 오른쪽 테이블의 일치하는 행을 선택
- **정의**: LEFT JOIN을 사용하여 왼쪽 테이블의 모든 행을 선택
- **예시**:  (employees 테이블을 기준으로 departments 테이블과 왼쪽 조인)

```sql
SELECT * FROM employees 
LEFT JOIN departments ON employees.department_id = departments.department_id;
```

**Self-join**

- **용도**: 동일한 테이블 내의 행을 비교
- **정의**: 테이블을 자체에 조인하여 사용
- **예시**: (employees 테이블을 자체에 조인하여 직원과 그 직원의 관리자를 선택)

```sql
SELECT e1.employee_name, e2.manager_name 
FROM employees AS e1 
JOIN employees AS e2 ON e1.manager_id = e2.employee_id;
```

**Full Outer Join**

- **용도**: 일치하는 행이 없는 테이블의 행을 포함
- **정의**: FULL OUTER JOIN을 사용하여 모든 행을 선택
- **예시**:  (employees 테이블과 departments 테이블을 외부 조인)

```sql
SELECT * FROM employees 
FULL OUTER JOIN departments ON employees.department_id = departments.department_id;
```

**Cross Join**

- **용도**: 두 테이블의 모든 가능한 조합을 생성
- **정의**: CROSS JOIN을 사용하여 테이블을 결합
- **예시**:  (employees 테이블과 departments 테이블의 모든 가능한 조합을 선택)

```sql
SELECT * FROM employees 
CROSS JOIN departments;
```

**Natural join**

- **용도**: 암시적 조인 조건을 사용하여 테이블을 결합
- **정의**: 공통 열 이름을 기반으로 조인을 수행
- **예시**:  (employees 테이블과 departments 테이블을 내부 조인하여 선택)

```sql
SELECT * FROM employees, departments 
WHERE employees.department_id = departments.department_id;
```

# 섹션 4. 데이터 그룹화

**Group By**

- **용도**: 행을 그룹으로 나누고 각 그룹에 대해 집계 함수를 적용하여 데이터를 요약
- **정의**: GROUP BY 절을 사용하여 데이터를 그룹화하고, 집계 함수를 사용하여 각 그룹의 결과를 계산
- **예시**: (employees 테이블을 department_id로 그룹화하고 각 부서의 평균 연봉을 계산)
- **유의할 점:**그룹화되지 않은 열을 SELECT 목록에 포함하면 오류가 발생하므로 그룹화된 열을 선택할 때는 집계 함수와 함께 사용

```sql
SELECT department_id, AVG(salary) 
FROM employees 
GROUP BY department_id;
```

**Having**

- **용도**: 그룹에 조건을 적용하여 특정 조건을 충족하는 그룹을 필터링
- **정의**: HAVING 절을 사용하여 그룹화된 결과에 조건을 적용
- **예시**:  (employees 테이블을 department_id로 그룹화하고 평균 연봉이 50000 이상인 부서만 선택)

```sql
SELECT department_id, AVG(salary) 
FROM employees 
GROUP BY department_id 
HAVING AVG(salary) > 50000;
```

# 섹션 5. 집합 연산

**Union**

- **용도**: 여러 쿼리의 결과를 하나의 결과 집합으로 결합
- **정의**: UNION 연산자를 사용하여 여러 쿼리의 결과를 결합
- **예시**: (employees와 archived_employees 테이블의 employee_id 열을 합침)

```sql
SELECT employee_id FROM employees 
UNION 
SELECT employee_id FROM archived_employees;
```

**Intersect**

- **용도**: 두 개 이상의 쿼리 결과 집합을 결합하고 양쪽 결과 집합에 모두 나타나는 행을 반환
- **정의**: INTERSECT 연산자를 사용하여 두 개 이상의 쿼리 결과를 교차
- **예시**:  (employees와 managers 테이블에서 동일한 employee_id를 가진 행을 선택)

```sql
SELECT employee_id FROM employees 
INTERSECT 
SELECT employee_id FROM managers;
```

**Except**

- **용도**: 두 번째 쿼리의 출력에 나타나지 않는 첫 번째 쿼리의 행을 반환
- **정의**: EXCEPT 연산자를 사용하여 첫 번째 쿼리 결과에서 두 번째 쿼리 결과를 제외
- **예시**: (managers 테이블에 있는 employee_id를 제외한 employees 테이블의 employee_id를 선택)

```sql
SELECT employee_id FROM employees 
EXCEPT 
SELECT employee_id FROM managers;
```

# 섹션 6. 그룹 세트, 큐브 및 롤업

**Grouping Sets**

- **용도**: 여러 그룹 세트를 생성하여 다양한 요약 레벨의 보고서를 생성
- **정의**: GROUPING SETS 절을 사용하여 여러 그룹 세트를 정의
- **예시**:(부서별, 직무별, 전체에 대한 평균 연봉을 계산)

```sql
SELECT department_id, job_id, AVG(salary) 
FROM employees 
GROUP BY GROUPING SETS ((department_id), (job_id), ());
```

**Cube:**

- **용도**: 모든 가능한 차원 조합을 포함하는 여러 그룹 세트를 정의
- **정의**: CUBE 절을 사용하여 모든 가능한 조합을 생성
- **예시**: (부서별, 직무별, 부서-직무 별, 전체에 대한 평균 연봉을 계산)

```sql

SELECT department_id, job_id, AVG(salary)
FROM employees
GROUP BY CUBE (department_id, job_id)
```

**Rollup**:

- **용도**: 합계 및 소계를 포함하는 보고서를 생성합니다.
- **정의**: ROLLUP 절을 사용하여 소계와 합계를 생성합니다.
- **예시**:(부서별, 직무별, 전체에 대한 평균 연봉과 부서별 및 전체에 대한 소계 및 합계를 계산)

```sql
SELECT department_id, job_id, AVG(salary) 
FROM employees 
GROUP BY ROLLUP (department_id, job_id);
```

# 섹션 7. 서브쿼리

**Subquery**:

- **용도**: 다른 쿼리 내에 중첩된 쿼리를 작성
- **정의**: 다른 쿼리 내에서 사용되는 중첩된 쿼리를 작성
- **예시**:(departments 테이블에서 HR 부서의 department_id를 가져와 employees 테이블에서 해당 부서에 속한 직원을 선택)
- **유의할 점**: 대량의 데이터를 처리하는 서브쿼리는 성능에 부정적인 영향을 미칠 수 있기에 서브쿼리를 사용할 때는 중첩된 쿼리의 결과가 너무 많은 행을 반환하지 않도록 주의

```sql
SELECT employee_name, department_id 
FROM employees 
WHERE department_id = (SELECT department_id FROM departments WHERE department_name = 'HR');
```

**Correlated Subquery**:

- **용도**: 현재 처리 중인 행의 값에 의존하여 쿼리를 수행
- **정의**: 외부 쿼리와 내부 쿼리 사이에 종속성이 있는 중첩된 쿼리를 작성
- **예시**:(각 직원의 연봉이 해당 부서의 평균 연봉보다 높은 경우 해당 직원을 선택)

```sql
SELECT employee_name, salary 
FROM employees e1 
WHERE salary > (SELECT AVG(salary) FROM employees e2 WHERE e2.department_id = e1.department_id);
```

**ANY**:

- **용도**: 하위 쿼리에서 반환된 값 집합과 값을 비교하여 데이터를 검색
- **정의**: ANY 연산자를 사용하여 하위 쿼리의 결과 집합 중 하나라도 조건을 만족하는 데이터를 선택
- **예시**:(직원의 연봉이 매니저 중 하나의 연봉보다 높은 경우 해당 직원을 선택)

```sql
SELECT employee_name 
FROM employees 
WHERE salary > ANY (SELECT salary FROM managers);
```

**ALL**:

- **용도**: 하위 쿼리에서 반환된 값 목록과 값을 비교하여 데이터를 쿼리
- **정의**: ALL 연산자를 사용하여 하위 쿼리의 결과 집합에 모든 값이 조건을 만족하는 데이터를 선택
- **예시**:(직원의 연봉이 모든 인턴의 연봉보다 높은 경우 해당 직원을 선택)

```sql
SELECT employee_name 
FROM employees 
WHERE salary > ALL (SELECT salary FROM interns);

```

**EXISTS**:

- **용도**: 하위 쿼리의 결과 집합의 존재 여부를 확인
- **정의**: EXISTS 연산자를 사용하여 하위 쿼리의 결과 집합이 비어있지 않은지 확인
- **예시**:(부서에 속한 직원이 있는 경우 해당 부서를 선택)

```sql
SELECT department_name 
FROM departments d 
WHERE EXISTS (SELECT * FROM employees e WHERE e.department_id = d.department_id);

```
