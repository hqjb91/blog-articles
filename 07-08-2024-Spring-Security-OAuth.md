#### OAuth 2.0

There are three main components of an OAuth 2.0 implementation :
- Client
	- Uses `client id` and `client secret` to identify itself to an authorization server and obtain resources from resource server
- Authorization Server
	- Issues a `token` to client after the client identifies itself
- Resource Server
	- Receives a `token` from the client and sends the resource if it is authorized

There are four main grant types of an OAuth 2.0 implementation :
- Authorization Code Grant Type
	- Typical Usage : Web applications (e.g. using Google account to log web users into your application)
	- Flow :
		- Client redirects user to endpoint of authorization server to login with the following details (client credentials) in request query
			- `response_type=code`, `client_id`, `redirect_uri`, `scope`, `state(CSRF token)`
		- User will then enter their user credentials
		- If authentication was successful, the authorization server calls back on the client at the redirect_uri with
			- an `authorization code` and `state` value
		- Client checks that the state value is the same as the request to confirm that it is receiving the response from the authorization server
		- Client uses the authorization code returned to obtain an access token
			- Client sends another request to the authorization server with
				- `code=the authorization code`, `client_id`, `client_secret`, `redirect_uri`, `grant_type=authorization_code`
		- Server sends back an access_token
		- Client can now use the access_token to get resource from the resource server
		- Resource server can validate the access_token via the following steps :
			- Authorization Server can encrpt the token using private key and then public key/cert given to Resource Server
			- When resource server gets the token, it can then decrypt/verify signature to verify the token
- Password Grant Type
	- Typical Usage : Outdated and only used if client and authorization server are built and maintained by the same organization
	- Flow :
		- Client directly requests for access token with the following details
			- `grant_type=password`, `client_id`, `client_secret`, `scope`, `username`, `password`
		- Authorization server sends back an access token which can be used on resource server
- Client Credentials Grant Type
	- Typical Usage : Between two applications with no user involved i.e. no username/passage
	- Flow :
		- Client directly requests for access token with the following details
			- `grant_type=client_credentials`, `client_id`,  `client_secret`, `scope`
		- Authorization server sends back an access token which can be used on resource server
- Refresh Token Grant Type

Simple SSO (Single Sign On) example of a client for `authorization code flow` with `GitHub` as both the `authorization and resource server` and the `Spring Boot application` as the `client`.

1. First, we register our application with GitHub by filling in the form to add a new OAuth application on GitHub - we will obtain the `client id` and `secret` in return
2. Next, we add in the dependencies for OAuth and Spring Security to the pom.xml

```
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

3. We then add in a dummy endpoint to secure for testing

```
@Controller
public class MainController {

	@GetMapping("/")
	public String main() {
		return "main.html";
	}
}
```

4. We then set the security configuration

We first write a configuration class that extends the WebSecurityConfigurerAdapter class and override the `configure(HttpSecurity http)` method.

We then call a new method on the HttpSecurity object, the `oauth2Login()` method which adds in a new authentication filter to the filter chain which will intercept requests and apply the needed logic for OAuth2 authentication.

```
@Configuration
public class ProjectConfig extends WebSecurityConfigurerAdapter {

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http.oauth2Login();

		http.authorizeRequests()
			.anyRequests()
			.authenticated();
	}
}
```

5. Next, we have to implement the link between the OAuth 2 client and authorization server. 

We do this by defining the ClientRegistration contract :

```
ClientRegistration cr =
	ClientRegistration.withRegistration("github")
		.clientId("")
		.clientSecret("")
		.scope(new String[]{"read:user"})
	    .authorizationUri("https://github.com/login/oauth/authorize")
		.tokenUri("https://github.com/login/oauth/access_token")
		.userInfoUri("https://api.github.com/user")
		.userNameAttributeName("id")
		.clientName("GitHub")
		.authorizationGrantType(AuthorizationGrantType.AUTHORIZATION_CODE)
		.redirectUriTemplate("{baseUrl}/{action}/oauth2/code/{registrationId}")
		.build();
```

We can use a CommonOAuth2Provider class as shortcut since Github is a well-known Authorization server :

```
ClientRegistration cr = 
   CommonOAuth2Provider.GITHUB
     .getBuilder("github")
       .clientId("")
       .clientSecret("")
       .build();  
```

6. There are three ways we can register this ClientRegistration object for Spring to use :
	1. First is to add ClientRegistrationRepository as a bean
	2. Second is to use Customizer to set ClientRegistrationRepository
	3. Easiest way is to use Spring Boot configuration to automagically create and build ClientRegistration and ClientRegistrationRepository objects directly from application.properties
7. We can then obtain user details from the Authentication object is which OAuth2AuthenticationToken