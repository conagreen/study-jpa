# 자바 ORM 표준 JPA 프로그래밍 - 기본편

**[ 이 강의를 듣고 난 후 스스로에게 기대하는 점 ]**

- 스프링 데이터 JPA가 아닌, JPA 자체의 기본기와 내부 동작방식을 이해할 수 있다.
- 평소 프로젝트를 진행하며 애매하게 알고있던 연관관계 관리를 명확하게 알 수 있다.
- 객체와 DB 테이블을 유연하게 설계하고 매핑할 수 있다.

## JPA란?
- **J**ava **P**ersistence **A**PI
- 자바 진영의 **ORM** 기술 표준
  - **그렇다면 ORM이란?**
    - Object-relational mapping (객체 관계 매핑)
    - ORM 프레임워크가 중간에서 매핑
    - 객체는 객체대로 / 관계형 데이터베이스는 관계형 데이터베이스대로 설계 가능
- JPA는 인터페이스의 모음
  - JPA 2.1 표준 명세를 구현한 3가지 구현체
    - 하이버네이트
    - EclipseLink
    - DataNucleus

#### [ 왜 JPA를 사용해야 하는가? ]
- **SQL 중심적인 개발에서 객체 중심으로 개발**
- **생산성**
  - 저장: **jpa.persist**(member)
  - 조회: Member member = **jpa.find**(memberId)
  - 수정: **member.setName**("변경할 이름")
  - 삭제: **jpa.remove**(member)
- **유지보수**
  - 기존: 필드 변경 시 모든 SQL 수정해야 함
  - JPA: 필드만 추가하면 됨. SQL은 JPA가 처리
- **패러다임의 불일치 해결**
  - 상속: JPA가 필요한 쿼리를 생성해서 날려줌
  - 연관관계: JPA가 필요한 쿼리를 생성해서 날려줌
  - 객체 그래프 탐색 가능
  - 동일한 트랜잭션에서 조회한 엔티티는 같음을 보장
- **성능 최적화 기능**
  - 1차 캐시와 동일성(identity) 보장 - 약간의 조회 성능 향상
  - 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)
  - 지연 로딩과 즉시로딩 설정 가능
    - 지연 로딩: 객체가 실제 사용될 때 로딩
    - 즉시 로딩: JOIN SQL로 한번엔 연관된 객체까지 미리 조회
- **데이터 접근 추상화와 벤더 독립성**
- **표준**

<br>

### JPA 시작하기

#### [ 데이터베이스 방언 ]
- JPA는 특정 데이터베이스에 종속적이지 않다.
- 각각의 데이터베이스가 제공하는 SQL 문법과 함수는 조금씩 다르다.
  - 가변 문자: MySQL은 VARCHAR, Oracle은 VARCHAR2
  - 문자열을 자르는 함수: SQL 표준은 SUBSTRING(), Orcle은 SUBSTR()
  - 페이징: MySQL은 LIMIT, Oracle은 ROWNUM
- 방언: SQL 표준을 지키지 않는 특정 데이터베이스만의 고유한 기능
- **hibernate.dialect** 속성에 지정
  - H2: org.hibernate.dialect.H2Dialect
  - Oracle 10g: org.hibernate.dialect.Oracle10gDialect
  - MySQL: org.hibernate.dialect.MySQL5InnoDBDialect
- 하이버네이트는 40가지 이상의 데이터베이스 방언 지원

#### [ 관련 애노테이션 ]
- **@Entity**: JPA가 관리할 객체
- **@Id**: 데이터베이스 PK와 매핑

#### [ 주의 ]
- **엔티티 매니저 팩토리**는 하나만 생성해서 애플리케이션 전체에서 공유
- **엔티티 매니저**는 쓰레드간에 공유X (사용하고 버려야 함)
- **JPA의 모든 데이터 변경은 트랜잭션 안에서 실행**

<br>

### JPQL
- JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어 제공
- SQL과 문법 유사 -> SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN 지원
- **JPQL은 엔티티 객체**를 대상으로 쿼리 -> 객체 지향 SQL
- **SQL은 데이터베이스 테이블을 대상으로 쿼리**
- 테이블이 아닌 **객체를 대상으로 검색하는 객체 지향 쿼리**
- SQL을 추상화해서 특정 데이터베이스 SQL에 의존 X
- 가장 단순한 조회 방법
  - EntityManager.find()
  - 객체 그래프 탐색(a.getB().getC())
- **JPQL은 뒤에서 자세히 다룸**

#### [ JPQL을 사용하는 이유 ]
JPA를 사용하면 객체를 중심으로 개발할 수 있다는 장점이 있지만, 문제는 검색 쿼리이다.
검색을 할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색한다. 모든 DB 데이터를 객체로 변환해서 검색하는 것은 불가능하기 때문에,
애플리케이션이 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된 SQL이 필요하다.























