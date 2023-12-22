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

---

<details><summary>PostgreSQL</summary>
PostgreSQL, MySQL, 그리고 MariaDB는 모두 강력한 오픈 소스 관계형 데이터베이스 시스템이지만, 각각의 특징과 차이점이 있습니다. 아래는 PostgreSQL과 MySQL(MariaDB) 간의 두드러진 차별점을 간단히 설명한 것입니다.

1. **ACID 준수:**
    - PostgreSQL은 ACID (Atomicity, Consistency, Isolation, Durability) 속성을 강조하며 트랜잭션의 안정성을 중시합니다.
    - MySQL과 MariaDB도 ACID를 준수하지만, PostgreSQL은 특히 일관성과 격리에 대한 엄격한 요구사항을 갖고 있습니다.

2. **JSON 및 JSONB 데이터 타입:**
    - PostgreSQL은 네이티브로 JSON과 JSONB(이진 형식의 JSON) 데이터 타입을 지원하여 JSON 데이터를 쉽게 저장하고 쿼리할 수 있습니다.
    - MySQL과 MariaDB도 JSON 데이터를 다룰 수 있지만, PostgreSQL은 JSONB의 성능과 유연성에서 우수합니다.

3. **확장 가능성 및 PL/pgSQL:**
    - PostgreSQL은 풍부한 확장성을 제공하며, 사용자 정의 데이터 타입, 함수, 연산자를 작성할 수 있습니다.
    - MySQL과 MariaDB는 확장성 측면에서 제약이 있습니다. PostgreSQL의 내장 프로시저 언어인 PL/pgSQL은 데이터베이스 내에서 프로시저 및 함수를 작성하는 데 사용됩니다.

4. **MVCC (Multi-Version Concurrency Control):**
    - PostgreSQL은 MVCC를 사용하여 동시성을 관리하며, 한 트랜잭션이 데이터를 수정하더라도 다른 트랜잭션은 수정 전의 버전을 볼 수 있습니다.
    - MySQL과 MariaDB도 트랜잭션을 지원하지만, PostgreSQL은 MVCC를 사용하여 더욱 강력한 동시성 제어를 제공합니다.

5. **성능 최적화:**
    - PostgreSQL은 대용량 데이터베이스 및 복잡한 쿼리에 대한 성능 최적화가 잘 이루어져 있습니다.
    - MySQL과 MariaDB도 높은 성능을 제공하지만, 데이터베이스 크기와 복잡성에 따라 PostgreSQL이 더 나은 결과를 낼 수 있습니다.

6. **외부 키 제약의 처리 방식:**
    - PostgreSQL은 외부 키 제약에 대한 처리를 엄격하게 수행하며, 데이터 무결성을 보장합니다.
    - MySQL은 외부 키 제약에 대해 덜 엄격한 경우가 있으며, MariaDB는 MySQL과 호환성을 유지하면서 일부 추가적인 기능을 제공합니다.

이러한 차이점들은 각각의 데이터베이스 시스템이 특정 사용 사례 또는 요구 사항에 더 적합한지를 결정하는 데 도움이 됩니다. 선택은 프로젝트의 목적, 규모, 요구 사항, 개발자의 선호도 등을 고려하여 이루어져야
합니다.


</details>