# DBMS(Database Management System)

![](https://volcanicashnew.com/wp-content/uploads/2021/08/DMBS-RDBMS.jpg)

DBMS는 데이터베이스를 관리하는 소프트웨어 시스템입니다. 이는 데이터의 저장, 검색, 수정, 삭제와 같은 기본적인 데이터 관리 작업을 수행합니다. DBMS는 데이터를 구조화하고 조직화하여 효율적으로 관리하며, 여러
사용자가 데이터에 접근할 수 있도록 지원합니다. 그러나 DBMS는 데이터 간의 관계를 명시적으로 정의하지 않으며, 주로 데이터의 파일 시스템을 관리하는 역할을 합니다.
---
<details><summary>RDBMS(Relational Database Management System)
</summary>

# RDBMS(Relational Database Management System)

RDBMS는 DBMS의 한 유형으로, 데이터를 테이블로 구성하고 테이블 간의 관계를 정의하는 데이터베이스 시스템입니다. RDBMS는 관계형 모델을 기반으로 하며, 데이터베이스 스키마를 사용하여 테이블 간의 관계를
정의합니다. 이는 데이터 중복을 최소화하고 데이터 일관성을 유지하는 데 도움이 됩니다.

RDBMS의 특징은 다음과 같습니다:

1. **데이터의 일관성:** 데이터 중복을 최소화하고, 데이터는 정규화된 형태로 저장됩니다.
2. **SQL 사용:** RDBMS는 SQL(Structured Query Language)을 사용하여 데이터베이스에 대한 쿼리와 조작을 수행합니다.
3. **관계:** 테이블 간의 관계를 사용하여 데이터를 구성하고 관리합니다.
4. **트랜잭션 지원:** 원자성, 일관성, 독립성, 지속성(ACID)과 같은 트랜잭션 속성을 지원하여 데이터의 안정성을 보장합니다.

일반적으로 RDBMS는 DBMS의 일종으로 볼 수 있습니다. 즉, 모든 RDBMS는 DBMS이지만, 모든 DBMS가 RDBMS는 아닙니다. RDBMS는 특정 유형의 DBMS로, 관계형 데이터베이스 모델을 따릅니다.
MySQL, PostgreSQL, Oracle Database 등은 RDBMS의 예입니다.

</details>

---
<details><summary>데이터베이스 언어</summary>
DBMS는 데이터베이스 언어를 제공한다:

![](https://media.geeksforgeeks.org/wp-content/uploads/sql-commands.jpg)

1. DDL (Data Definition Language) → Schema
2. DML (Data Manipulation Language) → Query & Command
3. DCL (Data Control Language) → Grant, Revoke, Commit, Rollback

대부분의 RDBMS에서 이들은 모두 SQL로 표현된다. Schema란 걸 강조하기 위해 **DDL**이란 표현을 자주 사용하니 기억해 둘 것.

SQL은 1970년대에 만들어진 SEQUEL이 이름을 바꾼 것. “에스큐엘” 또는 원래 이름이었던 “시퀄”로 읽는다. 이런 역사적인 맥락 때문에 SQL을 Structured Query Language의 약어라고
소개하는 걸 거부하는 사람들도 있다.

</details>