# OAuth 2.0

OAuth 2.0은 사용자의 인증 및 권한 부여를 위한 표준 프로토콜 중 하나로, 웹 및 모바일 애플리케이션을 위한 강력한 인증 및 권한 부여 메커니즘을 제공합니다. 주로 서드파티 애플리케이션이 사용자의 리소스에
안전하게 액세스할 수 있도록 하는 데 사용됩니다.

OAuth 2.0의 주요 구성 요소와 개념은 다음과 같습니다:

1. **클라이언트 (Client):**
    - OAuth 2.0를 사용하여 사용자의 리소스에 접근하려는 애플리케이션 또는 서비스입니다.
    - 클라이언트는 사용자의 권한을 얻기 위해 인증되어야 합니다.

2. **리소스 소유자 (Resource Owner):**
    - 리소스에 액세스 권한이 있는 사용자입니다. 이는 일반적으로 엔드 유저 또는 소유자의 역할을 합니다.

3. **인증 서버 (Authorization Server):**
    - 사용자의 인증 및 권한 부여를 처리하는 서버입니다.
    - 인증 서버는 클라이언트에게 액세스 토큰과 리프레시 토큰을 발급합니다.

4. **리소스 서버 (Resource Server):**
    - 사용자의 리소스를 호스팅하고 보호하는 서버입니다.
    - 클라이언트는 액세스 토큰을 사용하여 리소스 서버에 요청을 보내고, 인증 및 권한을 확인합니다.

5. **액세스 토큰 (Access Token):**
    - 클라이언트가 리소스 서버에 접근할 수 있는 토큰으로, 인증 서버에 의해 발급됩니다.
    - 토큰은 제한된 유효 기간 동안 유효하며, 클라이언트는 이를 사용하여 리소스에 액세스합니다.

6. **리프레시 토큰 (Refresh Token):**
    - 액세스 토큰의 만료 시간이 지난 경우, 리프레시 토큰을 사용하여 새로운 액세스 토큰을 얻을 수 있습니다.
    - 리프레시 토큰은 보안적으로 더 강화되어 있으며, 사용자의 비밀번호를 다시 입력하지 않고도 새로운 액세스 토큰을 얻을 수 있게 합니다.

OAuth 2.0은 클라이언트와 리소스 소유자 간의 권한 부여를 중앙화된 인증 서버를 통해 처리하므로, 클라이언트는 사용자의 암호를 직접 처리하지 않아도 됩니다. 이로써 보안적인 측면에서 유리하며, 서드파티
애플리케이션이 더욱 안전하게 사용자 데이터에 액세스할 수 있게 됩니다.

---

### Bearer Token

Bearer Token은 OAuth 2.0 및 OpenID Connect 프로토콜에서 사용되는 토큰 중 하나입니다. Bearer Token은 간단한 문자열로, 클라이언트가 리소스 서버에 인증 및 권한 부여를 요청할
때 사용됩니다. 이 토큰은 보통 액세스 토큰 또는 ID 토큰의 형태를 가지며, "Bearer" 스키마와 함께 HTTP 요청 헤더에 포함됩니다.

Bearer Token의 특징과 사용 방법은 다음과 같습니다:

1. **HTTP 헤더에 포함:**
    - Bearer Token은 주로 HTTP 요청의 Authorization 헤더에 "Bearer" 스키마와 함께 포함됩니다.
    -
   예: `Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`

2. **Stateless 특성:**
    - Bearer Token은 서버 측에서 상태를 유지하지 않고, 각 요청마다 토큰을 확인합니다.
    - 서버는 토큰 자체로 사용자의 권한을 인증하며, 이를 통해 상태를 저장하지 않고도 효율적으로 인증을 수행할 수 있습니다.

3. **보안성 주의:**
    - Bearer Token은 암호화된 문자열이지만, 중간자 공격에 노출될 경우 토큰이 빼앗길 수 있습니다.
    - HTTPS를 사용하여 통신을 암호화하는 것이 보안성을 향상시키는 중요한 요소입니다.

4. **헤더 예시:**
    - HTTP 요청 헤더에 Bearer Token을 포함하는 예시:
      ```
      GET /api/resource HTTP/1.1
      Host: example.com
      Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ...
      ```

5. **액세스 토큰 및 ID 토큰:**
    - 주로 OAuth 2.0의 액세스 토큰이나 OpenID Connect의 ID 토큰이 Bearer Token으로 사용됩니다.

Bearer Token은 간단하면서도 효과적인 방법으로 인증 및 권한을 처리합니다. 그러나 중요한 것은 토큰이 안전하게 관리되고 전송되어야 하며, 주로 HTTPS를 통한 안전한 통신이 권장됩니다.