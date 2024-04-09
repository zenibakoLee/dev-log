# Modules

<details>

<summary>JDBC</summary>

#### JDBC (Java Database Connectivity)

Java에서 RDBMS를 사용할 수 있게 해주는 API.

[JDBC](https://ko.wikipedia.org/wiki/JDBC)

[JDBC driver](https://en.wikipedia.org/wiki/JDBC\_driver)

[JDBC Basics](https://docs.oracle.com/javase/tutorial/jdbc/basics/index.html)

API는 그냥 인터페이스기 때문에, 각 DBMS 벤더에서 제공하는 JDBC Driver가 있어야 실제로 사용할 수 있다.

#### Connection

[Establishing a Connection](https://docs.oracle.com/javase/tutorial/jdbc/basics/connecting.html)

```java
String url="jdbc:postgresql://localhost:5432/postgres";

        Properties properties=new Properties();
        properties.put("user","postgres");
        properties.put("password","password");

        Connection connection=DriverManager.getConnection(url,properties);
```

“No suitable driver found for jdbc:postgresql://localhost:5432/postgres” 에러

→ PostgreSQL용 JDBC Driver가 필요함.

* [pgJDBC](https://jdbc.postgresql.org/)
* [Maven Repository](https://mvnrepository.com/artifact/org.postgresql/postgresql)

`build.gradle` 파일에 의존성 추가. 버전은 최신 버전으로 맞출 것.

```
implementation 'org.postgresql:postgresql:42.5.4'
```

#### Statement

[Processing SQL Statements with JDBC](https://docs.oracle.com/javase/tutorial/jdbc/basics/processingsqlstatements.html)

```java
Statement statement=connection.createStatement();

        String query="SELECT * FROM people";

        ResultSet resultSet=statement.executeQuery(query);

        while(resultSet.next()){
        String name=resultSet.getString("name");

        System.out.println(name);
        }
```

#### PreparedStatement

[Using Prepared Statements](https://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html)

* [기본 중 기본인 SQL Injection 공격](https://ko.wikipedia.org/wiki/SQL\_%EC%82%BD%EC%9E%85)
* [Exploits of a Mom](https://xkcd.com/327/)
* [국내 사례](https://www.google.com/search?q=%EB%BD%90%EB%BF%8C+SQL+Injection)

</details>
