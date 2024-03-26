### **섹션 8. 공통 테이블 표현식 (Common Table Expressions, CTEs):**

### PostgreSQL CTE:

공통 테이블 표현식(CTE)은 WITH 절을 사용하여 정의되며 임시 결과 집합을 생성하기에  복잡한 쿼리를 보다 간결하고 가독성 있게 작성

**주의사항:** 임시 결과 집합이므로 해당 쿼리 내에서만 유효하며, 쿼리가 실행될 때마다 재계산된다.

**예시:**

```sql

WITH cte_example AS (
    SELECT column1, column2
    FROM table_name
    WHERE condition
)
SELECT * FROM cte_example;

```

### CTE를 사용한 재귀 쿼리:

재귀 CTE는 자기 자신을 참조하는 쿼리를 생성하여 계층적 데이터나 그래프 탐색과 같은 작업을 수행

**주의사항:** 재귀 쿼리를 작성할 때는 종료 조건을 명시

**예시 (팩토리얼 계산을 위한 재귀 CTE):**

```sql

WITH RECURSIVE factorial(n, fact) AS (
    SELECT 1, 1
    UNION ALL
    SELECT n + 1, (n + 1) * fact
    FROM factorial
    WHERE n < 10
)
SELECT * FROM factorial;

```

### **섹션 9. 데이터 수정:**

### Insert:

INSERT 문을 사용하여 테이블에 새로운 데이터를 삽입

**주의사항:** 중복된 값을 허용하지 않는 열에 대해 값을 삽입할 때 유의

**예시:**

```sql

INSERT INTO table_name (column1, column2) VALUES (value1, value2);

```

### 여러 행 삽입:

INSERT 문을 사용하여 한 번에 여러 행을 삽입

**주의사항:** 여러 행을 삽입할 때는 각 행의 값이 순서대로 매칭되는지 확인

**예시:**

```sql

INSERT INTO table_name (column1, column2) VALUES
(value1, value2),
(value3, value4),
(value5, value6);

```

### Update:

UPDATE 문을 사용하여 테이블 내의 기존 데이터를 수정

**주의사항:** WHERE 절을 사용하여 수정할 대상 행을 명확히 지정

**예시:**

```sql

UPDATE table_name SET column1 = value1 WHERE condition;

```

### 조인을 통한 업데이트:

다른 테이블의 값에 따라 테이블의 값을 업데이트

**주의사항:** 업데이트하는 행이 여러 개인 경우 조인 조건을 잘 설정

**예시:**

```sql

UPDATE table1
SET table1.column = table2.value
FROM table2
WHERE table1.id = table2.id;

```

### Delete:

DELETE 문을 사용하여 테이블에서 데이터를 삭제

**주의사항:** WHERE 절을 사용하여 삭제할 행을 명확히 지정

**예시:**

```sql

DELETE FROM table_name WHERE condition;

```

### Upsert:

INSERT 문의 ON CONFLICT 절을 사용하여 새로운 행을 삽입하거나 이미 있는 행을 업데이트

**주의사항:** 충돌 시 업데이트되는 값을 명확히 설정

**예시:**

```sql

INSERT INTO table_name (id, column1, column2) VALUES (value_id, value1, value2)
ON CONFLICT (id) DO UPDATE SET column1 = EXCLUDED.column1, column2 = EXCLUDED.column2;

```

### **섹션 10. 트랜잭션:**

### PostgreSQL 트랜잭션:

트랜잭션은 BEGIN, COMMIT 및 ROLLBACK 문을 사용하여 관리되며 BEGIN으로 트랜잭션을 시작하고, COMMIT으로 트랜잭션을 확정하거나 ROLLBACK으로 트랜잭션을 취소

**주의사항:** 트랜잭션을 적절하게 활용하여 데이터 일관성을 유지

**예시:**

