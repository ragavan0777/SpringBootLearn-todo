# 🔐 Securing Your Spring Boot API: A Simple Guide

This guide explains how to add Authentication and Authorization to your Spring Boot application using **Spring Security** and **JSON Web Tokens (JWT)**. It's designed to be easy to understand and teach.

---

## 1. Core Concepts: Authentication vs. Authorization

### Authentication (AuthN) - "Who are you?"
This is the process of verifying a user's identity.
- **Example**: A user provides a username and password. The system checks if they are correct. If so, the user is **authenticated**.

### Authorization (AuthZ) - "What are you allowed to do?"
This is the process of checking if an authenticated user has permission to access a specific resource.
- **Example**: A regular user (`ROLE_USER`) can view products, but only an admin (`ROLE_ADMIN`) can add or delete them.

---

## 2. How JWT Works (The Token-Based Flow)

Instead of using sessions, we will use JWT. This is the standard for modern APIs.

1.  **Login**: The user sends their `username` and `password` to a login endpoint (e.g., `/api/auth/login`).
2.  **Token Generation**: The server validates the credentials. If they are correct, it generates a **JWT**—a long, encoded string containing user information (like username and roles).
3.  **Token Sent to Client**: The server sends this token back to the client (e.g., a web browser or mobile app).
4.  **Client Stores Token**: The client stores this token securely (e.g., in `localStorage`).
5.  **Making Authenticated Requests**: For every subsequent request to a protected endpoint (e.g., `/api/todos`), the client must include the token in the `Authorization` header.
    ```
    Authorization: Bearer <your_jwt_token>
    ```
6.  **Server Validates Token**: The server receives the request, inspects the token, and verifies its authenticity. If the token is valid, it grants access. If not, it returns a `401 Unauthorized` error.

**Why JWT?** It's **stateless**. The server doesn't need to store session information, which makes it highly scalable.

---

## 3. 📂 Code Organization for Security

Here’s how to structure the security-related code in your project:

```text
com.example.startSpring
│
├── config/
│   ├── SecurityConfig.java      (Main security rules)
│   └── ApplicationConfig.java   (Bean definitions like PasswordEncoder)
│
├── controller/
│   └── AuthController.java      (For /login and /register)
│
├── dto/
│   ├── LoginRequest.java        (DTO for login)
│   └── RegisterRequest.java     (DTO for registration)
│
├── model/
│   └── User.java                (User entity with username, password, roles)
│
├── repository/
│   └── UserRepository.java      (To find users in the database)
│
├── security/
│   ├── JwtUtil.java             (To create and validate JWTs)
│   ├── JwtRequestFilter.java    (To inspect the token on each request)
│   └── UserDetailsServiceImpl.java (To load user data for Spring Security)
```

---

## 4. 📜 Spring Security Cheat Sheet

### Key Annotations & Classes

| Annotation / Class | Purpose |
| :--- | :--- |
| `@EnableWebSecurity` | Enables Spring's web security support. |
| `@EnableMethodSecurity` | Enables `@PreAuthorize` for RBAC. |
| `SecurityFilterChain` | A bean that defines the main security rules, like which endpoints are public and which are protected. |
| `PasswordEncoder` | A bean used to hash passwords before saving them. **Never store plain text passwords!** |
| `AuthenticationManager` | The core component that processes an authentication request. |
| `@PreAuthorize` | An annotation to secure individual methods based on roles (e.g., `@PreAuthorize("hasRole('ADMIN')")`). |

### Security Configuration (`SecurityConfig.java`)

This is where you define your security rules.

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity // Enables @PreAuthorize
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable()) // Disable CSRF for stateless APIs
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll() // Allow login/register
                .anyRequest().authenticated() // Secure all other endpoints
            )
            .sessionManagement(sess -> sess.sessionCreationPolicy(SessionCreationPolicy.STATELESS)); // Use stateless sessions

        // Add JWT filter before the standard username/password filter
        http.addFilterBefore(jwtRequestFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}
```

---

## 5. 🛠️ Tips for Teaching Security

1.  **Start Unprotected**: First, show the students that all endpoints are public. Anyone can access `/api/todos`.
2.  **Add Spring Security**: Add the dependency. Show them that **everything** is now locked by default (returns 401). This demonstrates Spring Security's "secure by default" principle.
3.  **Open Up Login**: In `SecurityConfig`, explicitly permit access to `/api/auth/login` and `/api/auth/register`.
4.  **Demonstrate the Flow**:
    *   Use Postman to hit the `/register` endpoint to create a user.
    *   Hit the `/login` endpoint to get a JWT.
    *   Try to access `/api/todos` without the token (it will fail).
    *   Access `/api/todos` again, but this time add the `Authorization: Bearer <token>` header (it will succeed).

### Important Note on Roles
Spring Security expects roles to start with `ROLE_`.
In `User.java`, we handle this automatically:
```java
return List.of(new SimpleGrantedAuthority("ROLE_" + role.name()));
```
This allows `@PreAuthorize("hasRole('ADMIN')")` to work correctly.
