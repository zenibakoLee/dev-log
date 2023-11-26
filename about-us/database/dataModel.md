# Data Model

> [Data Model](https://en.wikipedia.org/wiki/Data_model)
>

> [Types of Data Models](https://opentextbc.ca/dbdesign01/chapter/chapter-4-types-of-database-models/)
>

데이터 모델은 크게 3가지로 구분한다:

1. Conceptual Data Model(개념)
2. Logical Data Model(논리)
3. Physical Data Model(물리)

일반적으로 데이터 모델을 구분한다고 하면 이들을 다시 세분화한 모델(특히 논리적 데이터 모델)의 유형을 다룬다. 여기서는 가장 인기 있는 논리적, 개념적 데이터 모델을 다룬다.

---

### Relational Model

> [관계형 모델](https://ko.wikipedia.org/wiki/관계형_모델)
>

> [관계형 데이터베이스](https://ko.wikipedia.org/wiki/관계형_데이터베이스) - 관계형 모델을 기반으로 데이터를 구축한 데이터베이스 시스템
>

> [The Relational Data Model](https://opentextbc.ca/dbdesign01/chapter/chapter-7-the-relational-data-model/)
>

가장 인기 있는 논리적 데이터 모델. 다른 논리적 데이터 모델로 Hierarchical, Network, Object-based Model이 있다.

정말 많은 사람들이 ERM(Entity-Relationship Model)과 RM(Relational Model)을 구분하지 못 한다. ERM의 Relationship은 Entity 사이의 관계를 의미하고, RM의
Relation은 “Tuple의 집합(거칠게 말하면 Table)”을 의미한다. Relationship과 Relation은 유사해 보이지만(심지어 번역하면 둘 다 “관계”라서 구분하기 어렵다), 실제로 이 둘은
“락”과 “락스”처럼 아무 상관 없는 용어라고 생각하자.

> [**A Relational Model of Data for Large Shared Data Banks](https://web.archive.org/web/20070612235326/http://www.acm.org/classics/nov95/toc.html)**
>
>*“The term **relation** is used here in its accepted mathematical sense. Given sets S1, S1, ···, Sn, (not necessarily
> distinct), R is a **relation** on these n sets if it is a **set of n-tuples** each of which has its first element from
> S1, its second element from S1, and so on.”*
>


관계형 모델에서 쓰이는 3개의 개념을 정확히 이해해야 한다:

1. Relation
2. Tuple
3. Attribute

![](https://upload.wikimedia.org/wikipedia/commons/7/7c/Relational_database_terms.svg)

여기서는 가장 작은 것부터 역순으로 살펴보자.

---
<details><summary>속성(Attribute)</summary>

### 속성(Attribute)

속성은 이름과 타입으로 구성된다. 이름은 집합 안에서 유일해야 한다.

예를 들면,

- 이름/문자열
- 나이/정수
- 성별/문자

이런 게 속성이다.

대개는 Column으로 구현된다.

</details>


---
<details><summary>튜플(Tuple)</summary>

### 튜플(Tuple)

(속성, 값) 쌍의 집합. 하나의 집합에서 속성 이름은 유일하기 때문에, 속성 이름은 겹치지 않는다.

예를 들면 다음과 같다:

```
{ (이름/문자열, 견우), (나이/정수, 13), (성별/문자, 남) }
```

대개는 Row, Record로 구현된다.

튜플은 집합이기 때문에 중복을 허용하지 않지만, 대부분의 RDBMS는 중복을 허용한다. 그리고 RDBMS는 NULL도 허용한다.


</details>


---

<details><summary>관계(Relation)</summary>

### 관계(Relation)

> [Relational Model](https://en.wikipedia.org/wiki/Relational_model)
>

> [Relation (Database)](https://en.wikipedia.org/wiki/Relation_(database))
>

(속성의 집합, 튜플의 집합) 쌍. 속성의 집합을 heading, 튜플의 집합을 body라고 구분한다. 그냥 관계는 “튜플의 집합”이라고 할 수도 있다.

예를 들면 다음과 같다:

```
(
	// Heading
	{ 이름/문자열, 나이/정수, 성별/문자 },

	// Body
	{
		{ (이름/문자열, 견우), (나이/정수, 13), (성별/문자, 남) },
		{ (이름/문자열,직녀), (나이/정수, 12), (성별/문자, 여) }
	}
)
```

관계는 시간에 따라 변하기 때문에, 관계 변수(Relation Value)란 개념을 구분해서 사용할 수 있다.

대개는 Table로 구현되고, 속성 집합을 Schema로 표현한다.


</details>

---
<details><summary>연산</summary>

### 연산

> [관계대수](https://ko.wikipedia.org/wiki/관계대수)
>

하나 이상의 Relation으로 새로운 Relation을 만들 수 있다.

산술 연산의 경우 1 + 2 같은 산술 표현식으로 1과 2라는 두 개의 정수의 덧셈 연산을 표현할 수 있고, 이를 계산(평가)하면 3이라는 새로운 정수를 얻을 수 있다. Relation도 마찬가지로 여러 연산을 통해
새로운 Relation을 얻을 수 있다.

- 단항 연산: Selection, Projection, Rename, …
- 이항 연산: Cartesian Product, Set Union, Set Difference, …

여기서는 대표적인 연산 몇 가지만 알아보자.

---

### Projection

원하는 Attribute을 포함하는 Pair로 Tuple을 구성.

SQL에선 SELECT절 바로 뒤에 속성 이름을 나열하는 방식으로 표현할 수 있다.

```sql
*
*
SELECT name, age, gender * *
FROM people;
```

특별히 속성을 제한하고 싶지 않다면 그냥 와일드카드(*)를 쓰면 된다.

```sql
*
*
SELECT * * *
FROM people;
```

---

### Selection

주어진 술어(거칠게 말하면 “조건”)를 만족하는 Tuple만 선택.

SQL에선 SELECT문의 WHERE절로 술어를 표현할 수 있다.

```sql
SELECT name, age, gender
FROM people **
WHERE age < 13;
*
*
```

---

### Cartesian Product

> [곱집합](https://ko.wikipedia.org/wiki/곱집합)
>

Relation의 Cartesian Product는 원래 의미와 다르다. 원래는 각 요소의 쌍을 요소로 취하지만(x와 y가 있다면 (x, y)를 새로운 요소로 사용), 관계 대수에서는 그냥 Tuple을 합친다.

SQL에선 그냥 FROM 뒤에 관계(SQL에선 Table) 이름을 나열하면 된다.

```sql
SELECT *
           * *
FROM r1,
     r2;
*
*
```

대부분은 Cartesian Product를 그대로 사용하지 않고 Selection과 함께 사용하는데, 이를 Join이라고 한다.

견우, 직녀 등 사람을 다루는 관계 people이 있고, 각 사람이 소유한 물건을 다루는 관계 items가 있을 때, 사람의 이름(name)을 키로 사용한다면 다음과 같이 표현할 수 있다.

> ***σpeople.name = items.person_name(people × items)***

**σ** = Selection
**×** = Cartesian Product
>

SQL로 표현하면 다음과 같다. 속성 이름이 겹칠 경우엔 관계(SQL에선 Table) 이름과 마침표를 써서 특정해줄 수 있다. 속성 이름을 바꿔주기 위해 AS도 함께 써주자.

```sql
SELECT people.name, age, gender, items.name AS item_name, usage
FROM people, items
WHERE people.name = items.person_name;
```

SQL에선 SELECT와 WHERE로 표현할 수도 있지만, JOIN을 쓰면 편하다.

```sql
SELECT * * people.name * *, age, gender, * * items.name AS item_name**, usage
FROM people
    ** JOIN items
ON people.name = items.person_name;
*
*
```

아무 것도 소유하지 않은 사람을 포함시키고 싶다면 OUTER JOIN을 사용하면 된다.

```sql
SELECT people.name, age, gender, items.name AS item_name, usage
FROM people
    ** LEFT OUTER JOIN items
ON people.name = items.person_name**;
```

누구도 소유하지 않은 아이템을 포함시키고 싶다면 LEFT 대신 RIGHT를 쓰면 된다. 하지만 이렇게 쓰지 않는 걸 추천한다(SELECT - FROM items로 아이템에 방점을 찍는 게 더 낫다).

```sql
SELECT people.name, age, gender, items.name AS item_name, usage
FROM people
    ** RIGHT OUTER JOIN items
ON people.name = items.person_name**;
```

실제로는 사람이 아니라 아이템에 방점이 찍히는 경우가 대부분이라 그냥 조인도 다음과 같이 쓸 때가 많다.

```sql
SELECT items.name AS name, usage, people.gender
FROM items
    JOIN people
ON items.person_name = people.name;
```

</details>

---
<details><summary>Entity-Relationship Model</summary>

### Entity-Relationship Model

> [Entity-Relationship Model](https://en.wikipedia.org/wiki/Entity–relationship_model)
>

> [The Entity Relationship Data Model](https://opentextbc.ca/dbdesign01/chapter/chapter-8-entity-relationship-model/)
>

가장 인기 있는 개념적 데이터 모델.

ER 모델에서 Entity는 개별적으로 다룰 수 있는 데이터를 의미하며, Entity Type은 같은 Attributes를 가진 Entity의 집합이다(OOP의 Class와 유사하다). ERD 등을 그릴 때 쓰이는
건 Entity Type이다(Class Diagram을 떠올려보자).

참고로 OOP, DDD에선 Entity를 연속성과 식별성이 있는 객체란 의미로 사용한다. 똑같이 Entity란 표현을 쓰지만 실제로는 완전히 다른 의미를 갖는다. 데이터 모델링과 객체 모델링은 목표도 다르고, 용어,
원칙 등도 다르니 주의하자.

다시 강조하면, ER 모델에서 Relationship은 Entity 사이의 관계를 의미한다. 또 다시 강조하면, 관계형 데이터베이스의 관계(Relation)를 ERM의 관계(Relationship)로 잘못 알고 있는
경우가 정말 많다. 주의할 것!


</details>

---
<details><summary>ERD (Entity-Relationship Diagram)</summary>

## ERD (Entity-Relationship Diagram)

> [ER Diagram MMORPG](https://commons.wikimedia.org/wiki/File:ER_Diagram_MMORPG.png)
>

> [Cardinality (data modeling)](https://en.wikipedia.org/wiki/Cardinality_(data_modeling))
>

개념적 데이터 모델인 ER 모델을 시각화하는 (엄청나게 인기 있는) 방법. 논리적 설계에 들어가기 전에 그려보면 도움이 된다.

도구나 표기법에 집착하지 말고, 모델을 검증하는 도구로 활용할 것! 엔티티를 발견(!)하고, 적정하게 재배치하는 것만으로도 많은 통찰을 얻을 수 있다. 기계적으로 스키마 변환 작업만 하거나, 한번 정하고 수정하지
않는 건 추천하지 않는다.

현업에서는 마름모로 표시하는 Relationship을 생략할 때가 많은데, 처음에는 꼭 그려보자. “말이 되는” 설계가 되도록 연습할 것.

Crow's Foot Notation을 쓰더라도 Relationship을 꼬박꼬박 써줄 수도 있다.

→ [ERD-artist-performs-song](https://commons.wikimedia.org/wiki/File:ERD-artist-performs-song.svg)

</details>

---

<details><summary>정규화(Normalization)</summary>

![](https://galaktika-soft.com/wp-content/uploads/2019/07/The-concept-of-data-processing-1.jpg)

정규화는 관계형 데이터베이스 설계에서 중복을 최소화하고 데이터의 일관성을 유지하기 위한 프로세스입니다. 데이터의 저장 및 관리를 효율적으로 하기 위해 테이블을 구조화하는 과정으로, 주로 이상현상(Anomalies)을
방지하고 데이터 일관성을 유지하기 위해 사용됩니다. 이상현상에는 삽입 이상(Insertion Anomaly), 삭제 이상(Deletion Anomaly), 갱신 이상(Update Anomaly)이 포함됩니다.

정규화는 주로 다음의 정규형에 따라 수행됩니다:

1. **제1정규형(1NF):** 각 열은 원자적인 값을 가져야 합니다. 즉, 모든 속성의 도메인이 원자값(더 이상 나눌 수 없는 최소한의 단위)이어야 합니다.

2. **제2정규형(2NF):** 모든 비주요 속성이 주요 속성에 대해 완전 함수 종속이어야 합니다. 이는 부분 함수 종속을 방지합니다.

3. **제3정규형(3NF):** 모든 속성이 주요 속성에 대해 이행적 함수 종속이 아니어야 합니다. 이는 이중 특성을 방지합니다.

4. **보이스-코드 정규형(BCNF):** 모든 결정자가 후보키이어야 합니다. 이는 다치 종속을 방지합니다.

5. **제4정규형(4NF):** 다치 종속이 없어야 합니다. 즉, 다치 종속 관계에서 발생할 수 있는 문제를 해결합니다.

6. **제5정규형(5NF):** 조인 종속이 없어야 합니다. 즉, 조인 연산을 통해 얻을 수 있는 정보는 이미 테이블에 저장되어 있어야 합니다.

이러한 정규화 단계를 통해 데이터 중복을 최소화하고 데이터의 일관성을 유지하며, 데이터베이스의 효율성을 향상시킬 수 있습니다.

**관계형 데이터베이스와 정규화의 관계:**

- **데이터 중복 감소:** 정규화를 통해 테이블을 분해하면서 중복 데이터를 최소화할 수 있습니다. 이는 저장 공간을 절약하고 데이터 일관성을 높이는 데 도움이 됩니다.

- **검색 및 갱신의 효율성 향상:** 정규화된 테이블은 작은 단위로 구성되어 있으므로 쿼리 실행 시에 데이터 검색과 갱신이 빠릅니다.

- **이상현상 방지:** 정규화는 이상현상을 방지하여 데이터의 일관성을 유지합니다. 이상현상이 발생하면 데이터의 무결성이 깨질 수 있습니다.

- **설계의 유연성:** 정규화를 통해 데이터를 더 작은 단위로 분해하면서 테이블 간의 관계를 명확하게 정의할 수 있습니다. 이는 데이터베이스 스키마의 유연성을 제공합니다.

그러나 지나치게 많은 정규화는 쿼리 성능을 떨어뜨릴 수 있으며, 실제 사용 사례에 맞게 적절한 정규화 수준을 선택하는 것이 중요합니다.

</details>