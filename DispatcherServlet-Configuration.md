# Spring MVC — DispatcherServlet Configuration

## 1. What is DispatcherServlet?

`DispatcherServlet` is the **Front Controller** of Spring MVC.

It receives HTTP requests and forwards them to the appropriate Controller.

```text
Browser
   ↓
DispatcherServlet
   ↓
Controller
   ↓
Service
   ↓
DAO
   ↓
Database
```

---

## 2. Basic Configuration

In `web.xml`:

```xml
<servlet>
    <servlet-name>spring</servlet-name>

    <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
    </servlet-class>

    <load-on-startup>1</load-on-startup>
</servlet>
```

The servlet name is:

```text
spring
```

Spring MVC uses this name to find the default configuration file.

---

## 3. Default Configuration File

Spring MVC follows this naming convention:

```text
/WEB-INF/{servlet-name}-servlet.xml
```

For example:

```text
servlet-name = spring
        ↓
/WEB-INF/spring-servlet.xml
```

So the project can look like:

```text
SpringMVCProject
│
└── WebContent
    └── WEB-INF
        ├── web.xml
        └── spring-servlet.xml
```

With the default naming convention, **`contextConfigLocation` is not required**.

---

## 4. What is contextConfigLocation?

`contextConfigLocation` is used when you want to **explicitly specify the Spring MVC configuration file location**.

For example, if your file is:

```text
/WEB-INF/Springservlet.xml
```

instead of the default:

```text
/WEB-INF/spring-servlet.xml
```

configure it using:

```xml
<init-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/Springservlet.xml</param-value>
</init-param>
```

---

## 5. Complete Custom Configuration

```xml
<servlet>
    <servlet-name>spring</servlet-name>

    <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
    </servlet-class>

    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/Springservlet.xml</param-value>
    </init-param>

    <load-on-startup>1</load-on-startup>
</servlet>
```

---

## 6. Default Name vs Custom Name

| Servlet Name | Configuration File            | contextConfigLocation |
| ------------ | ----------------------------- | --------------------- |
| `spring`     | `/WEB-INF/spring-servlet.xml` | ❌ Not required        |
| `app`        | `/WEB-INF/app-servlet.xml`    | ❌ Not required        |
| `spring`     | `/WEB-INF/Springservlet.xml`  | ✅ Required            |
| `app`        | `/WEB-INF/myconfig.xml`       | ✅ Required            |

### Remember

```text
Default:
 /WEB-INF/{servlet-name}-servlet.xml

Custom:
 Use contextConfigLocation
```

---

## 7. Important Formula

```text
servlet-name
      ↓
/WEB-INF/{servlet-name}-servlet.xml
```

Examples:

```text
spring
  ↓
/WEB-INF/spring-servlet.xml
```

```text
app
  ↓
/WEB-INF/app-servlet.xml
```

```text
dispatcher
  ↓
/WEB-INF/dispatcher-servlet.xml
```

---

## 8. Request Flow

When a request such as:

```text
/student
```

comes from the browser:

```text
Browser
   ↓
DispatcherServlet
   ↓
Controller
   ↓
Service
   ↓
DAO
   ↓
Database
```

`DispatcherServlet` acts as the **central entry point** for Spring MVC requests.

---

## 9. Final web.xml Example

```xml
<web-app>

    <servlet>

        <servlet-name>spring</servlet-name>

        <servlet-class>
            org.springframework.web.servlet.DispatcherServlet
        </servlet-class>

        <load-on-startup>1</load-on-startup>

    </servlet>

    <servlet-mapping>

        <servlet-name>spring</servlet-name>

        <url-pattern>/</url-pattern>

    </servlet-mapping>

</web-app>
```

Because the servlet name is `spring`, Spring MVC looks for:

```text
/WEB-INF/spring-servlet.xml
```

---

## 10. Interview Answer

> **`contextConfigLocation` is used to explicitly specify the location of the Spring MVC configuration file when we don't want to use the default `/WEB-INF/{servlet-name}-servlet.xml` naming convention.**

---

## 11. Quick Revision

```text
DispatcherServlet
       ↓
Front Controller
       ↓
Reads Spring MVC configuration
       ↓
Default:
 /WEB-INF/{servlet-name}-servlet.xml
       ↓
Default file name → contextConfigLocation not required
       ↓
Custom file name/location → use contextConfigLocation
```

### Easy Example

```text
spring
  ↓
/WEB-INF/spring-servlet.xml
  ↓
No contextConfigLocation
```

```text
spring
  ↓
/WEB-INF/Springservlet.xml
  ↓
Use contextConfigLocation
```
---

# 🎉 Thank You for Learning!

> 💡 **Keep Learning, Keep Coding, Keep Growing!**

Thank you for spending your valuable time learning **Spring MVC**. ❤️

Remember:

```text
Learn → Practice → Make Mistakes → Fix Them → Improve 🚀
```

Don't just read the concepts — **write the code and practice it!** 💻

### 🌟 Happy Coding!

**Keep coding. Keep building. Keep achieving your goals! 🚀**

---

⭐ If these notes helped you, consider giving the repository a **Star ⭐** on GitHub.

**See you in the next lesson! 👋😊**