```sql

BEGIN; -- 트랜잭션 시작
-- SQL 문 실행
COMMIT; -- 트랜잭션 커밋
ROLLBACK; -- 트랜잭션 롤백

```

### **섹션 11. 데이터 가져오기 및 내보내기:**

### 테이블로 CSV 파일 가져오기:

COPY 문을 사용하여 CSV 파일의 데이터를 테이블로 가져온다.

**주의사항:** 데이터 형식과 일치하는지 확인

**예시:**

```sql
COPY table_name FROM 'path_to_file.csv' DELIMITER ',' CSV HEADER;

```

### PostgreSQL 테이블을 CSV 파일로 내보내기:

COPY 문을 사용하여 테이블의 데이터를 CSV 파일로 내보냄

**주의사항:** 데이터가 올바르게 내보내졌는지 확인

**예시:**

```sql
COPY table_name TO 'path_to_file.csv' DELIMITER ',' CSV HEADER;

```

### **섹션 12. 테이블 관리:**

### 데이터 유형:

PostgreSQL에서는 다양한 데이터 유형을 제공하며, 이를 활용하여 테이블을 생성할 수 있다.

예를 들어 INTEGER는 정수를 저장하는 데 사용되고, TEXT는 긴 문자열을 저장하는 데 사용

데이터 유형을 올바르게 선택하여 데이터의 무결성과 효율성을 유지

**주의사항:** 각 데이터 유형은 데이터 크기, 저장 가능한 값의 범위 및 작업 가능한 연산에 영향을 주기에 데이터의 특성에 따라 적절한 데이터 유형을 선택

**예시:**

```sql

CREATE TABLE example_table (
    id SERIAL PRIMARY KEY,
    name TEXT,
    age INTEGER,
    created_at TIMESTAMP
);

```

### 테이블 생성:

CREATE TABLE 문을 사용하여 새로운 테이블을 생성하며 이때 각 열의 이름과 데이터 유형을 명시

PRIMARY KEY 제약 조건은 해당 열을 테이블의 기본 키로 설정

**주의사항:** 테이블의 열 이름과 데이터 유형을 정확하게 지정하고,  기본 키를 설정하여 데이터의 고유성을 보장

**예시:**

```sql

CREATE TABLE example_table (
    id SERIAL PRIMARY KEY,
    name TEXT,
    age INTEGER,
    created_at TIMESTAMP
);

```

### Select Into 및 Create table as:

SELECT INTO 문이나 CREATE TABLE AS 문을 사용하여 기존 쿼리의 결과를 기반으로 새로운 테이블을 생성하며 이는 기존 데이터를 기반으로 특정 필드를 선택하여 새로운 테이블을 만들 때 유용

**주의사항:** 새로운 테이블의 구조와 데이터를 정확하게 결정하기 위해 원본 쿼리를 신중하게 선택

**예시:**

```sql

CREATE TABLE new_table AS
SELECT id, name
FROM existing_table
WHERE condition;

```

### Auto-increment column with SERIAL:

SERIAL 데이터 유형을 사용하여 자동 증가 열을 생성할 수 있고  주로 고유 식별자로 사용되며, 새로운 행이 삽입될 때마다 자동으로 값이 증가

**주의사항:** SERIAL로 설정된 열은 데이터베이스가 자동으로 관리하므로 직접 값을 할당하거나 변경하지 않아야 함

**예시:**

```sql
CREATE TABLE example_table (
    id SERIAL PRIMARY KEY,
    name TEXT
);

```

### 시퀀스:

시퀀스는 순차적으로 증가하는 숫자 값을 생성하는 데 사용하는데 일반적으로 데이터베이스가 자동으로 관리하는 SERIAL 열의 작동 방식을 이해하기 위해 사용

**주의사항:** 시퀀스를 사용하여 값을 생성할 때 다른 트랜잭션에서 동일한 값을 생성하지 않도록 주의

**예시:**

