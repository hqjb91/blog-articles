# OAuth 2.0

OAuth 2.0 is a widely used protocol for authorization, enabling applications to securely access resources on behalf of a user. This article outlines the main components, grant types, and a practical implementation example using GitHub and Spring Boot.

## Components of OAuth 2.0

An OAuth 2.0 implementation involves three key components:

### Client
- Uses `client_id` and `client_secret` to identify itself to an authorization server.
- Obtains resources from the resource server.

### 2. Authorization Server
- Issues a `token` to the client after verifying its credentials.

### 3. Resource Server
- Validates the `token` and provides resources if authorized.

---

## Grant Types in OAuth 2.0

OAuth 2.0 supports four main grant types, each suited for different scenarios:

### Authorization Code Grant Type
**Typical Usage**: Web applications (e.g., using Google login for your application).  
**Flow**:
1. Client redirects the user to the authorization server's endpoint, passing:
   - `response_type=code`, `client_id`, `redirect_uri`, `scope`, and `state` (to prevent CSRF).
2. User logs in.
3. Authorization server redirects the user back to the client with:
   - An `authorization code` and `state`.
4. Client validates the state and exchanges the `authorization code` for an access token:
   - Sends `code`, `client_id`, `client_secret`, `redirect_uri`, and `grant_type=authorization_code`.
5. Authorization server responds with an `access_token`.
6. Client uses the `access_token` to access resources.
7. **Token Validation**:
   - Authorization server encrypts the token using a private key.
   - Resource server decrypts and validates the token using a public key.

---

### Password Grant Type
**Typical Usage**: Deprecated; used when the client and authorization server are maintained by the same organization.  
**Flow**:
1. Client requests an access token with:
   - `grant_type=password`, `client_id`, `client_secret`, `username`, and `password`.
2. Authorization server responds with an access token.

---

### Client Credentials Grant Type
**Typical Usage**: Application-to-application communication (no user involved).  
**Flow**:
1. Client requests an access token with:
   - `grant_type=client_credentials`, `client_id`, `client_secret`, and `scope`.
2. Authorization server responds with an access token.

---

### Refresh Token Grant Type
Allows clients to obtain a new access token using a valid refresh token.

---

## Implementing OAuth 2.0: A Practical Example

### Using GitHub as the Authorization Server and Spring Boot as the Client

**Step 1**: Register the application with GitHub:
- Add a new OAuth application on GitHub to get the `client_id` and `client_secret`.

**Step 2**: Add dependencies in `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**Step 3**: Create a test endpoint:

```java
@Controller
public class MainController {
    @GetMapping("/")
    public String main() {
        return "main.html";
    }
}
```

**Step 4**: Configure security with `oauth2Login`:

```java
@Configuration
public class ProjectConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.oauth2Login();
        http.authorizeRequests().anyRequest().authenticated();
    }
}
```

**Step 5**: Link the OAuth client to the authorization server:  
Define a `ClientRegistration` object:

```java
ClientRegistration cr = ClientRegistration.withRegistrationId("github")
    .clientId("")
    .clientSecret("")
    .scope("read:user")
    .authorizationUri("https://github.com/login/oauth/authorize")
    .tokenUri("https://github.com/login/oauth/access_token")
    .userInfoUri("https://api.github.com/user")
    .userNameAttributeName("id")
    .clientName("GitHub")
    .authorizationGrantType(AuthorizationGrantType.AUTHORIZATION_CODE)
    .redirectUriTemplate("{baseUrl}/{action}/oauth2/code/{registrationId}")
    .build();
```

Or, use a prebuilt provider for GitHub:

```java
ClientRegistration cr = CommonOAuth2Provider.GITHUB
    .getBuilder("github")
    .clientId("")
    .clientSecret("")
    .build();
```

**Step 6**: Register the `ClientRegistration`:
1. As a `ClientRegistrationRepository` bean.
2. Using a `Customizer`.
3. Via `application.properties`.

**Step 7**: Access user details from the `OAuth2AuthenticationToken`.