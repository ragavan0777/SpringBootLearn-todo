# Application Properties Configuration Guide

## Complete Reference

### 1. MySQL Database Configuration

```properties
# Connection URL
spring.datasource.url=jdbc:mysql://localhost:3306/todo_db?useSSL=true&serverTimezone=UTC

# Database credentials
spring.datasource.username=root
spring.datasource.password=your-secure-password

# MySQL Connector Driver
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# Connection Pool Settings
spring.datasource.hikari.maximum-pool-size=10
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.connection-timeout=20000
```

**Explanation:**
- `url`: Where your database is hosted (localhost for development, Clever Cloud host for production)
- `username`: Database user (root is default, create dedicated user for production)
- `password`: Database password (use strong password in production)
- `useSSL=true`: Encrypt connection to database (important for security)
- `serverTimezone=UTC`: Set timezone to avoid timestamp issues

---

### 2. JPA/Hibernate Configuration

```properties
# Hibernate DDL Auto - Controls database schema management
spring.jpa.hibernate.ddl-auto=update

# Dialect - Tells Hibernate which SQL database to use
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect

# Show SQL queries in console (useful for debugging)
spring.jpa.show-sql=false

# Format SQL output nicely
spring.jpa.properties.hibernate.format_sql=true

# Additional Hibernate properties
spring.jpa.properties.hibernate.use_sql_comments=true
```

**DDL Auto Options:**
| Value | Behavior | When to Use |
|-------|----------|------------|
| `create` | Drop & recreate tables every startup | Development only (DANGER: loses data) |
| `create-drop` | Create on startup, drop on shutdown | Testing |
| `update` | Modify existing tables (ADD columns only) | Development & Production |
| `validate` | Only validate tables match entities | Production (safe, no changes) |
| `none` | Do nothing | Production with manual migrations |

---

### 3. Server Configuration

```properties
# Port the application listens on
server.port=${PORT:8080}

# Context path for all endpoints
server.servlet.context-path=/api

# Application name
spring.application.name=todo-app

# Server configuration
server.servlet.session.timeout=30m
server.compression.enabled=true
server.compression.mime-types=text/html,text/xml,text/plain,text/css,text/javascript,application/javascript,application/json
```

**Explanation:**
- `${PORT:8080}`: Use PORT environment variable if set, otherwise default to 8080
- `context-path=/api`: All endpoints prefixed with /api (so localhost:8080/api/todos instead of localhost:8080/todos)

---

### 4. Logging Configuration

```properties
# Root logger level
logging.level.root=INFO

# Your application package logging
logging.level.com.example=DEBUG
logging.level.com.example.controller=DEBUG
logging.level.com.example.service=DEBUG

# Spring specific logging
logging.level.org.springframework=INFO
logging.level.org.springframework.security=DEBUG
logging.level.org.springframework.web=DEBUG

# Hibernate SQL logging
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE

# Log file configuration
logging.file.name=logs/app.log
logging.file.max-size=10MB
logging.file.max-history=10
logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss} - %msg%n
```

**Log Levels (from least to most verbose):**
- `ERROR`: Only errors
- `WARN`: Warnings and errors
- `INFO`: General info, warnings, errors (DEFAULT)
- `DEBUG`: Detailed debugging info
- `TRACE`: Very detailed, rarely needed

---

### 5. JWT Security Configuration

```properties
# JWT Secret Key (use strong, random key, at least 256 bits for HS256)
jwt.secret=your-very-long-secure-secret-key-make-it-at-least-256-bits-long-randomkey12345

# Token expiration time in milliseconds
# 86400000 = 24 hours
jwt.expiration=86400000

# Alternative: Use environment variable
jwt.secret=${JWT_SECRET:default-dev-secret}
jwt.expiration=${JWT_EXPIRATION:86400000}
```

**Generating a Secure Secret:**
```bash
# Using OpenSSL (Linux/Mac)
openssl rand -base64 32

# Online generator: https://www.allkeysgenerator.com/
```

---

### 6. Spring Security Configuration

```properties
# CORS Configuration (if frontend is separate)
cors.allowed-origins=http://localhost:3000,https://yourfrontend.com
cors.allowed-methods=GET,POST,PUT,DELETE,OPTIONS
cors.allowed-headers=*
cors.allow-credentials=true
cors.max-age=3600

# Disable default Spring Security login form
spring.security.user.name=admin
spring.security.user.password=admin123
```

