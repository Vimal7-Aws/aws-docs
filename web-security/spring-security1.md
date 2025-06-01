Great — let’s look at how to **protect a Spring Boot application** from common web attacks like SQL injection, XSS, CSRF, bot abuse, and more.

---

## ✅ **Spring Boot Security Checklist**

### 🔐 1. **Prevent SQL Injection**

**Use JPA / Hibernate (Recommended)** – they use parameterized queries internally.

Example:

```java
@Query("SELECT u FROM User u WHERE u.username = :username")
User findByUsername(@Param("username") String username);
```

If you use JDBC:

```java
String query = "SELECT * FROM users WHERE username = ?";
PreparedStatement ps = conn.prepareStatement(query);
ps.setString(1, username);
```

---

### 🧼 2. **Prevent Cross-Site Scripting (XSS)**

**Escape output** in templates:

* If you’re using **Thymeleaf**, it auto-escapes output:

  ```html
  <p th:text="${userInput}"></p> <!-- Safe -->
  ```

**Sanitize user input** (optional but recommended):

```java
import org.owasp.html.PolicyFactory;
import org.owasp.html.Sanitizers;

PolicyFactory policy = Sanitizers.FORMATTING.and(Sanitizers.LINKS);
String cleanHtml = policy.sanitize(userInput);
```

---

### 🛡️ 3. **Enable CSRF Protection**

**Spring Security** enables CSRF by default for web apps.

In `SecurityConfig`:

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http
      .csrf().csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())
      .and()
      .authorizeRequests()
      .anyRequest().authenticated();
  }
}
```

For APIs (e.g., REST), you might **disable CSRF** or use **token-based authentication** (like JWT), since CSRF applies mainly to cookie-based sessions.

---

### 🤖 4. **Limit Bot Traffic / DDoS**

**Option 1: Rate Limiting (in memory)** using [Bucket4j](https://github.com/vladimir-bukhtoyarov/bucket4j):

```java
Bucket bucket = Bucket4j.builder()
    .addLimit(Bandwidth.simple(100, Duration.ofMinutes(1)))
    .build();

if (bucket.tryConsume(1)) {
    // process request
} else {
    // reject with 429 Too Many Requests
}
```

**Option 2: Cloud protection**

* Use **AWS WAF + CloudFront**
* Add **CAPTCHA** (Google reCAPTCHA or hCaptcha) in your forms

---

### 🧠 5. **Add HTTP Security Headers**

Spring Security can auto-configure most headers:

```java
http
  .headers()
    .contentSecurityPolicy("script-src 'self'")
    .xssProtection()
    .and()
    .frameOptions().deny();
```

You get:

* `X-Content-Type-Options: nosniff`
* `X-XSS-Protection: 1; mode=block`
* `X-Frame-Options: DENY`

---

### 🧾 6. **Logging and Monitoring**

* Use **Spring Boot Actuator** for health and metrics.
* Integrate with **AWS CloudWatch**, **ELK**, or **Grafana** for logging and alerting.
* Log suspicious activity (e.g., failed logins, rate limits).

---

## 🚀 Bonus: reCAPTCHA Integration (Simplified)

In your HTML:

```html
<form method="post">
  <div class="g-recaptcha" data-sitekey="YOUR_SITE_KEY"></div>
  <button type="submit">Submit</button>
</form>
<script src="https://www.google.com/recaptcha/api.js"></script>
```

In your controller:

```java
public boolean verifyRecaptcha(String responseToken) {
    String url = "https://www.google.com/recaptcha/api/siteverify";
    RestTemplate restTemplate = new RestTemplate();
    MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
    params.add("secret", "YOUR_SECRET_KEY");
    params.add("response", responseToken);
    RecaptchaResponse resp = restTemplate.postForObject(url, params, RecaptchaResponse.class);
    return resp.isSuccess();
}
```

---

Would you like a **starter Spring Boot project** with all these security features pre-configured?
