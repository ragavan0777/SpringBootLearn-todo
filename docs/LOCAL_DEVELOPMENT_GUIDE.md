# 💻 Local Development Guide

This guide explains how to run and test your Todo API on your own computer.

---

### 1. The Maven Command (Terminal)
The standard way to run a Spring Boot project is using the Maven Wrapper:
```bash
./mvnw spring-boot:run
```

### 2. Changing the Port
By default, the app runs on port **5000**. If you want to run it on a different port (e.g., 8080), you can use an environment variable:
```bash
PORT=8080 ./mvnw spring-boot:run
```

### 3. Using your Local Database
Your [application.properties](file:///home/loordhujeyakumar/Downloads/startSpring/src/main/resources/application.properties) is set up with these **Local Defaults**:
- **Host**: `localhost`
- **Username**: `root`
- **Password**: `John@1710#`

If your local MySQL settings are different, you can "override" them without changing the code:
```bash
DB_USERNAME=my_user DB_PASSWORD=my_password ./mvnw spring-boot:run
```

### 4. Running in your IDE
If you use **IntelliJ** or **VS Code**:
1. Open the file `src/main/java/com/example/startSpring/StartSpringApplication.java`.
2. Look for the green "Play" icon next to the `main` method.
3. Click it and select **Run**.

---

### 🛠️ Useful Local Links:
- **Home Dashboard**: [http://localhost:5000/](http://localhost:5000/)
- **API Documentation**: [http://localhost:5000/swagger-ui/index.html](http://localhost:5000/swagger-ui/index.html)
- **Health Check**: [http://localhost:5000/api/health](http://localhost:5000/api/health)
