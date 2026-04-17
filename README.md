# 🚀 Spring Boot Todo API (Masterclass Edition)

A professional-grade, distributed Spring Boot application designed to teach modern backend engineering. This project covers everything from **Stateless Authentication** to **Production Deployment**.

---

## 📖 Essential Documentation
Follow these guides in order to master the project:

### 🎓 For Students (The Roadmap)
1. **[00: Student Learning Path](docs/00_Student_Learning_Path.md)** - Start here! A step-by-step guide to understanding the code layers.
2. **[01: GitHub & Secrets Best Practice](docs/01_GitHub_and_Secrets_Best_Practice.md)** - Learn what to push to Git and how to keep passwords safe.
3. **[02: Project Structure](docs/02_Project_Structure.md)** - A map of the folders and files.
4. **[03: Code Best Practices](docs/03_Code_Best_Practices.md)** - Learn how to write clean, maintainable Java code.

### 🛡️ Security & Identity
- **[04: Security In-Depth](docs/04_Security_In_Depth.md)** - How we protect the API.
- **[05: Auth & JWT Deep Dive](docs/05_Auth_Deep_Dive.md)** - Understanding Tokens and Roles.

### 🚀 Operations & Deployment
- **[Local Development Guide](docs/LOCAL_DEVELOPMENT_GUIDE.md)** - Run the app on your own computer.
- **[Cloud Deployment Guide](docs/Step-by-Step_Deployment_Guide.md)** - Deploy to Render + TiDB Cloud for free.
- **[Environment Variables Guide](docs/ENVIRONMENT_VARIABLES_EXPLAINED.md)** - Managing secrets in the cloud.

---

## 🛠️ Tech Stack
- **Java 21** (LTS)
- **Spring Boot 3.5.x**
- **Spring Security** (Stateless JWT)
- **Spring Data JPA** (Hibernate)
- **MySQL / TiDB Cloud** (Distributed Database)
- **Swagger/OpenAPI 3** (Interactive API Documentation)

---

## 🚦 Quick Start

### 1. Run Locally
```bash
./mvnw spring-boot:run
```
👉 Access the **Interactive Dashboard** at: [http://localhost:5000/](http://localhost:5000/)

### 2. View API Documentation
Once the app is running, visit: [http://localhost:5000/swagger-ui/index.html](http://localhost:5000/swagger-ui/index.html)

### 3. Connection Health
Check if the database is connected: [http://localhost:5000/api/health](http://localhost:5000/api/health)

---

## 🧪 Admin Credentials (Demo)
By default, the application allows anyone to register. To test **ADMIN-only** features (like deleting any todo):
1. Register a user with the role `ADMIN`.
2. Use the generated token in the `Authorization` header.

---

## 📚 Reference Library
- **[Annotation Glossary](docs/reference/Annotation_Glossary.md)**
- **[Core Concepts](docs/reference/Core_Concepts.md)**
- **[Properties Reference](docs/reference/Properties_Guide.md)**
- **[Cheat Sheet](docs/99_Cheat_Sheet.md)**

---

## 📝 License
This project is for educational purposes. 🌟 **Leave a star** if you found it helpful!
