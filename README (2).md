# 🔐 Secure User Authentication

> 🚀 A professional **Spring Boot + Spring Security + MySQL** authentication project built with Java 21.

![Java](https://img.shields.io/badge/Java-21-orange?style=for-the-badge&logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-4.1.0-brightgreen?style=for-the-badge&logo=springboot)
![Spring Security](https://img.shields.io/badge/Spring%20Security-Enabled-6DB33F?style=for-the-badge&logo=springsecurity)
![MySQL](https://img.shields.io/badge/MySQL-Database-blue?style=for-the-badge&logo=mysql)
![Maven](https://img.shields.io/badge/Maven-Build-C71A36?style=for-the-badge&logo=apachemaven)

## ✨ Overview

**Secure User Authentication** is a Spring Boot application designed as a foundation for building a secure, database-backed authentication and authorization system.

The project uses modern Java and Spring technologies and is structured so that user registration, login, password security, role-based authorization, and database persistence can be added cleanly.

> 💡 **Current status:** The repository currently contains the Spring Boot application bootstrap class and a context-load test. Authentication endpoints, user entities, repositories, services, and complete security configuration are the next implementation steps.

---

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| ☕ Java 21 | Application development |
| 🍃 Spring Boot 4.1.0 | Application framework |
| 🔐 Spring Security | Authentication & authorization |
| 🗄️ Spring Data JPA | Database persistence |
| 🐬 MySQL | Relational database |
| 🌐 Spring MVC | REST/API layer |
| ✅ Bean Validation | Request validation |
| ✨ Lombok | Boilerplate reduction |
| 📦 Maven | Build & dependency management |
| 🧪 JUnit | Automated testing |

---

## 📁 Project Structure

```text
secure-user-authentication/
├── 📂 src/
│   ├── 📂 main/
│   │   ├── 📂 java/
│   │   │   └── 📂 com/growfinix/secure_user_authentication/
│   │   │       └── ☕ SecureUserAuthenticationApplication.java
│   │   └── 📂 resources/
│   │       ├── 📂 static/
│   │       ├── 📂 templates/
│   │       └── ⚙️ application.properties
│   └── 📂 test/
│       └── 📂 java/
│           └── 📂 com/growfinix/secure_user_authentication/
│               └── 🧪 SecureUserAuthenticationApplicationTests.java
├── 📂 .mvn/
│   └── 📂 wrapper/
├── 📄 .gitignore
├── 📄 .gitattributes
├── 📄 HELP.md
├── 📄 mvnw
├── 📄 mvnw.cmd
└── 📄 pom.xml
```

---

# 🚀 Getting Started

## 1️⃣ Prerequisites

Install the following before running the project:

- ☕ **JDK 21+**
- 🐬 **MySQL 8.x**
- 📦 Maven Wrapper (already included)
- 💻 IntelliJ IDEA / Eclipse / VS Code — optional

Check your Java version:

```bash
java -version
```

Check Maven Wrapper:

```bash
./mvnw -version
```

Windows:

```powershell
.\mvnw.cmd -version
```

---

## 2️⃣ Clone the Repository

```bash
git clone <repository-url>
cd secure-user-authentication
```

Replace `<repository-url>` with your actual GitHub repository URL.

---

# 🐬 Database Setup

Create a MySQL database:

```sql
CREATE DATABASE secure_user_authentication;
```

Verify that it exists:

```sql
SHOW DATABASES;
```

Select the database:

```sql
USE secure_user_authentication;
```

---

# ⚙️ Application Configuration

Open:

```text
src/main/resources/application.properties
```

### 🔧 Local Development Configuration

```properties
spring.application.name=secure-user-authentication

spring.datasource.url=jdbc:mysql://localhost:3306/secure_user_authentication
spring.datasource.username=YOUR_DB_USERNAME
spring.datasource.password=YOUR_DB_PASSWORD

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=false
spring.jpa.properties.hibernate.format_sql=true
```

⚠️ **Never commit real database passwords, API keys, JWT secrets, or other credentials to Git.**

For production, use environment variables or a secure secrets manager.

---

# 🧩 Main Application Code

The Spring Boot application starts from the main application class:

```java
package com.growfinix.secure_user_authentication;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SecureUserAuthenticationApplication {

    public static void main(String[] args) {
        SpringApplication.run(
            SecureUserAuthenticationApplication.class,
            args
        );
    }
}
```

### ▶️ What this code does

- 🚀 Starts the Spring Boot application.
- 🔍 Enables component scanning.
- ⚙️ Enables Spring Boot auto-configuration.
- 🌐 Starts the embedded web server.
- 🔗 Loads configured Spring components.

---

# 🔐 Recommended Authentication Architecture

A professional authentication implementation can follow this structure:

```text
                 👤 Client
                    │
                    ▼
             🌐 REST Controller
                    │
                    ▼
             ✅ Request Validation
                    │
                    ▼
             🔐 Spring Security
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
    👤 User Service      🛡️ Authorization
          │                   │
          └─────────┬─────────┘
                    ▼
              🗃️ Repository
                    │
                    ▼
              🐬 MySQL Database
```

---

# 👤 User Entity Example

A typical user entity can be implemented like this:

```java
package com.growfinix.secure_user_authentication.entity;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "users")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;

    @Column(nullable = false, unique = true)
    private String email;

    @Column(nullable = false)
    private String password;

    @Column(nullable = false)
    private String role;
}
```

> 🔒 Passwords must **never** be stored as plain text. Store a secure password hash instead.

---

# 🗃️ Repository Example

```java
package com.growfinix.secure_user_authentication.repository;

import com.growfinix.secure_user_authentication.entity.User;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.Optional;

public interface UserRepository extends JpaRepository<User, Long> {

    Optional<User> findByUsername(String username);

    Optional<User> findByEmail(String email);
}
```

---

# 🔐 Password Encoder Example

Spring Security should hash passwords before they are stored.

```java
package com.growfinix.secure_user_authentication.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

@Configuration
public class SecurityConfig {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

### 🔒 Why BCrypt?

BCrypt is designed specifically for password hashing and includes a salt, making stored password hashes much safer than plain-text passwords.

---

# 📝 Registration Service Example

```java
package com.growfinix.secure_user_authentication.service;

import com.growfinix.secure_user_authentication.entity.User;
import com.growfinix.secure_user_authentication.repository.UserRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Service;

@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository userRepository;
    private final PasswordEncoder passwordEncoder;

    public User register(User user) {

        user.setPassword(
            passwordEncoder.encode(user.getPassword())
        );

        if (user.getRole() == null || user.getRole().isBlank()) {
            user.setRole("USER");
        }

        return userRepository.save(user);
    }
}
```

---

# 🌐 Registration Controller Example

```java
package com.growfinix.secure_user_authentication.controller;

import com.growfinix.secure_user_authentication.entity.User;
import com.growfinix.secure_user_authentication.service.UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/auth")
@RequiredArgsConstructor
public class AuthController {

    private final UserService userService;

    @PostMapping("/register")
    public ResponseEntity<User> register(@RequestBody User user) {
        return ResponseEntity.ok(userService.register(user));
    }
}
```

⚠️ **Production improvement:** Use a DTO rather than returning the `User` entity directly, so the password field can never accidentally be exposed in an API response.

---

# 🛡️ Security Configuration Example

For a modern Spring Security application:

```java
package com.growfinix.secure_user_authentication.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(
            HttpSecurity http) throws Exception {

        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .anyRequest().authenticated()
            );

        return http.build();
    }
}
```

> ⚠️ Disabling CSRF is appropriate only for certain stateless API architectures. For browser/session-based applications, CSRF protection should normally remain enabled.

---

# 🧪 Testing

Run the test suite:

```bash
./mvnw test
```

Windows:

```powershell
.\mvnw.cmd test
```

The current test verifies that the Spring application context loads successfully.

Example:

```java
@SpringBootTest
class SecureUserAuthenticationApplicationTests {

    @Test
    void contextLoads() {
    }
}
```

---

# ▶️ Run the Application

### Linux / macOS

```bash
./mvnw spring-boot:run
```

### Windows

```powershell
.\mvnw.cmd spring-boot:run
```

### Build a JAR

```bash
./mvnw clean package
```

Run the generated JAR:

```bash
java -jar target/*.jar
```

---

# 🧪 Example API Request

After implementing the registration endpoint, a request can look like:

```bash
curl -X POST http://localhost:8080/api/auth/register   -H "Content-Type: application/json"   -d '{
    "username": "john",
    "email": "john@example.com",
    "password": "StrongPassword123!"
  }'
```

Example response should **not expose the password**:

```json
{
  "id": 1,
  "username": "john",
  "email": "john@example.com",
  "role": "USER"
}
```

---

# 🔒 Security Best Practices

Follow these practices when completing the project:

- 🔐 Hash passwords using BCrypt or Argon2.
- 🚫 Never store plain-text passwords.
- 🚫 Never return password hashes from REST APIs.
- 🎫 Use secure JWT/session handling when authentication is implemented.
- 🛡️ Apply role-based authorization.
- ✅ Validate every external input.
- 🚦 Add rate limiting to sensitive authentication endpoints.
- 🔑 Keep secrets in environment variables or a secrets manager.
- 🌐 Use HTTPS outside local development.
- 🧾 Add security/audit logging without logging passwords or tokens.
- 🔄 Keep dependencies updated.
- 🧪 Add unit, integration, and security tests.

---

# 🗺️ Development Roadmap

### Phase 1 — Foundation ✅

- [x] Spring Boot project
- [x] Maven configuration
- [x] Java 21
- [x] Spring Security dependency
- [x] Spring Data JPA dependency
- [x] MySQL dependency
- [x] Basic application test

### Phase 2 — Authentication 🔐

- [ ] User entity
- [ ] User repository
- [ ] Registration API
- [ ] Password hashing
- [ ] Login API
- [ ] Authentication service
- [ ] UserDetailsService
- [ ] Security configuration

### Phase 3 — Authorization 🛡️

- [ ] USER role
- [ ] ADMIN role
- [ ] Role-based endpoints
- [ ] Protected APIs
- [ ] Unauthorized/forbidden handlers

### Phase 4 — Production Security 🚀

- [ ] JWT authentication or secure sessions
- [ ] Refresh-token strategy
- [ ] Email verification
- [ ] Password reset
- [ ] Login rate limiting
- [ ] Security audit logs
- [ ] Integration tests
- [ ] Docker deployment
- [ ] CI/CD pipeline

---

# 📦 Build & Deployment

Build the project:

```bash
./mvnw clean package
```

The generated artifacts will be available in:

```text
target/
```

For production deployment, configure:

```text
DATABASE URL
DATABASE USERNAME
DATABASE PASSWORD
JWT SECRET
SERVER PORT
APPLICATION PROFILE
```

using environment variables or a secure configuration service.

---

# 🤝 Contributing

Contributions are welcome! 🎉

1. 🍴 Fork the repository.
2. 🌿 Create a feature branch.
3. ✍️ Implement your changes.
4. 🧪 Add or update tests.
5. 📦 Build the project.
6. 🚀 Create a pull request.

Example:

```bash
git checkout -b feature/user-authentication
git add .
git commit -m "feat: implement user authentication"
git push origin feature/user-authentication
```

---

# 📄 License

No project-specific license is currently defined.

Before distributing this project publicly, add an appropriate license such as MIT, Apache-2.0, or another license suitable for your project.

---

# 👨‍💻 Author

**GrowFinix**

🔗 Repository: `<repository-url>`

---

## ⭐ Project Status

**🟢 Foundation Ready**

The project is ready to be extended into a complete secure authentication system.

### 🎯 Next Recommended Step

Implement:

```text
👤 User Entity
      ↓
🗃️ User Repository
      ↓
🔐 Password Encoder
      ↓
⚙️ Authentication Service
      ↓
🌐 Register/Login APIs
      ↓
🛡️ Spring Security
      ↓
🎫 JWT or Session Authentication
```

> 💡 **Security first:** Authentication systems handle sensitive credentials. Review the security configuration carefully before using the project in production.
