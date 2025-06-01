Here’s a breakdown of the HTTP **security headers** you mentioned — each is designed to help protect against specific types of web attacks:

---

### 🛡️ 1. **Content-Security-Policy (CSP)**

**Purpose:**
Mitigates **XSS**, **clickjacking**, and data injection attacks by restricting what content (scripts, styles, images, etc.) the browser is allowed to load.

**Example:**

```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://apis.google.com; object-src 'none';
```

**Explanation:**

* `default-src 'self'` → allow all content to load only from the same origin
* `script-src 'self' https://apis.google.com` → allow scripts from self and Google APIs
* `object-src 'none'` → block Flash and other plugins

**Why it's important:**
Stops browsers from loading malicious scripts injected by attackers.

---

### 🧼 2. **X-XSS-Protection** (Legacy)

**Purpose:**
Enables or disables the browser’s **built-in XSS filter** (mostly for older browsers).

**Example:**

```http
X-XSS-Protection: 1; mode=block
```

**Options:**

* `0` → disable XSS protection
* `1` → enable XSS filtering
* `1; mode=block` → block the page if an attack is detected

**Note:**
Modern browsers like Chrome and Edge **ignore this header** when **Content-Security-Policy** is set. Use CSP as the main protection instead.

---

### 🧪 3. **X-Content-Type-Options**

**Purpose:**
Prevents the browser from **MIME-sniffing** (guessing content types) which could lead to executing non-script files (like images) as JavaScript.

**Example:**

```http
X-Content-Type-Options: nosniff
```

**Why it's important:**
It forces the browser to obey the declared `Content-Type` of a resource. This helps prevent certain XSS and drive-by download attacks.

---

### 🧠 Summary Table

| Header                    | Use Case                           | Example               |
| ------------------------- | ---------------------------------- | --------------------- |
| `Content-Security-Policy` | Prevent XSS, restrict sources      | `default-src 'self';` |
| `X-XSS-Protection`        | Legacy XSS protection (deprecated) | `1; mode=block`       |
| `X-Content-Type-Options`  | Prevent MIME type sniffing         | `nosniff`             |

---

Would you like a Spring Boot configuration that automatically sets these headers in HTTP responses?