```sql

CREATE SEQUENCE example_sequence START 1 INCREMENT 1;

```

### Identity column:

IDENTITY 열은 SQL 표준에 따라 생성된 자동 증가 열을 나타내고, SERIAL과 유사하게 새로운 행이 삽입될 때마다 값을 자동으로 증가

**주의사항:** IDENTITY 열은 PostgreSQL 10부터 지원되며, 기본 키로 사용

**예시:**

```sql

CREATE TABLE example_table (
    id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name TEXT
);

```

### Alter table:

ALTER TABLE 문을 사용하여 기존 테이블의 구조를 변경할 수 있고, 이는 새로운 열을 추가하거나 기존 열을 수정할 때 사용

**주의사항:** 테이블을 수정할 때는 해당 테이블이 사용 중인지 확인하고, 변경 사항이 예상대로 실행되는지 테스트

**예시:**

```sql

ALTER TABLE example_table ADD COLUMN email VARCHAR(100);

```

### Rename table:

ALTER TABLE 문을 사용하여 테이블의 이름을 변경 가능

**주의사항:** 테이블의 이름을 변경할 때는 해당 테이블을 참조하는 모든 응용 프로그램 코드나 쿼리를 업데이트

**예시:**

```sql

ALTER TABLE old_table RENAME TO new_table;

```

### Add column:

ALTER TABLE 문을 사용하여 기존 테이블에 새로운 열을 추가할 수  있음.

**주의사항:** 새로운 열을 추가할 때 기존 데이터와의 호환성을 고려해야 합니다. 또한 추가된 열에 대해 적절한 기본값을 설정

**예시:**

```sql

ALTER TABLE example_table ADD COLUMN email VARCHAR(100);

```

### Drop column:

ALTER TABLE 문을 사용하여 기존 테이블에서 열을 삭제

**주의사항:** 열을 삭제할 때 해당 열에 저장된 데이터를 잃게 되며 데이터 손실을 피하기 위해 열을 삭제하기 전에 잘 고려

**예시:**

```sql

ALTER TABLE example_table DROP COLUMN email;

```

DROP TABLE 문을 사용하여 테이블을 삭제

**주의사항:** 테이블을 삭제하면 해당 테이블의 모든 데이터 및 구조가 영구적으로 삭제되기에  삭제 전에 백업을 만들거나 삭제할 테이블에 대한 권한이 있는지 확인

**예시:**

```sql

DROP TABLE example_table;

```

### Truncate table:

TRUNCATE TABLE 문을 사용하여 테이블의 모든 데이터를 삭제할 수 있고, 이는 DELETE 문보다 효율적이며, 테이블의 구조는 변경하지 않음

**주의사항:** TRUNCATE TABLE 문을 사용하면 해당 테이블의 모든 데이터가 삭제되므로 주의해야 하고, 트랜잭션 롤백이 불가능하며, 시퀀스나 제약 조건은 영향을 받지 않음

**예시:**

```sql

TRUNCATE TABLE example_table;

```

### Temporary table:

임시 테이블은 현재 세션에만 존재하는 테이블로 임시 테이블은 특정 작업을 수행하는 동안 데이터를 저장하거나 처리할 때 유용

**주의사항:** 임시 테이블은 세션이 종료되면 자동으로 삭제되므로 영구적인 데이터 저장에는 사용되어선 안됨

**예시:**

```sql

CREATE TEMP TABLE temp_table (
    id SERIAL PRIMARY KEY,
    name TEXT
);

```

### Copy a table:

기존 테이블을 복사하여 새로운 테이블을 생성할 수 있고, 이는 데이터를 백업하거나 기존 데이터를 가지고 새로운 실험을 수행할 때 유용

**주의사항:** 새로운 테이블을 생성할 때 테이블 구조와 데이터 유형이 정확히 일치하는지 확인

**예시:**

```sql

CREATE TABLE new_table AS
SELECT *
FROM existing_table;

```
