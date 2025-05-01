A JSON Web Token (JWT) is a compact, URL-safe means of representing claims to be transferred between two parties. These claims are encoded as a JSON object that is digitally signed using a JSON Web Signature (JWS). JWTs are commonly used for authentication and authorization in web applications and APIs.

Here's a breakdown of how JWT tokens work:

### 1. Structure of a JWT
A JWT consists of three parts, separated by dots (`.`):

* **Header:** Contains metadata about the token, such as the type of token (JWT) and the signing algorithm being used (e.g., HMAC SHA256, RSA). This header is Base64Url encoded.
* **Payload:** Contains the claims, which are statements about an entity (typically the user) and any additional data. There are three types of claims:
    * **Registered claims:** Predefined claims like `iss` (issuer), `exp` (expiration time), `sub` (subject), `aud` (audience), `iat` (issued at), and `nbf` (not before).
    * **Public claims:** Custom claims that can be defined by users of JWTs. To avoid collisions, these should be defined in the IANA JSON Web Token Registry or as a URI with a collision-resistant namespace.
    * **Private claims:** Custom claims created to share information between parties that agree on their use.
    The payload is also Base64Url encoded. **It's important to note that while the payload is encoded, it is not encrypted and can be read by anyone who has the token.** Therefore, sensitive information should not be stored directly in the payload.
* **Signature:** Ensures the integrity and authenticity of the token. It is created by taking the Base64Url encoded header, the Base64Url encoded payload, a secret key (for symmetric algorithms like HMAC), or a private key (for asymmetric algorithms like RSA or ECDSA), the specified algorithm in the header, and signing them.

A complete JWT looks like this: `header.payload.signature`

### 2. How JWT Authentication Works
The typical JWT authentication flow involves the following steps:

1.  **Authentication:** The user provides their credentials (e.g., username and password) to the server.
2.  **Token Generation:** Upon successful authentication, the server generates a JWT containing information about the user (e.g., user ID, roles) and signs it using a secret key (for symmetric signing) or its private key (for asymmetric signing).
3.  **Token Delivery:** The server sends the JWT back to the client. This is often done in the HTTP response body or as a cookie.
4.  **Token Storage:** The client stores the JWT (typically in local storage, session storage, or an HTTP-only cookie).
5.  **Subsequent Requests:** When the client wants to access a protected resource, it includes the JWT in the `Authorization` header of the HTTP request, usually using the `Bearer` schema (e.g., `Authorization: Bearer <token>`).
6.  **Token Verification:** The server receives the request with the JWT. It then verifies the JWT's signature using the secret key (or the corresponding public key if asymmetric signing is used).
7.  **Authorization:** If the signature is valid, the server decodes the payload and can extract the user's information and their associated permissions (claims). Based on these claims, the server determines if the user is authorized to access the requested resource.
8.  **Resource Access:** If the user is authorized, the server returns the requested resource. Otherwise, it returns an error.

### 3. Advantages of JWTs
* **Stateless:** The server doesn't need to store session information, as all the necessary user information is contained within the token itself. This makes JWTs ideal for scalable, distributed systems.
* **Secure:** The signature ensures that the token hasn't been tampered with and, in the case of asymmetric signing, verifies the issuer.
* **Compact:** JWTs are relatively small and can be easily transmitted in HTTP headers or URLs.
* **Cross-Platform Compatibility:** JWTs are based on open standards and can be used with various programming languages and platforms.
* **Granular Access Control:** Claims in the payload can be used to define specific permissions and roles for users.
* **Single Sign-On (SSO):** A single JWT can be passed between different applications to authenticate a user across multiple services.

### 4. Disadvantages of JWTs
* **Revocation is Difficult:** Once a JWT is issued, it's challenging to revoke it before its expiration time without additional mechanisms like token blacklisting.
* **Token Size:** JWTs can become larger if many claims are included, which can impact bandwidth.
* **Security Risks with Poor Implementation:** Storing sensitive data in the payload (even if encoded) or using weak signing algorithms can lead to security vulnerabilities. Improper handling of the secret key is also a significant risk.
* **Expiration Management:** While expiration claims (`exp`) help limit the lifespan of tokens, managing token expiration and renewal (e.g., using refresh tokens) adds complexity.
* **Statelessness Limitations for Authorization Changes:** If a user's permissions change after a JWT is issued but before it expires, those changes won't be reflected until a new token is obtained. This can be mitigated with shorter expiration times or by re-verifying against a data store on critical operations.

In summary, JWTs provide a secure and stateless way to handle authentication and authorization. However, it's crucial to implement them correctly and be aware of their limitations, especially regarding token revocation and the sensitivity of data stored within the token.
