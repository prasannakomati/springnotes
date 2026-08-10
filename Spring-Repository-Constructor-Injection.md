# Spring — `@Repository` + Constructor Injection + JdbcTemplate

## 1. What is `@Repository`?

`@Repository` is a Spring annotation used to identify a class as a **DAO (Data Access Object)**.

It tells Spring:

> "This class is responsible for communicating with the database."

Example:

```java
@Repository
public class CategoryDAO {

    // DAO methods
}
```

Because `@Repository` is a Spring stereotype annotation, Spring can automatically detect this class when component scanning is enabled.

---

# 2. Component Scanning

Suppose your XML contains:

```xml
<context:component-scan base-package="com.ecommerce" />
```

Spring scans the `com.ecommerce` package and its sub-packages.

For example:

```text
com.ecommerce
│
├── controller
│
├── service
│
└── dao
    ├── CategoryDAO
    ├── ProductDAO
    ├── CartDAO
    └── OrderDAO
```

If the DAO classes have `@Repository`, Spring automatically detects them.

```java
@Repository
public class CategoryDAO {
}
```

You don't need to manually define every DAO as an XML `<bean>`.

---

# 3. What is Constructor Injection?

Constructor injection means providing a dependency to a class through its constructor.

For example:

```java
public CategoryDAO(JdbcTemplate template) {
    this.template = template;
}
```

Here:

```text
CategoryDAO
     ↓
needs JdbcTemplate
     ↓
JdbcTemplate is provided through constructor
```

This is called **Constructor Dependency Injection**.

---

# 4. CategoryDAO Example

A clean DAO implementation can look like this:

```java
@Repository
public class CategoryDAO {

    private final JdbcTemplate template;

    public CategoryDAO(JdbcTemplate template) {
        this.template = template;
    }

    // DAO methods
}
```

Let's understand it step by step.

### `@Repository`

```java
@Repository
```

Tells Spring that `CategoryDAO` is a DAO component.

### `private final JdbcTemplate`

```java
private final JdbcTemplate template;
```

The DAO needs a `JdbcTemplate` to communicate with the database.

### Constructor

```java
public CategoryDAO(JdbcTemplate template) {
    this.template = template;
}
```

Spring provides the `JdbcTemplate` object through the constructor.

---

# 5. JdbcTemplate Bean

Suppose your Spring XML already contains:

```xml
<bean id="dataSource"
      class="org.springframework.jdbc.datasource.DriverManagerDataSource">

    <!-- Database configuration -->

</bean>
```

And:

```xml
<bean id="jdbcTemplate"
      class="org.springframework.jdbc.core.JdbcTemplate">

    <property name="dataSource" ref="dataSource"/>

</bean>
```

Now Spring has a `JdbcTemplate` bean.

The dependency relationship is:

```text
dataSource
     ↓
JdbcTemplate
     ↓
DAO
```

---

# 6. How Spring Injects JdbcTemplate

Your XML contains:

```xml
<bean id="jdbcTemplate"
      class="org.springframework.jdbc.core.JdbcTemplate">

    <property name="dataSource" ref="dataSource"/>

</bean>
```

And your DAO contains:

```java
@Repository
public class CategoryDAO {

    private final JdbcTemplate template;

    public CategoryDAO(JdbcTemplate template) {
        this.template = template;
    }
}
```

Spring sees:

```text
@Repository
     ↓
CategoryDAO
     ↓
Constructor requires JdbcTemplate
     ↓
Spring searches for JdbcTemplate bean
     ↓
Finds jdbcTemplate
     ↓
Injects it into constructor
     ↓
CategoryDAO is ready ✅
```

---

# 7. Do We Need to Define CategoryDAO in XML?

Without component scanning, you might manually configure the DAO:

```xml
<bean id="categoryDAO"
      class="com.ecommerce.dao.CategoryDAO">

    <property name="template" ref="jdbcTemplate"/>

</bean>
```

But if you are using:

```xml
<context:component-scan base-package="com.ecommerce" />
```

and:

```java
@Repository
public class CategoryDAO {
}
```

you don't need to manually define the DAO bean.

So you can remove:

```xml
<bean id="categoryDAO"
      class="com.ecommerce.dao.CategoryDAO">
</bean>
```

Spring creates it automatically.

---

# 8. Multiple DAOs

The same approach can be used for all your DAOs.

## CategoryDAO

```java
@Repository
public class CategoryDAO {

    private final JdbcTemplate template;

    public CategoryDAO(JdbcTemplate template) {
        this.template = template;
    }
}
```

## ProductDAO

```java
@Repository
public class ProductDAO {

    private final JdbcTemplate template;

    public ProductDAO(JdbcTemplate template) {
        this.template = template;
    }
}
```

## CartDAO

```java
@Repository
public class CartDAO {

    private final JdbcTemplate template;

    public CartDAO(JdbcTemplate template) {
        this.template = template;
    }
}
```

## OrderDAO

```java
@Repository
public class OrderDAO {

    private final JdbcTemplate template;

    public OrderDAO(JdbcTemplate template) {
        this.template = template;
    }
}
```

All of these DAOs can receive the same `JdbcTemplate` bean.

---

# 9. What Happens Inside Spring?

The overall process is:

