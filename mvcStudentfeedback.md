# Project Structure

```text
mvcStudentfeedback
│
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   │   ├── com.spring.studentController
│   │   │   │   └── StudentController.java
│   │   │   └── com.spring.studentRepository
│   │   │       └── StudentRepository.java
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
This project uses a MySQL database to store student feedback submitted through the web application. The `student_feedback` table stores the student's name, course name, feedback, and submission date.

```sql
CREATE DATABASE mvcdb;

USE mvcdb;

CREATE TABLE student_feedback (
    feedback_id INT AUTO_INCREMENT PRIMARY KEY,
    student_name VARCHAR(100) NOT NULL,
    course_name VARCHAR(100) NOT NULL,
    feedback_text TEXT NOT NULL,
    submission_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

SELECT * FROM student_feedback;
```

---

# 📄 StudentController.java

### Description
The **StudentController.java** file is the Spring MVC Controller of the application. It receives feedback form submissions, processes the request, interacts with the repository layer, and returns the appropriate JSP page.

```java
package com.spring.studentController;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.spring.studentRepository.StudentRepository;

@Controller
public class StudentController {

	
	
	@GetMapping("/")
	public String index()
	{
		return "index";
	}
	
	@Autowired
	private StudentRepository st;
	@PostMapping("/save")
	public String savestudent(@RequestParam String name ,@RequestParam String course , @RequestParam String feedback)
	{
		
		st.save(name, course, feedback);
		return "welcome";
		
	}
	
}

```

---

# 📄 StudentRepository.java

### Description
The **StudentRepository.java** file is responsible for performing database operations on the `student_feedback` table, such as inserting and retrieving student feedback.

```java
package com.spring.studentRepository;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;
 
@Repository
public class StudentRepository 
{

	@Autowired
	private JdbcTemplate jdbcTemplate;
	
	public void save(String name , String course , String feedback)
	{
		
 
		        
		        String sql = "insert into student_feedback(Student_name,course_name,feedback_text ) values(?,?,?)";
		        
		        try {
		        	
		        jdbcTemplate.update(sql, name,course,feedback );
		        
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
The **index.jsp** file is the application's home page. It contains the student feedback form where users enter their name, course name, and feedback.

```jsp
 <%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Feedback Form</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f7f6;
            padding: 20px;
        }
        .form-container {
            max-width: 500px;
            background: #ffffff;
            padding: 30px;
            margin: 0 auto;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        h2 {
            text-align: center;
            color: #333;
            margin-bottom: 20px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            font-weight: bold;
            margin-bottom: 5px;
            color: #555;
        }
        input[type="text"], select, textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
            font-size: 14px;
        }
        textarea {
            resize: vertical;
            height: 120px;
        }
        button {
            width: 100%;
            background-color: #4CAF50;
            color: white;
            padding: 12px;
            border: none;
            border-radius: 4px;
            font-size: 16px;
            cursor: pointer;
            font-weight: bold;
        }
        button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>

<div class="form-container">
    <h2>Course Feedback Form</h2>
    
    <form action="save" method="POST">
        <!-- Student Name Field -->
        <div class="form-group">
            <label for="student-name">Student Name:</label>
            <input type="text" id="student-name" name="name" placeholder="Enter your full name" required>
        </div>

        <!-- Course Name Field -->
        <div class="form-group">
            <label for="course-name">Course Name:</label>
            <select id="course-name" name="course" required>
                <option value="" disabled selected>-- Select Your Course --</option>
                <option value="web-development">Web Development</option>
                <option value="data-science">Data Science</option>
                <option value="cyber-security">Cyber Security</option>
                <option value="digital-marketing">Digital Marketing</option>
            </select>
        </div>

        <!-- Feedback Field -->
        <div class="form-group">
            <label for="feedback">Your Feedback:</label>
            <textarea id="feedback" name="feedback" placeholder="Provide your thoughts on the course..." required></textarea>
        </div>

        <!-- Submit Button -->
        <button type="submit">Submit Feedback</button>
    </form>
</div>

</body>
</html>

```

---

# 📄 welcome.jsp

### Description
The **welcome.jsp** file displays a confirmation message after the student feedback has been submitted successfully.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
successfully inserted <br>
<a href ="/mvcStudentfeedback/" ><b>add another response</b> </a>
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

<context:component-scan base-package="com.spring.studentController,com.spring.studentRepository"/>
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
  <artifactId>mvcStudentfeedback</artifactId>
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
        <finalName>mvcStudentfeedback</finalName>
    </build>
</project>
```

---
