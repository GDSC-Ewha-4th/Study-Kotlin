# Kotlin in Action 11.
## 1. DSL의 구조
프로그램의 구성 요소를 설계할 때 고려해야 하는 기본적인 인터페이스 설계 원칙 중 하나는 커맨드(Command)와 쿼리(Query)를 분리하는 것이다.

“커맨드-쿼리 분리(Command-Query Separation, CQS)” 원칙의 기본 개념은 메서드는 반드시 부수 효과를 발생시키는 커맨드이거나 부수 효과를 발생시키지 않는 쿼리 둘 중 하나여야하고 두 가지 특성 모두를 가져서는 안 된다는 것이다.

즉, 커맨드 메서드는 상태를 변경하는 대신 값을 반환해서는 안되며 쿼리 메서드는 값을 반환화는 대신 상태를 변경해서는 안 된다.

**커맨드-쿼리 분리 원칙**은 객체들을 독립적인 기계로 보는 객체지향의 오랜 전통에 기인한다.

이 관점에서 객체는 블랙박스이며 객체의 인터페이스는 객체의 관찰 가능한 상태를 보기 위한 일련의 디스플레이와 객체의 상태를 변경하기 위해 누를 수 있는 버튼의 집합이다.

이런 스타일의 인터페이스를 사용함으로써 객체의 캡슐화와 다양한 문맥에서의 재사용을 보장할 수 있다.

## 2. DSL구조의 장점

외부 DSL로 생각해보면 메소드의 방식이 다 정리되어 있다. 범용언어로 생각하면 그것을 다 정의를 해주고 일련의 과정을 반복해야하는데 DSL을 사용하면 동일한 메소드에 관한 것은 같은 문맥을 사용하여 재사용을 줄일 수 있따.

---
# 과제!
## 3. 섹션 11.4.3. SQL을 위한 internal DSL - Exposed 살펴보기!

아쉽게도 Exposed는 레퍼런스 할 만한 자료들이 많지 않아서 QueryDSL도 함께 살펴보았다.

# Exposed 란 ?

Exposed는 JetBrains에서 만든 Kotlin 언어 기반의 ORM 프레임워크이다. Exposed는 두 가지 레벨의 데이터베이스 access를 제공한다.

SQL을 매핑 한 **DSL 방식** , 경량화한 DAO 방식을 제공하며 공식적으로 H2, MySQL, MariaDB, Oracle, PostgreSQL, SQL Server, SQLite 데이터베이스를 지원한다.

## Exposed 프로젝트에 도입하기
### MySQL
```
version: '3'

services:
    db_mysql:
        container_name: mysql.local
        image: mysql/mysql-server:5.7
        environment:
            MYSQL_ROOT_HOST: '%'
            MYSQL_DATABASE: 'exposed_study'
            MYSQL_ALLOW_EMPTY_PASSWORD: 'yes'
        ports:
            - '3366:3306'
        volumes:
            - './volumes/mysql/default:/var/lib/mysql'
        command:
            - 'mysqld'
            - '--character-set-server=utf8mb4'
            - '--collation-server=utf8mb4_unicode_ci'
            - '--sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'
1
```
```bash
$ docker-compose up -d
```

Exposed에서 지원해 주는 MySQL 기반으로 진행하기 위해서 해당 환경을 Docker로 구성한다.

그리고 Gradle 파일에 의존성을 추가해준다.
### Gradle
```
dependencies {
    implementation("mysql:mysql-connector-java")
    implementation("org.jetbrains.exposed:exposed-core:0.31.1")
    implementation("org.jetbrains.exposed:exposed-dao:0.31.1")
    implementation("org.jetbrains.exposed:exposed-jdbc:0.31.1")
    implementation("org.jetbrains.exposed:exposed-java-time:0.31.1")
}
```

