#### Introduction

Spring Security is a convention over configuration framework and the default configuration for a simple project is the HTTP Basic access authentication where the client is only required to send a username and password through the HTTP Authorization header, with the prefix Basic and Base64 encoded string of username and password.

When you add the Spring Security dependency to your Spring Boot application, it automagically adds Spring Security Servlet Filters Chain to your Spring Boot application in the default configuration. However, if you are not using Spring Boot you would have to add the filter to your web.xml file manually. The Spring Security filters will work on the wildcard path "/*" and intercept all requests. The starting filter is called the Delegating Filter Proxy and this filter delegates the different features to different filters in the filter chain. One of the filter is called the Authentication filter that intercepts all authentication requests and initiates authentication process and there are also Authorization related filters.

To create the most simple bare bones Spring Security project, we start by importing the following dependencies Spring Web, Spring Dev Tools and Spring Security.

We then create a simple endpoint to test out the default security configurations provided :

src.main.java.com.reku.controller - HelloController.java

```
@RestController
public class HelloController {
    
    @GetMapping("/hello")
    public String hello() {
        return "Hello";
    }
}
```

With this the endpoint http://localhost:8080/hello is protected with basic authentication unless we log in with the password provided in the console logs and the default username “user” by using the below command in the command line :

```
curl –u user:password http://localhost:8080/hello
```

Which is shortcut for the below :

```
echo –n user:password | base64
< Base64 String is returned >
curl –H “Authorization: Basic ” localhost:8080/hello
```

In the Authentication process, the Authentication object holds both the credentials(input) of the users and once the user is authenticated it holds the principal(output) which contains information about the logged in user.

An AuthenticationProvider is the interface that has a method called authenticate() and implementation of this interface and method in our application. Spring Security will put the passed in credentials into an Authentication object and use this method to check the credentials passed in. The authenticate method will then return an Authentication that will hold information of the principal.

An application can have multiple AuthenticationProviders with each one having different authentcation methods e.g. Basic Authentication, LDAP Authentication. In order to coordinate these Providers, we use the AuthenticationManager. In order for the AuthenticationManager to find the correct AuthenticationManager, it calls the supports() method of the AuthenticationProvider.

AuthenticationProviders will require a way to retrieve user information the identity store(be it a database, LDAP store) which may be different from Provider to Provider even though the steps of verifying it after retrieving it is the same. Hence, Spring Security has extracted this out into the UserDetailsService which has a method called loadUserByUserName() which takes in a username and returns an object called UserDetails.


In case we want to protect our endpoints with SSL/TLS, we can generate our own self-signed certificate :

```
openssl req –newkey rsa:2048 –x509 –keyout key.pem –out cert.pem –days 365
```

This will output key.pem (private key) and cert.pem (public certificate). We will then use these files to further generate self-signed certification. We can use JKS or PKCS12. We will use PKCS12 :

```
openssl pkcs12 –export –in  cert.pem –inkey key.pem –out certificate.p12 –name “certificate”
```

With the outputted self-signed certificate, we can configure HTTPS for the endpoints in application.properties :

```
server.ssl.key-store-type=PKCS12
server.ssl.key-store=classpath:certificate.p12
server.ssl.key-store-password=password
```