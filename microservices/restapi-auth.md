## REST API Authentication Methods

The image displays four common methods for authenticating REST API requests: Token Authentication, OAuth Authentication, API Key Authentication, and Basic Authentication.

**1. Token Authentication:**

* Utilizes generated tokens, like JSON Web Tokens (JWT), exchanged between client and server.
* **Workflow:**
    * An end-user initiates a login request to a client application.
    * The client application sends a user authentication request (e.g., username/password) to an API Gateway.
    * The API Gateway authenticates the user and acts as a JWT issuer, generating a JSON Web Token.
    * The client application receives and stores the JSON Web Token.
    * For subsequent requests to a protected resource, the client application includes the JSON Web Token in the request headers.
    * The API Gateway with JWT authentication verifies the token before allowing access to the protected resource.

**2. OAuth Authentication:**

* Enables third-party limited access to user resources without exposing credentials.
* **Workflow:**
    * A client application requests authorization from a user to access their resources on a service.
    * The user grants authorization to the client application.
    * The client application receives an authorization grant.
    * The client application exchanges the authorization grant for an access token from an authorization server.
    * The client application uses the access token to request the protected resource from the service API.
    * The service API validates the access token with the authorization server before granting access to the protected resource.

**3. API Key Authentication:**

* Assigns unique keys to users or applications, sent in headers or parameters with each request.
* **Workflow:**
    * A client application makes a request to an API server, including its assigned API key (either in the header or as a parameter).
    * The API server receives the request and verifies the API key against a database of valid keys.
    * If the API key is valid, the server processes the request and returns the requested resource.
    * If the API key is invalid, the server returns a 401 Unauthorized error.

**4. Basic Authentication:**

* Involves sending a username and password with each request.
* **Workflow:**
    * A client application sends a request to a server that requires authentication.
    * The server responds requesting authentication.
    * The client application resends the request, including the username and password (typically Base64 encoded) in the "Authorization" header.
    * The server receives the request, decodes the credentials, and authenticates the user.
    * If the credentials are valid, the server returns the requested resource.
 
---

![image](https://github.com/user-attachments/assets/6246be5e-c0b9-4300-a014-25df539f8dd6)
