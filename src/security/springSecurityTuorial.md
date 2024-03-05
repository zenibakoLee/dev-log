# Spring Security Tutorial

### 의존성 추가

build.gradle 파일에 회원 가입, 로그인 등을 처리하기 위한 의존성을 추가한다.

```yaml
// Spring Security
implementation 'org.springframework.boot:spring-boot-starter-security'

// TSID → Generate ID
implementation 'io.hypersistence:hypersistence-tsid:2.0.0'

// Bouncy Castle Provider → Argon2
implementation 'org.bouncycastle:bcprov-jdk15on:1.70'

// JWT
implementation 'com.auth0:java-jwt:4.4.0'
```

---

### Configuration

Spring Security 설정을 위해 `WebSecurityConfig` 클래스를 만든다.

```java

@Configuration
@EnableWebSecurity
@EnableMethodSecurity(securedEnabled = true)
public class WebSecurityConfig {
    private final AccessTokenAuthenticationFilter authenticationFilter; // 사용자 지정 인증 필터

    public WebSecurityConfig(
            AccessTokenAuthenticationFilter authenticationFilter) {
        this.authenticationFilter = authenticationFilter;
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http)
            throws Exception {
        http.cors();
        http.csrf().disable();

        http.addFilterBefore(
                authenticationFilter, BasicAuthenticationFilter.class);

        http.authorizeHttpRequests()
                .requestMatchers(HttpMethod.POST, "/session").permitAll()
                .requestMatchers(HttpMethod.POST, "/users").permitAll()
                .requestMatchers(HttpMethod.GET, "/categories").permitAll()
                .requestMatchers(HttpMethod.GET, "/products/**").permitAll()
                .requestMatchers(HttpMethod.GET, "/backdoor/**").permitAll()
                .anyRequest().authenticated();

        /*
        ~cors()' is deprecated and marked for removal
        Spring Security 6.1.0부터는 메서드 체이닝의 사용을 지양하고 람다식을 통해 함수형으로 설정하게 지향하고 있습니다.
         */
        http // HttpSecurity: Spring Security의 구성을 제공하는 클래스
                .csrf((csrfConfig) ->
                        csrfConfig.disable()) // CSRF(Cross-Site Request Forgery) 보호를 비활성화
                .addFilterBefore(authenticationFilter, BasicAuthenticationFilter.class) // 사용자 지정 인증 필터(authenticationFilter)를 기본 인증 필터(BasicAuthenticationFilter) 앞에 추가
                .authorizeHttpRequests((authorizeRequests) ->
                        authorizeRequests
                                .requestMatchers(HttpMethod.POST, "/session").permitAll()
                                .requestMatchers(HttpMethod.POST, "/users").permitAll()
                                .requestMatchers(HttpMethod.GET, "/categories").permitAll()
                                .requestMatchers(HttpMethod.GET, "/products/**").permitAll()
                                .requestMatchers(HttpMethod.GET, "/backdoor/**").permitAll()
                                .anyRequest().authenticated()
                );

        return http.build();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return Argon2PasswordEncoder.defaultsForSpringSecurity_v5_8();
    }
}
```

CORS를 위해 WebConfig 클래스도 만들자.

```java

@Configuration // 해당 클래스가 스프링 IoC 컨테이너에게 이 클래스가 Bean 구성을 제공하고 있음을 나타냅니다.
public class WebConfig implements WebMvcConfigurer { // WebMvcConfigurer 인터페이스를 구현하고 있으며, 이를 통해 Spring MVC의 구성을 커스터마이징할 수 있습니다.
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**") // 모든 경로에 대해서 CORS를 허용
                .allowedMethods("GET", "POST", "PATCH", "DELETE", "OPTIONS")
                .allowedOrigins("*"); //  모든 오리진(출처)에 대해 CORS를 허용
    }
}
```

### Filter

OncePerRequestFilter를 이용해 AccessTokenAuthenticationFilter 클래스를 만든다.

```java

@Component // 해당 클래스가 Spring의 컴포넌트로 등록되어 IoC 컨테이너에서 관리된다는 것을 나타냅니다. 즉, 이 클래스는 애플리케이션 컨텍스트에서 싱글톤으로 사용될 수 있습니다.
public class AccessTokenAuthenticationFilter extends OncePerRequestFilter { // Spring에서 제공하는 필터 중 하나로, 각 요청당 한 번만 실행되도록 보장합니다.
    private static final String AUTHORIZATION_PREFIX = "Bearer ";

    private final AccessTokenService accessTokenService;

    public AccessTokenAuthenticationFilter(
            AccessTokenService accessTokenService) {
        this.accessTokenService = accessTokenService;
    }

    /*
            실제 필터 동작을 정의합니다. HTTP 요청에서 액세스 토큰을 추출하고, 추출한 토큰을 사용하여 AccessTokenService를 통해 사용자를 인증합니다. 
            성공한 경우에는 SecurityContextHolder에 인증 객체를 설정하고, 그 후에는 다음 필터 체인으로 제어를 전달합니다.
     */
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {
        String accessToken = parseAccessToken(request);

        Authentication authentication =
                accessTokenService.authenticate(accessToken);

        SecurityContextHolder.getContext()
                .setAuthentication(authentication);

        filterChain.doFilter(request, response);
    }

    /*
            HTTP 요청의 헤더에서 Authorization 헤더를 추출하고, 헤더 값이 "Bearer "로 시작하는 경우에만 실제 액세스 토큰을 추출합니다.
     */
    private String parseAccessToken(HttpServletRequest request) {
        return Optional.ofNullable(request.getHeader("Authorization"))
                .filter(i -> i.startsWith(AUTHORIZATION_PREFIX))
                .map(i -> i.substring(AUTHORIZATION_PREFIX.length()))
                .orElse("");
    }
}
```

