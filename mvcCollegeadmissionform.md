# Project Structure

```text
mvcCollegeAdmissionForm
│
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   │   ├── com.college.controller
│   │   │   │   └── CollegeController.java
│   │   │   └── com.college.repository
│   │   │       └── CollegeRepository.java
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

# 📄 CollegeController.java

### Description
The **CollegeController.java** file is the Spring MVC Controller of the application. It handles incoming HTTP requests, processes user input, interacts with the repository layer, and returns the appropriate JSP view.

```java
package com.college.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.college.repository.CollegeRepository;
@Controller
public class CollegeController
{

	@GetMapping("/")
	public String home()
	{
		return "index";
	}
	
	@Autowired
	private CollegeRepository cc;
	@PostMapping("/save")
	public String savebook(@RequestParam String name,@RequestParam String email,@RequestParam String course,  @RequestParam int phone, @RequestParam String address)
	
	{
		cc.insertbook(name, email,course,  phone,address);


		System.out.println("name"+name);
		System.out.println(" email:"+ email);
		System.out.println("phone"+phone);
		System.out.println("address"+address);
		return "welcome";
	}
	
}

```

---

# 📄 CollegeRepository.java

### Description
The **CollegeRepository.java** file is responsible for interacting with the database. It performs database operations such as inserting, retrieving, updating, and deleting college admission records.

```java
package com.college.repository;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;


@Repository
public class CollegeRepository {
	@Autowired 
	private JdbcTemplate jdbcTemplate;
	public void insertbook(String name, String email, String course ,int phone, String address) 
	
	{
	 
		 
			String sql = "insert into CollegeAdmissions (name,email,phone,course,address) values(?,?,?,?,?)";
			
			try {
				
			
			jdbcTemplate.update(sql,name,email,phone,course,address);
			
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
The **index.jsp** file is the application's home page. It provides the user interface where users can enter their admission details.

```jsp
<%-- <!DOCTYPE HTML>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Student Registration Form</title>
    <style>
        body { font-family: Arial, sans-serif; background-color: #f4f4f9; padding: 20px; }
        .form-container { max-width: 500px; background: white; padding: 30px; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); margin: 0 auto; }
        .form-group { margin-bottom: 15px; }
        .form-group label { display: block; margin-bottom: 5px; font-weight: bold; }
        .form-group input, .form-group select, .form-group textarea { width: 100%; padding: 10px; border: 1px solid #ccc; border-radius: 4px; box-sizing: border-box; }
        .btn-submit { background-color: #007bff; color: white; padding: 12px 20px; border: none; border-radius: 4px; cursor: pointer; width: 100%; font-size: 16px; }
        .btn-submit:hover { background-color: #0056b3; }
    </style>
</head>
<body>

<div class="form-container">
    <h2>Student Registration</h2>
    <form action="save" method="POST">
        
        <!-- Student Name Field -->
        <div class="form-group">
            <label for="studentName">Student Name:</label>
            <input type="text" id="studentName" name="name" placeholder="Enter full name" required>
        </div>

        <!-- Email Field -->
        <div class="form-group">
            <label for="studentEmail">Email Address:</label>
            <input type="email" id="studentEmail" name="email" placeholder="example@domain.com" required>
        </div>

        <!-- Phone Number Field -->
        <div class="form-group">
            <label for="studentPhone">Phone Number:</label>
            <input type="tel" id="studentPhone" name="phone" placeholder="123-456-7890" pattern="[0-9]{10,15}" required>
        </div>

        <!-- Course Dropdown Field -->
        <div class="form-group">
            <label for="studentCourse">Select Course:</label>
            <select id="studentCourse" name="course" required>
                <option value="" disabled selected>-- Choose a Course --</option>
                <option value="computer_science">Computer Science</option>
                <option value="data_science">Data Science</option>
                <option value="business_administration">Business Administration</option>
                <option value="mechanical_engineering">Mechanical Engineering</option>
            </select>
        </div>

        <!-- Address Field -->
        <div class="form-group">
            <label for="studentAddress">Address:</label>
            <textarea id="studentAddress" name="address" rows="4" placeholder="Enter complete home address" required></textarea>
        </div>

        <!-- Submit Button -->
        <button type="submit" class="btn-submit">Submit Registration</button>
        
    </form>
</div>

</body>
</html>
 --%>
```

---

# 📄 welcome.jsp

### Description
The **welcome.jsp** file displays the submitted admission details or a success message after the form has been processed.

```jsp
<%--<%@ page language="java"
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
 <a href="/mvcCollegeAdmissionForm/">add Another student form</a>
 
    
</body>
</html> --%>
```

---

# 📄 SpringServlet.xml

### Description
The **SpringServlet.xml** file contains the Spring MVC configuration. It defines component scanning, the view resolver, and other required Spring beans.

```xml
<!-- <?xml version="1.0" encoding="UTF-8"?>

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

<context:component-scan base-package="com.college.controller,com.college.repository"/>
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

</beans> -->
```

---

# 📄 web.xml

### Description
The **web.xml** file is the deployment descriptor of the application. It configures the `DispatcherServlet` and maps incoming requests to the Spring MVC framework.

```xml
<!--<?xml version="1.0" encoding="UTF-8"?>

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

</web-app>  -->
```

---

# 📄 pom.xml

### Description
The **pom.xml** file is the Maven configuration file. It manages project dependencies, plugins, Java version, and build settings.

```xml
<!-- <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>mvc</groupId>
  <artifactId>mvcCollegeAdmissionForm</artifactId>
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
        <finalName>mvcCollegeAdmissionForm</finalName>
    </build>

</project> -->
```

---
