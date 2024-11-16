# Introduction to Spring Security

Spring Security is a **convention-over-configuration** framework designed to handle authentication and authorization in Java applications. By default, it uses **HTTP Basic Authentication**, where the client sends a `username` and `password` in the `Authorization` header, prefixed with `Basic`, followed by a Base64-encoded string of the credentials.

## How Spring Security Works

When you add the Spring Security dependency to a **Spring Boot** application, it automatically configures a **Servlet Filter Chain** to intercept and secure requests. The filter chain includes several filters for authentication and authorization. 

In non-Spring Boot projects, the security filter must be manually added to the `web.xml` file. The key filter, **Delegating Filter Proxy**, delegates various security tasks to specialized filters in the chain. Two essential filters include:

1. **Authentication Filter**: Handles authentication requests.
2. **Authorization Filters**: Manage access control for protected resources.

---

## Setting Up a Basic Spring Security Application

### Dependencies

To create a minimal Spring Security project, add the following dependencies:

- **Spring Web**
- **Spring DevTools**
- **Spring Security**

### Create a Simple Secured Endpoint

Here’s a basic REST endpoint to test default security configurations:

**`HelloController.java`**  

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello";
    }
}
```

By default, accessing `http://localhost:8080/hello` requires authentication. Spring Security protects this endpoint using **Basic Authentication**. Use the credentials provided in the console logs:

- **Username**: `user`
- **Password**: Automatically generated (visible in logs).

Test using the following `curl` command:

```bash
curl -u user:password http://localhost:8080/hello
```

This is equivalent to:

1. Generating the Base64 string for credentials:

    ```bash
    echo -n user:password | base64
    ```

2. Sending the `Authorization` header:

    ```bash
    curl -H "Authorization: Basic <Base64-encoded string>" http://localhost:8080/hello
    ```

---

## Authentication and Authorization Process

### Authentication Flow

During authentication, the **Authentication** object plays a key role:

1. **Input**: Contains user credentials (e.g., username and password).
2. **Output**: After authentication, contains user details (`Principal`), representing the authenticated user.

---

### AuthenticationProvider

The `AuthenticationProvider` interface defines the method:

```java
Authentication authenticate(Authentication authentication);
```

- The implementation checks credentials and returns an `Authentication` object holding the `Principal`.
- An application can use multiple `AuthenticationProviders` for various methods (e.g., Basic Auth, LDAP).

To determine the correct provider, the `AuthenticationManager` invokes the `supports()` method of each `AuthenticationProvider`.

---

### UserDetailsService

To retrieve user information from an identity store (e.g., database, LDAP), Spring Security uses `UserDetailsService`. It defines the method:

```java
UserDetails loadUserByUsername(String username);
```

This method returns a `UserDetails` object containing the user’s credentials and roles.

---

## Enabling SSL/TLS for Secure Endpoints

### Generating a Self-Signed Certificate

You can secure your application with HTTPS by generating a self-signed certificate:

1. Generate the private key and public certificate:

    ```bash
    openssl req -newkey rsa:2048 -x509 -keyout key.pem -out cert.pem -days 365
    ```

2. Create a **PKCS12** keystore from the certificate:

    ```bash
    openssl pkcs12 -export -in cert.pem -inkey key.pem -out certificate.p12 -name "certificate"
    ```

### Configuring HTTPS in Spring Boot

Add the following properties in `application.properties` to enable HTTPS:

```properties
server.ssl.key-store-type=PKCS12
server.ssl.key-store=classpath:certificate.p12
server.ssl.key-store-password=password
```

With this configuration, Spring Boot will use the self-signed certificate to secure endpoints over HTTPS.