### Config
```
class ExposedGettingStarted {
    private val config = HikariConfig().apply {
        jdbcUrl = "jdbc:mysql://localhost:3366/exposed_study?useSSL=false&serverTimezone=UTC&autoReconnect=true&rewriteBatchedStatements=true"
        driverClassName = "com.mysql.cj.jdbc.Driver"
        username = "root"
        password = ""
    }

    private val dataSource = HikariDataSource(config)
    ...
}
```
`HikariConfig`에 위에서 생성한 MySQL 정보를 입력하여 `DataSource`를 생성한다.

```
object Payments : LongIdTable(name = "payment") {
    val orderId = long("order_id")
    val amount = decimal("amount", 19, 4)
}

class ExposedGettingStarted {

    @Test
    fun `exposed DSL`() {
        // connection to MySQL
        Database.connect(dataSource)

        transaction {
            // Show SQL logging
            addLogger(StdOutSqlLogger)

            // CREATE TABLE IF NOT EXISTS payment (id BIGINT AUTO_INCREMENT PRIMARY KEY, order_id BIGINT NOT NULL, amount DECIMAL(19, 4) NOT NULL)
            SchemaUtils.create(Payments)

            // INSERT INTO payment (amount, order_id) VALUES (1, 1)
            // ...
            (1..5).map {
                Payments.insert { payments ->
                    payments[amount] = it.toBigDecimal()
                    payments[orderId] = it.toLong()
                }
            }

            // UPDATE payment SET amount=0 WHERE payment.amount >= 0
            Payments.update({ amount greaterEq BigDecimal.ZERO })
            {
                it[amount] = BigDecimal.ZERO
            }

            // SELECT payment.id, payment.order_id, payment.amount FROM payment WHERE payment.amount = 0
            // Payment(amount=1.0000, orderId=1)
            Payments.select { amount eq BigDecimal.ZERO }
                    .forEach { println(it) }

            // DELETE FROM payment WHERE payment.amount >= 1
            Payments.deleteWhere { amount greaterEq BigDecimal.ONE }

            // DROP TABLE IF EXISTS payment
            SchemaUtils.drop(Payments)
        }
    }
}
```

> 코드 설명
: Payments객체에 테이블 정보를 작성 하여 테이블을 생성하여 Insert, Select, Update, Delete를 하고 테이블을 Drop 한다.


SQL를 매핑한 DSL 방식으로 코틀린 코드 베이스로 SQL을 조작할 수 있다.


위는 DSL을 사용한 것인데 DSL은 QueryDSL 처럼 작동한다. 그런데 여기서는 컴파일러가 만들어주지 않고 QClass를 직접 선언해서 사용한다.


그리고 우리가 흔하게 사용하는 JPA와 비슷한 기능을 DAO Exposed에서 제공하는데, jpa와 달리 eager이 바로 join query 하나만 사용하는 것은 아니고, 메인 엔티티 조회 시 참조 객체를 가져오는 쿼리를 깥이 수행한다. 다건 조회의 경우에는 추가 in 절 쿼리를 수행하여 n+1을 방지한다.


Exposed의 깃허브는 [이 링크](https://github.com/JetBrains/Exposed)에서 확인해볼 수 있다.


## Querydsl은?

Spring Data JPA가 제공해주는 CRUD 메서드 및 쿼리 메서드 기능을 사용하더라도, 원하는 데이터를 수집하기 위해서는 필연적으로 JPQL을 작성하게 된다. 간단한 로직을 작성하는데 큰 문제는 없으나 복잡한 로직일 경우 개행이 포함된 쿼리 문자열이 상당히 길어진다.

이러한 문제를 어느정도 해소하는데 기여하는 프레임워크가 바로 **QueryDSL** 이다. 

QueryDSL의 장점은 다음과 같다.

1. 문자가 아닌 코드로 쿼리를 작성함으로써, 컴파일 시점에 문법 오류를 쉽게 확인할 수 있다.
2. 자동 완성 등 IDE의 도움을 받을 수 있다.
3. 동적인 쿼리 작성이 편리하다.
4. 쿼리 작성 시 제약 조건 등을 메서드 추출을 통해 재사용할 수 있다.

이렇게 DSL이 나의 관심분야에서 어떻게 쓰이는지 알아보았다.

이로써 Kotlin in Action 스터디 끝.
