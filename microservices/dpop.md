**DPoP** stands for **Demonstration of Proof-of-Possession**, and it's a security mechanism used in **OAuth 2.0** to **bind an access token to a particular client** and **prevent token misuse**, especially **token replay attacks**.

---

### 🔐 Why DPoP?

In standard OAuth 2.0, access tokens are **bearer tokens**, meaning **anyone who has the token can use it**—no questions asked. This is insecure if:

* Tokens are intercepted (e.g., in a MITM attack)
* Tokens are stolen from a log or a compromised client

**DPoP addresses this by proving that the client presenting the token is the one it was originally issued to.**

---

### 🧠 How DPoP Works (Simplified)

1. **Client generates a DPoP key pair (public/private)**.
2. For each request, the client:

   * Creates a **DPoP proof**: a signed JWT (with private key) containing:

     * HTTP method and URL
     * A timestamp (`iat`)
     * A unique identifier (`jti`) to prevent reuse
   * Sends this JWT in the `DPoP` header
3. The server:

   * Verifies the signature using the public key
   * Ensures the request matches the method and URI in the JWT
   * Binds the access token to the public key (`cnf` claim)
4. Now, even if someone steals the access token, they **can’t use it** without the private key that created the DPoP proof.

---

### 📦 What’s in a DPoP Proof JWT?

Here’s a typical structure:

```json
{
  "typ": "dpop+jwt",
  "alg": "ES256",
  "jwk": { ... } // public key
}
.
{
  "htu": "https://api.example.com/resource",
  "htm": "GET",
  "iat": 1716812345,
  "jti": "unique-id-123"
}
```

---

### ✅ Benefits

* **Prevents token replay** by binding the token to the client
* Works with **public clients** (like browser apps) in a more secure way
* Does **not require TLS client certificates** (unlike MTLS)

---

### 🚫 Limitations

* **Relatively new** and not supported by all OAuth servers
* Requires **client-side key management**, which can be tricky in browsers
* **Adds overhead** to each request (extra JWT generation and validation)

---

Let me know if you'd like code examples, a diagram, or how DPoP compares to MTLS!