---

### 7. JSON/API Configuration

```properties
# JSON date format
spring.jackson.serialization.write-dates-as-timestamps=false
spring.jackson.time-zone=UTC
spring.jackson.default-property-inclusion=non_null
spring.jackson.serialization.indent-output=true

# HTTP encoding
server.servlet.encoding.charset=UTF-8
server.servlet.encoding.enabled=true
server.servlet.encoding.force=true
```

---

### 8. Application Info (shown in Actuator)

```properties
spring.application.name=todo-app
info.app.name=Spring Boot Todo Application
info.app.description=Learn Spring Boot with Todo App
info.app.version=1.0.0
info.app.encoding=${project.build.sourceEncoding}
info.app.java.version=${java.version}
info.company.name=Your Company
info.company.email=your@email.com
```

---

### 9. Profiles (Different configs for different environments)

Create separate files:
- `application.properties` (shared)
- `application-dev.properties` (development)
- `application-prod.properties` (production)
- `application-test.properties` (testing)

**application.properties (shared):**
```properties
spring.profiles.active=dev
logging.level.root=INFO
```

**application-dev.properties:**
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/todo_dev
spring.datasource.username=root
spring.datasource.password=dev-password
spring.jpa.hibernate.ddl-auto=create-drop
logging.level.root=DEBUG
```

**application-prod.properties:**
```properties
spring.datasource.url=${SPRING_DATASOURCE_URL}
spring.datasource.username=${SPRING_DATASOURCE_USERNAME}
spring.datasource.password=${SPRING_DATASOURCE_PASSWORD}
spring.jpa.hibernate.ddl-auto=validate
logging.level.root=WARN
```

**Run with profile:**
```bash
./mvnw spring-boot:run -Dspring-boot.run.arguments="--spring.profiles.active=prod"
```

---

### 10. Complete Production Example

```properties
# Application
spring.application.name=todo-app
spring.profiles.active=production

# MySQL (from environment variables)
spring.datasource.url=${SPRING_DATASOURCE_URL}
spring.datasource.username=${SPRING_DATASOURCE_USERNAME}
spring.datasource.password=${SPRING_DATASOURCE_PASSWORD}
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.hikari.maximum-pool-size=20

# JPA/Hibernate
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=false
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect

# Server
server.port=${PORT:8080}
server.servlet.context-path=/api
server.compression.enabled=true

# Logging
logging.level.root=WARN
logging.level.com.example=INFO
logging.file.name=/var/log/app.log

# Security
jwt.secret=${JWT_SECRET}
jwt.expiration=${JWT_EXPIRATION}

# JSON
spring.jackson.default-property-inclusion=non_null
```

---

## Environment Variables for Deployment

For Render.com, set these environment variables:

```
SPRING_DATASOURCE_URL=jdbc:mysql://host:3306/db?useSSL=true&serverTimezone=UTC
SPRING_DATASOURCE_USERNAME=username
SPRING_DATASOURCE_PASSWORD=password
JWT_SECRET=your-very-long-secret-key
JWT_EXPIRATION=86400000
PORT=10000
```

---

## Best Practices

✅ **DO:**
- Use environment variables for sensitive data
- Use different profiles for dev/prod
- Set `spring.jpa.hibernate.ddl-auto=validate` in production
- Use strong JWT secrets (min 256 bits)
- Enable HTTPS in production
- Set appropriate log levels (DEBUG for dev, WARN for prod)

❌ **DON'T:**
- Commit passwords to GitHub
- Use hardcoded credentials in code
- Use `ddl-auto=create` in production
- Log sensitive information
- Disable SSL in production
- Share JWT secret in emails/messages

---

## Troubleshooting

### Database Connection Issues
Check if URL format is correct:
```properties
# Local MySQL
spring.datasource.url=jdbc:mysql://localhost:3306/todo

# Remote MySQL (Clever Cloud)
spring.datasource.url=jdbc:mysql://xxx.MySQL.cleverapps.io:3306/xxx?useSSL=true&serverTimezone=UTC
```

### Port Already in Use
```properties
# Use different port
server.port=9090
```

### Context Path Issues
```properties
# Make sure endpoints match context path
server.servlet.context-path=/api
# So endpoints become: http://localhost:8080/api/todos
```