### DB Schema

```sql
CREATE TABLE users
(
    id         varchar(100) PRIMARY KEY,
    email      varchar(200) NOT NULL,
    name       varchar(100) NOT NULL,
    password   varchar(100) NOT NULL,
    role       varchar(50)  NOT NULL,
    created_at timestamp    NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp    NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE access_tokens
(
    token      varchar(200) PRIMARY KEY,
    user_id    varchar(100) NOT NULL,
    created_at timestamp    NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp    NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

---

### Services

Service라고 모두 Application Layer에 속하는 게 아니란 점에 주의!

`AccessTokenService` 클래스를 만든다.

```java

@Component
public class AccessTokenService {
    private final AccessTokenGenerator accessTokenGenerator;
    private final AuthUserDao authUserDao;

    public AccessTokenService(AccessTokenGenerator accessTokenGenerator,
                              AuthUserDao authUserDao) {
        this.accessTokenGenerator = accessTokenGenerator;
        this.authUserDao = authUserDao;
    }

    // 이 메서드는 주어진 액세스 토큰을 사용하여 사용자를 인증합니다.
    public Authentication authenticate(String accessToken) {
        if (!accessTokenGenerator.verify(accessToken)) {
            return null;
        }

        return authUserDao.findByAccessToken(accessToken)
                .map(authUser ->
                        UsernamePasswordAuthenticationToken.authenticated(
                                // Principal로 AuthUser 객체를 그대로 활용한다.
                                authUser, null, List.of(authUser::role)))
                .orElse(null);
    }
}
```

AccessTokenGenerator 클래스를 만든다.

```java

@Component
public class AccessTokenGenerator {
    private final Algorithm algorithm;

    public AccessTokenGenerator(
            @Value("${jwt.secret}")
            String secret
    ) {
        this.algorithm = Algorithm.HMAC256(secret);
    }

    public String generate(String userId) {
        return JWT.create()
                .withClaim("userId", userId)
                .withExpiresAt(Instant.now().plus(24, ChronoUnit.HOURS))
                .sign(algorithm);
    }

    public boolean verify(String accessToken) {
        try {
            JWTVerifier verifier = JWT.require(algorithm).build();
            verifier.verify(accessToken);
            return true;
        } catch (JWTVerificationException e) {
            return false;
        }
    }
}
```

### DB Access

`AuthUserDao` 클래스도 만들자. 원래는 하나씩 만들어야 하지만, 여기선 그냥 모든 코드를 다 넣겠다.

```java

@Component
public class AuthUserDao {
    private final JdbcTemplate jdbcTemplate;

    public AuthUserDao(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public Optional<AuthUser> findByEmail(String email) {
        String query = "SELECT id, password, role FROM users WHERE email=?";

        return jdbcTemplate.query(query, resultSet -> {
            if (!resultSet.next()) {
                return Optional.empty();
            }

            AuthUser authUser = AuthUser.of(
                    resultSet.getString("id"),
                    email,
                    resultSet.getString("password"),
                    resultSet.getString("role")
            );

            return Optional.of(authUser);
        }, email);
    }

    public void addAccessToken(String userId, String accessToken) {
        jdbcTemplate.update("""
                        INSERT INTO access_tokens (token, user_id)
                        VALUES (?, ?)
                        """,
                accessToken, userId
        );
    }

    public Optional<AuthUser> findByAccessToken(String accessToken) {
        String query = """
                SELECT users.id, users.role
                FROM users
                JOIN access_tokens ON access_tokens.user_id=users.id
                WHERE access_tokens.token=?
                """;

        return jdbcTemplate.query(query, resultSet -> {
            if (!resultSet.next()) {
                return Optional.empty();
            }

            AuthUser authUser = AuthUser.authenticated(
                    resultSet.getString("id"),
                    resultSet.getString("role"),
                    accessToken
            );

            return Optional.of(authUser);
        }, accessToken);
    }

    public void removeAccessToken(String accessToken) {
        jdbcTemplate.update("""
                        DELETE FROM access_tokens WHERE token=?
                        """,
                accessToken
        );
    }
}
```

UserDetails 대신 사용할 AuthUser 클래스를 만든다.

```java
public record AuthUser(
        String id,
        String email,
        String password,
        String role,
        String accessToken
) {
    public static AuthUser of(
            String id, String email, String password, String role) {
        return new AuthUser(id, email, password, role, "");
    }

    public static AuthUser authenticated(
            String id, String role, String accessToken) {
        return new AuthUser(id, "", "", role, accessToken);
    }
}
```