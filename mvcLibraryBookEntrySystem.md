# Project Structure

```text
mvcLibraryBookEntrySystem
│
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   │   ├── com.library.controller
│   │   │   │   └── LibraryController.java
│   │   │   └── com.library.repository
│   │   │       └── LibraryRepository.java
│   │   ├── resources
│   │   └── webapp
│   │       ├── WEB-INF
│   │       │   ├── pages
│   │       │   │   ├── index.jsp
│   │       │   │   └── welcome.jsp
│   │       │   └── SpringServlet.xml
│   │       └── web.xml
│   └── test
│       ├── java
│       └── resources
└── target
```

---

# 🗄️ Database

### Description
This project uses a MySQL database named **mvc**. The `books` table stores information about library books, including the book name, author, and price.

```sql
CREATE DATABASE mvc;

USE mvc;

CREATE TABLE books(
    id INT PRIMARY KEY AUTO_INCREMENT,
    bookname VARCHAR(100),
    author VARCHAR(100),
    price DOUBLE
);

SELECT * FROM books;
```

---

# 📄 LibraryController.java

### Description
The **LibraryController.java** file is the Spring MVC Controller of the application. It handles HTTP requests, collects user input from the JSP pages, communicates with the repository layer, and returns the appropriate view.

```java
package com.library.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.library.repository.LibraryRepository;

@Controller
public class LibraryController

{

	
	@GetMapping("/")
	public String home()
	{
		return "index";
	}
	
	@Autowired
	private LibraryRepository li;
	@PostMapping("/save")
	public String savebook(@RequestParam String bookName,@RequestParam String authorName, @RequestParam int price)
	{
		li.insertbook(bookName, authorName, price);


		System.out.println("bookName:"+bookName);
		System.out.println("authorName:"+authorName);
		System.out.println("price:"+price);
		return "welcome";
	}
	
}

```

---

# 📄 LibraryRepository.java

### Description
The **LibraryRepository.java** file is responsible for interacting with the MySQL database. It performs CRUD (Create, Read, Update, Delete) operations on the `books` table.

```java
package com.library.repository;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository
public class LibraryRepository
{

	@Autowired 
	private JdbcTemplate jdbcTemplate;
	
	public void insertbook(String bookname, String author ,int price)
	{
		String sql = "insert into books (bookname,author,price) values(?,?,?)";
		
		try {
			
		
		jdbcTemplate.update(sql,bookname,author,price);
		
		}
		 catch(Exception e)
		{
			 e.getStackTrace();
			 System.out.println(e.getMessage());
		}
	}
	
}

```

---

# 📄 index.jsp

### Description
The **index.jsp** file is the application's home page. It provides a form for entering book details such as the book name, author, and price.

```jsp
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Add New Book</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 50px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        input[type="text"], input[type="number"] {
            width: 300px;
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        input[type="submit"] {
            padding: 10px 15px;
            background-color: #007BFF;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        input[type="submit"]:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>

    <h2>Book Information Form</h2>

    <!-- The <form> container manages data collection and submission -->
    <form action="save" method="POST">
        
        <!-- Book Name Field -->
        <div class="form-group">
            <label for="bookName">Book Name:</label>
            <input type="text" id="bookName" name="bookName" placeholder="Enter book title" required>
        </div>

        <!-- Author Name Field -->
        <div class="form-group">
            <label for="authorName">Author Name:</label>
            <input type="text" id="authorName" name="authorName" placeholder="Enter author's full name" required>
        </div>

        <!-- Price Field -->
        <div class="form-group">
            <label for="bookPrice">Price ($):</label>
            <!-- Using type="number" with "step" allows decimals for cents -->
            <input type="number" id="bookPrice" name="price" min="0" step="0.01" placeholder="0.00" required>
        </div>

        <!-- Submit Button -->
        <div class="form-group">
            <input type="submit" value="Save Book Information">
        </div>

    </form>

</body>
</html>

```

---

# 📄 welcome.jsp

### Description
The **welcome.jsp** file displays a confirmation message or the submitted book details after successful form submission.

```jsp
<%@ page language="java"
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Regestration</title>
</head>
<body>
 <h2> Student Regestration Successfull</h2>
 <a href="/mvcLibraryBookEntrySystem/">add Another book</a>
 
    
</body>
</html>
```

---

# 📄 SpringServlet.xml

### Description
The **SpringServlet.xml** file contains the Spring MVC configuration, including component scanning, bean definitions, and the view resolver.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans.xsd

       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd

       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

<context:component-scan base-package="com.library.controller,com.library.repository"/>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">

        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>

    </bean>
    
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
       <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
       <property name="url" value="jdbc:mysql://localhost:3306/mvcdb"/>
       <property name="username" value="root"/>
       <property name="password" value="Prasanna@2007"/>
    </bean>
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate"  autowire="byType">
  
    </bean>
    

    <mvc:annotation-driven/>

</beans>
```

---

# 📄 web.xml

### Description
The **web.xml** file is the deployment descriptor of the application. It configures the `DispatcherServlet` and maps incoming requests to the Spring MVC framework.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="
         http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <servlet>
        <servlet-name>Spring</servlet-name>

        <servlet-class>
            org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
          <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/SpringServlet.xml</param-value>
    </init-param>
        

        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>Spring</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```

---

# 📄 pom.xml

### Description
The **pom.xml** file is the Maven Project Object Model (POM). It manages project dependencies, plugins, Java version, and build configuration.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>mvc</groupId>
  <artifactId>mvcLibraryBookEntrySystem</artifactId>
  <version>0.0.1-SNAPSHOT</version>
 




 
  <packaging>war</packaging>

    <dependencies>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.32</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.3.32</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.33</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>

    </dependencies>

    <build>
        <finalName>mvcLibraryBookEntrySystem</finalName>
    </build>

</project>
```

---
