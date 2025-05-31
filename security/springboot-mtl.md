Great! Here's a **Spring Boot 3.x example** using **WebClient** with **mutual TLS** for calling a **downstream service**. This approach is recommended for **reactive** or non-blocking applications.

---

## 🛡️ Mutual TLS with `WebClient` (Spring Boot 3.x+)

---

### ✅ What You Need

* **Keystore (`.p12`)**: Client certificate + private key.
* **Truststore (`.p12`)**: Contains downstream server’s cert or CA cert.

---

### 📁 File Structure

```
src/
└── main/
    ├── java/
    └── resources/
        ├── application.yml
        ├── client-keystore.p12
        └── client-truststore.p12
```

---

### ⚙️ `application.yml`

```yaml
server:
  port: 8080

myapp:
  tls:
    keystore:
      path: classpath:client-keystore.p12
      password: changeit
      type: PKCS12
    truststore:
      path: classpath:client-truststore.p12
      password: changeit
      type: PKCS12
```

---

### 🧠 TLS WebClient Config

```java
@Configuration
public class WebClientTlsConfig {

    @Value("${myapp.tls.keystore.path}")
    private Resource keystore;

    @Value("${myapp.tls.keystore.password}")
    private String keystorePassword;

    @Value("${myapp.tls.truststore.path}")
    private Resource truststore;

    @Value("${myapp.tls.truststore.password}")
    private String truststorePassword;

    @Bean
    public WebClient secureWebClient() throws Exception {
        // Load KeyStore
        KeyStore keyStore = KeyStore.getInstance("PKCS12");
        keyStore.load(keystore.getInputStream(), keystorePassword.toCharArray());

        // Load TrustStore
        KeyStore trustStore = KeyStore.getInstance("PKCS12");
        trustStore.load(truststore.getInputStream(), truststorePassword.toCharArray());

        // SSL Context
        SSLContext sslContext = SSLContexts.custom()
                .loadKeyMaterial(keyStore, keystorePassword.toCharArray())
                .loadTrustMaterial(trustStore, null)
                .build();

        HttpClient httpClient = HttpClient.create()
                .secure(sslSpec -> sslSpec.sslContext(sslContext));

        ClientHttpConnector connector = new ReactorClientHttpConnector(httpClient);

        return WebClient.builder()
                .clientConnector(connector)
                .baseUrl("https://secured-downstream-service")
                .build();
    }
}
```

---

### 📡 WebClient Usage

```java
@RestController
@RequestMapping("/api")
public class MutualTlsController {

    private final WebClient secureWebClient;

    public MutualTlsController(WebClient secureWebClient) {
        this.secureWebClient = secureWebClient;
    }

    @GetMapping("/downstream")
    public Mono<String> callSecureDownstream() {
        return secureWebClient
                .get()
                .uri("/api/data")
                .retrieve()
                .bodyToMono(String.class);
    }
}
```

---

### 🔐 Generate Keystore & Truststore

#### Generate Self-Signed Certificates:

```bash
# 1. Generate client keypair
keytool -genkeypair -alias client-cert \
  -keyalg RSA -keysize 2048 -storetype PKCS12 \
  -keystore client-keystore.p12 \
  -dname "CN=client" -storepass changeit -validity 365

# 2. Export client cert
keytool -export -alias client-cert \
  -storetype PKCS12 -keystore client-keystore.p12 \
  -rfc -file client-cert.pem -storepass changeit

# 3. Import server cert or CA to truststore
keytool -import -alias server-cert \
  -file server-cert.pem -storetype PKCS12 \
  -keystore client-truststore.p12 -storepass changeit -noprompt
```

> Replace `server-cert.pem` with the actual cert from the downstream service.

---

### ✅ Notes

* Works with **Spring Boot 3.x**, **WebClient**, and **Reactor Netty**.
* For **Spring Security** clients, the WebClient can be injected into OAuth flows too.
* Truststore should contain **CA or downstream server cert**.

Would you like me to provide a **dockerized version** or **test downstream server** to try this out?
