Here’s a clear `README`-style explanation for your **JWT token generation** and **password hashing** logic, step-by-step based on your code. This will help you communicate how authentication is handled in your backend:

---

## 🔐 Authentication Overview (JWT & Password Hashing)

### ✅ What We're Doing:
1. **Generating JWT Tokens** after successful login for stateless authentication.
2. **Storing and validating hashed passwords** to ensure security.
3. **Using roles in tokens** to authorize access based on user type (e.g., `ADMIN`, `CUSTOMER`).

---

## 1️⃣ JWT Token Generation & Validation

📄 **Class**: `JwtUtil.java`  
📦 **Package**: `com.kyc.onboarding.security`

### 🔧 Key Features:

- **Secret Key**:  
  A secure key is used to sign and validate JWT tokens:
  ```java
  private static final String SECRET = "yourSuperSecretKeyWithAtLeast32Characters";
  ```

- **Generate Token**:
  When a user logs in, we generate a token with:
  - Email (as subject)
  - Role (as claim)
  - Expiration time (1 hour)

  ```java
  public String generateToken(String email, String role) {
      return Jwts.builder()
          .setSubject(email)
          .claim("role", role)
          .setIssuedAt(new Date())
          .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60))
          .signWith(SECRET_KEY, SignatureAlgorithm.HS256)
          .compact();
  }
  ```

- **Extract Information**:
  We can extract `email` and `role` from a valid token:
  ```java
  public String extractEmail(String token) { ... }
  public String extractRole(String token) { ... }
  ```

### 📦 Example Workflow:

1. **User logs in with credentials**
2. **Backend verifies credentials**
3. **JWT token is generated and returned**
4. **Frontend stores and sends token in `Authorization: Bearer <token>` header**
5. **Backend extracts user info from token on protected API calls**

---

## 2️⃣ Password Hashing (Spring Security)

Although you didn’t include the code snippet, here's a typical Spring Security setup that matches most projects like yours:

### 🔐 Password Encoding

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

### 🔄 During Registration:
- The password is hashed before saving:
  ```java
  String hashedPassword = passwordEncoder.encode(rawPassword);
  user.setPassword(hashedPassword);
  ```

### 🔒 During Login:
- The raw password is compared with the stored hashed password:
  ```java
  if (passwordEncoder.matches(rawPassword, storedHashedPassword)) {
      // Success
  } else {
      // Invalid credentials
  }
  ```

---

## 📌 Summary of Backend Security

| Feature             | Description                                               |
|---------------------|-----------------------------------------------------------|
| JWT Token           | Stateless, signed, contains email & role                  |
| Token Expiry        | 1 hour                                                    |
| Secret Key          | Base64 encoded and securely signed                        |
| Password Hashing    | Uses BCrypt for secure password storage                   |
| Token Usage         | Sent in header: `Authorization: Bearer <token>`           |

---

Let me know if you want me to include actual code for password hashing, or describe how this ties into your login controller and filters.
