# 🔐 Authentication & Authorization Guide (RBAC)

This guide explains how we secure our application using **Spring Security**.

---

## 1. The Concepts

### 🆔 Authentication (AuthN) - "Who are you?"
*   **Goal**: Verify the user's identity.
*   **How we do it**: The user sends a `username` and `password` to `/api/auth/login`.
*   **Result**: If correct, we give them a **JWT Token** (like a digital ID card).

### 👮 Authorization (AuthZ) - "What are you allowed to do?"
*   **Goal**: Check if the user has permission to perform an action.
*   **How we do it**: We check the **Role** inside their JWT Token.
*   **Example**: "Only Admins can delete tasks."

---

## 2. Role-Based Access Control (RBAC)

We have two roles in our system:
1.  **USER**: Can create, read, and update tasks.
2.  **ADMIN**: Can do everything, including **deleting** tasks.

### How it works in code:

In `TodoController.java`, look at the `deleteTodo` method:

```java
@DeleteMapping("/{todoId}")
@PreAuthorize("hasRole('ADMIN')") // <--- The Guard
public ResponseEntity<Void> deleteTodo(...) {
    // ...
}
```

The `@PreAuthorize("hasRole('ADMIN')")` annotation tells Spring Security:
> "Stop! Before running this method, check if the user has the 'ADMIN' role. If not, kick them out!"

---

## 3. How to Test This (Step-by-Step)

### Step 1: Register an Admin
Send a POST request to `http://localhost:5000/api/auth/register`:
```json
{
  "username": "admin",
  "password": "password123"
  // Note: In a real app, you'd have a way to specify roles. 
  // For this demo, you might need to manually change the role in the database 
  // or update the RegisterRequest to accept a role.
}
```

### Step 2: Login
Send a POST request to `http://localhost:5000/api/auth/login`:
```json
{
  "username": "admin",
  "password": "password123"
}
```
**Copy the Token** from the response.

### Step 3: Try to Delete (As Admin)
Send a DELETE request to `http://localhost:5000/api/v1/todos/1`.
*   **Header**: `Authorization: Bearer <YOUR_TOKEN>`
*   **Result**: `200 OK` (Success!)

### Step 4: Try to Delete (As Normal User)
1.  Register a new user "john".
2.  Login as "john" and get his token.
3.  Try to DELETE the same todo.
4.  **Result**: `403 Forbidden` (Access Denied!)

---

## 4. Summary

| Feature | Annotation | Where? |
| :--- | :--- | :--- |
| **Enable Security** | `@EnableWebSecurity` | `SecurityConfig.java` |
| **Enable RBAC** | `@EnableMethodSecurity` | `SecurityConfig.java` |
| **Protect Method** | `@PreAuthorize("hasRole('ADMIN')")` | `TodoController.java` |