```text
<context:component-scan>
            ↓
Spring scans com.ecommerce
            ↓
Finds @Repository
            ↓
Creates DAO objects
            ↓
DAO constructor requires JdbcTemplate
            ↓
Spring finds JdbcTemplate bean
            ↓
Spring injects JdbcTemplate
            ↓
DAO is ready to use ✅
```

---

# 10. Your XML Becomes Simpler

Instead of manually defining every DAO:

```xml
<bean id="categoryDAO" ... />

<bean id="productDAO" ... />

<bean id="cartDAO" ... />

<bean id="orderDAO" ... />
```

you can use component scanning:

```xml
<context:component-scan base-package="com.ecommerce" />
```

Then keep the infrastructure beans:

```xml
<bean id="dataSource"
      class="org.springframework.jdbc.datasource.DriverManagerDataSource">

    <!-- Database configuration -->

</bean>

<bean id="jdbcTemplate"
      class="org.springframework.jdbc.core.JdbcTemplate">

    <property name="dataSource" ref="dataSource"/>

</bean>
```

---

# 11. Why Constructor Injection?

Constructor injection has several advantages.

### 1. Dependency is clear

The constructor clearly shows what the DAO needs:

```java
public CategoryDAO(JdbcTemplate template)
```

### 2. `final` can be used

```java
private final JdbcTemplate template;
```

This means the dependency cannot be reassigned after construction.

### 3. Easier testing

You can create the DAO with a test or mock `JdbcTemplate`:

```java
CategoryDAO dao = new CategoryDAO(template);
```

### 4. Required dependency

The DAO cannot be created without its required dependency.

---

# 12. Do We Need `@Autowired`?

If your class has **only one constructor**, Spring can use that constructor for dependency injection without explicitly writing `@Autowired`.

So this is enough:

```java
@Repository
public class CategoryDAO {

    private final JdbcTemplate template;

    public CategoryDAO(JdbcTemplate template) {
        this.template = template;
    }
}
```

You don't necessarily need:

```java
@Autowired
public CategoryDAO(JdbcTemplate template) {
    this.template = template;
}
```

With a single constructor, Spring can automatically use it.

---

# 13. Complete Flow

The complete dependency flow is:

```text
Database
    ↑
DataSource
    ↑
JdbcTemplate
    ↑
@Repository DAO
    ↑
@Service
    ↑
@Controller
```

For example:

```text
Browser
   ↓
Controller
   ↓
Service
   ↓
CategoryDAO
   ↓
JdbcTemplate
   ↓
DataSource
   ↓
Database
```

---

# 14. Important Point

Remember these three things:

```text
@Repository
    ↓
Spring automatically detects the DAO
```

```text
Constructor Injection
    ↓
Spring provides required dependencies
```

```text
JdbcTemplate Bean
    ↓
Spring injects it into the DAO
```

---

# 15. Easy Example to Remember

Think of it like this:

```text
@Repository
     ↓
"Spring, manage this DAO."

Constructor
     ↓
"DAO needs JdbcTemplate."

JdbcTemplate Bean
     ↓
"Spring already has JdbcTemplate."

Spring
     ↓
"Here is the JdbcTemplate."

DAO
     ↓
Ready to access the database! ✅
```

---

# 16. In Short

Instead of doing this:

```xml
<bean id="categoryDAO"
      class="com.ecommerce.dao.CategoryDAO">
    <property name="template" ref="jdbcTemplate"/>
</bean>
```

Use:

```java
@Repository
public class CategoryDAO {

    private final JdbcTemplate template;

    public CategoryDAO(JdbcTemplate template) {
        this.template = template;
    }
}
```

And make sure component scanning is enabled:

```xml
<context:component-scan base-package="com.ecommerce"/>
```

And `JdbcTemplate` is configured as a Spring bean:

```xml
<bean id="jdbcTemplate"
      class="org.springframework.jdbc.core.JdbcTemplate">

    <property name="dataSource" ref="dataSource"/>

</bean>
```

---

# 🎯 Interview Answer

> **`@Repository` marks a class as a Spring DAO component. With component scanning, Spring automatically creates the DAO bean. If the DAO has a constructor that requires `JdbcTemplate`, Spring injects the available `JdbcTemplate` bean through constructor injection. Therefore, we don't need to manually define each DAO in XML.**

---

# 📝 Quick Revision

```text
@Component Scan
      ↓
Finds @Repository
      ↓
Creates DAO Bean
      ↓
Constructor requires JdbcTemplate
      ↓
Spring finds JdbcTemplate Bean
      ↓
Constructor Injection
      ↓
DAO Ready ✅
```

---

# 🎉 Thank You for Learning!

> 💡 **Write Less Configuration, Understand More, and Build More!** 🚀

You have now learned how:

```text
@Repository
      ↓
Component Scanning
      ↓
Constructor Injection
      ↓
JdbcTemplate
      ↓
Spring Dependency Injection
```

Remember:

> **Don't just read the code — write it, run it, break it, fix it, and understand it.** 💻🔥

Every error you face while learning is helping you become a better developer.

### 🚀 Keep Learning. Keep Coding. Keep Growing!

```text
Learn → Practice → Debug → Build → Improve → Succeed 🎯
```

**Happy Coding! ❤️**

See you in the next lesson! 👋😊

⭐ If these notes helped you, consider giving the repository a **Star ⭐** on GitHub.
