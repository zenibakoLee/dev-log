# Java DAO (Data Access Object)

DAO (Data Access Object)는 데이터베이스와 상호작용하고 데이터베이스 연산을 추상화하는 객체지향 디자인 패턴입니다.

DAO는 데이터베이스와의 통신을 캡슐화하고 비즈니스 로직에서 데이터 액세스 로직을 분리함으로써 데이터베이스와의 결합도를 낮추는 역할을 합니다. 주요 특징은 다음과 같습니다:

1. **데이터 액세스 추상화**: DAO는 데이터베이스와의 통신을 추상화하고 데이터베이스와 직접 상호작용하는 코드를 비즈니스 로직으로부터 분리합니다. 이를 통해 데이터 액세스 로직을 더 효율적으로 관리할 수
   있습니다.

2. **CRUD 작업**: DAO는 주로 CRUD (Create, Read, Update, Delete) 작업을 수행합니다. 즉, 데이터를 생성, 조회, 업데이트 및 삭제하는 메서드를 제공합니다.

3. **트랜잭션 관리**: DAO는 데이터베이스 트랜잭션을 관리할 수 있으며, 데이터베이스 연산의 일관성과 안정성을 보장합니다. 예외 발생 시 롤백 등의 트랜잭션 관리 기능을 제공합니다.

4. **재사용성**: DAO는 데이터 액세스 로직을 재사용 가능한 컴포넌트로 만듭니다. 이로써 같은 데이터베이스 테이블에 대한 다양한 애플리케이션 부분에서 동일한 데이터 액세스 로직을 사용할 수 있습니다.

5. **데이터베이스 독립성**: DAO는 데이터베이스 종속성을 최소화하며, 데이터베이스가 변경되어도 애플리케이션 코드를 수정할 필요가 없도록 합니다.

6. **설계 원칙 준수**: DAO는 SOLID 원칙과 같은 객체지향 설계 원칙을 준수하고, 코드의 가독성과 유지보수성을 향상시킵니다.

일반적으로 DAO는 특정 데이터베이스 테이블 또는 엔터티에 대한 클래스로 구현되며, 해당 데이터베이스 테이블에 대한 데이터 액세스 로직을 제공합니다. 예를 들어, "UserDAO" 클래스는 "User" 엔터티와
상호작용하고, 사용자 데이터베이스 테이블에 대한 CRUD 작업을 제공할 수 있습니다. 이를 통해 데이터베이스와의 상호작용을 추상화하고 코드의 모듈성을 향상시킵니다.

```java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class UserDAO {
    private Connection connection; // 데이터베이스 연결

    public UserDAO(Connection connection) {
        this.connection = connection;
    }

    // 새로운 사용자 생성
    public void createUser(User user) {
        String insertQuery = "INSERT INTO users (username, email) VALUES (?, ?)";
        try (PreparedStatement preparedStatement = connection.prepareStatement(insertQuery)) {
            preparedStatement.setString(1, user.getUsername());
            preparedStatement.setString(2, user.getEmail());
            preparedStatement.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    // ID에 따른 사용자 정보 조회
    public User getUserById(int userId) {
        String selectQuery = "SELECT * FROM users WHERE id = ?";
        try (PreparedStatement preparedStatement = connection.prepareStatement(selectQuery)) {
            preparedStatement.setInt(1, userId);
            ResultSet resultSet = preparedStatement.executeQuery();
            if (resultSet.next()) {
                String username = resultSet.getString("username");
                String email = resultSet.getString("email");
                return new User(userId, username, email);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }

    // 사용자 정보 업데이트
    public void updateUser(User user) {
        String updateQuery = "UPDATE users SET username = ?, email = ? WHERE id = ?";
        try (PreparedStatement preparedStatement = connection.prepareStatement(updateQuery)) {
            preparedStatement.setString(1, user.getUsername());
            preparedStatement.setString(2, user.getEmail());
            preparedStatement.setInt(3, user.getId());
            preparedStatement.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    // ID에 따른 사용자 삭제
    public void deleteUser(int userId) {
        String deleteQuery = "DELETE FROM users WHERE id = ?";
        try (PreparedStatement preparedStatement = connection.prepareStatement(deleteQuery)) {
            preparedStatement.setInt(1, userId);
            preparedStatement.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```

예제 코드에서 UserDAO는 사용자 레코드를 생성, 조회, 업데이트 및 삭제하는 메서드를 제공합니다.

데이터베이스 상호작용을 위한 Connection 객체를 사용합니다.

각 메서드는 SQL 쿼리를 준비하고 필요한 경우 매개변수를 설정한 다음 쿼리를 실행하여 해당 데이터베이스 작업을 수행합니다.

이 예제는 데이터베이스 통신을 위해 JDBC를 사용하며, 데이터베이스 스키마와 테이블 이름은 가상의 것입니다.

실제 애플리케이션에서는 데이터베이스 연결을 설정하고 User 클래스를 실제 요구 사항에 따라 정의해야 합니다.