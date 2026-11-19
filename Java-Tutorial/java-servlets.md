# **Java Servlets – Complete Notes for Learning & Interviews**

These notes cover Servlet concepts from basics to interview level, including architecture, lifecycle, GET vs POST, complete working code, JDBC integration, and important interview questions with answers.

The examples use **Tomcat 9 and&#x20;**`javax.servlet`, matching the setup you've been working with in Eclipse.

**What these notes cover**

- Servlet fundamentals and architecture
- Servlet container and Apache Tomcat
- Servlet lifecycle
- GET vs POST
- Request and Response objects
- Servlet mapping and annotations
- Complete Login Servlet program
- JDBC integration
- Sessions and cookies
- 30 important interview questions with answers

# **1. Introduction to Servlets**

## **What is a Servlet?**

A **Servlet is a Java class that runs on a web server and handles client requests and generates responses.**

In simple words:

A Servlet acts as a middleman between the browser and the server-side application.

**Example: Login application**

1. User enters username and password.
2. Browser sends the information to the server.
3. Servlet receives the username and password.
4. Servlet validates the credentials.
5. Servlet sends a success or failure response.

### **Why do we need Servlets?**

HTML alone can display static content, but it cannot independently perform server-side processing.

We need Servlets for:

- Processing login and registration forms.
- Connecting with databases.
- Validating user information.
- Performing CRUD operations.
- Generating dynamic web pages.
- Managing user sessions.

**Static webpage:** Displays the same content to everyone.

**Dynamic webpage:** Displays different content depending on user input or database information.

# **2. Servlet Architecture**

[Medium](https://images.openai.com/static-rsc-4/-VrReTAeBa_NsOgM1pGoB1loxCVfGOLEeCaP7xDj_Y_xOlZWb-kablanicvMDc3l3pfH6SY8bORFh90wxXMaGAG6XFcEF8OlvkYGizrfHNG9JO51AQ-oCmFSfJtgiNdjTSpQn2V44AsKe3TwcU0I8L51FXtwkIJKAKgS50yUC3M?purpose=inline)

[medium.com](https://medium.com/%40ilakshitha7921/lankanet-building-the-future-of-internet-connectivity-mapping-with-blockchain-technology-d9b467773f90?utm_source=chatgpt.com)

### **How does Servlet architecture work?**

Consider a user opening a login application.

1. **Client:** Browser sends an HTTP request.
2. **Web server:** Receives the request.
3. **Servlet container:** Identifies the correct Servlet.
4. **Servlet:** Processes the request.
5. **Database:** Servlet can use JDBC to retrieve or store information.
6. **Response:** Servlet sends the result back to the browser.

### **Important components**

| Component         | Meaning                                  |
| ----------------- | ---------------------------------------- |
| Client            | Browser or application sending a request |
| Web server        | Receives HTTP requests                   |
| Servlet           | Java class that handles requests         |
| Servlet container | Manages Servlet execution and lifecycle  |
| Request           | Information sent by the client           |
| Response          | Information returned to the client       |
| HTTP              | Protocol used for communication          |

# **3. What is Apache Tomcat?**

[Apache Tomcat Consulting, Deployment & Application Server Optimization in US & India](https://images.openai.com/static-rsc-4/NZHoZnP4LDpdDiBoIcZTLOBYs1ahlmPCQ_PQbO23DXaNXiYEXEGXkesiu1e_-UmFD_mPzwL9Vt2puV_b15rNCrSMwL64AYtLGHPL1WlRALRvjkiY7Ew6tpUioswrA-4U0ZLJnTlGc-XI7yAAt8-mryGg9mNmV3wYd3xn8qKd7dg?purpose=inline)

**Apache Tomcat**

Tomcat is a web server and Servlet container used to run Java web applications.

### **Why do we need Tomcat?**

A normal Java program can run directly using JVM.

But a Servlet requires a Servlet container to:

- Create Servlet objects.
- Manage the Servlet lifecycle.
- Receive HTTP requests.
- Call the appropriate Servlet methods.
- Manage request and response objects.
- Handle multiple requests.

### **What is a Servlet Container?**

A Servlet container is an environment that manages the execution of Servlets.

**Examples:**

- Apache Tomcat
- Jetty
- Undertow

**Remember for interviews:**

Tomcat is not just a Java compiler. It provides the environment required to run and manage Java web applications.

# **4. Servlet API**

Servlet API provides the interfaces and classes needed to develop Servlets.

Some important classes and interfaces:

| Name                  | Type           | Purpose                                 |
| --------------------- | -------------- | --------------------------------------- |
| `Servlet`             | Interface      | Defines basic Servlet lifecycle methods |
| `GenericServlet`      | Abstract class | Protocol-independent Servlet base class |
| `HttpServlet`         | Abstract class | Provides HTTP-specific functionality    |
| `HttpServletRequest`  | Interface      | Reads client request information        |
| `HttpServletResponse` | Interface      | Sends response to client                |
| `ServletConfig`       | Interface      | Provides Servlet-specific configuration |
| `ServletContext`      | Interface      | Shares application-wide information     |

### **What is HttpServlet?**

`HttpServlet` is a class that extends `GenericServlet`.

We generally extend `HttpServlet` when creating a web application.

Example:

```
public class LoginServlet extends HttpServlet {

}
```

Why?

Because it provides HTTP-specific methods such as:

- `doGet()`
- `doPost()`
- `doPut()`
- `doDelete()`

# **5. Servlet Lifecycle**

The Servlet lifecycle describes how a Servlet is created, used, and destroyed.

[Servlet Life Cycle](https://images.openai.com/static-rsc-4/5ZFYIMFpJTJMa6AQkDyX3OPmbDIubB787QRmIlXAyYEFXFOpJ6vRlf1qkGE8q9A_TeW-1EKeUIK6_76obJo-WQPavVUJv7CLGUEug8sL_YyvG2jtkfekl-31Lyw6mb__BBzzFux1NV5itV8shZveDaZp_rMZ906WpmvHa3t2FOQ?purpose=inline)

[profcdpatel.blogspot.com](https://profcdpatel.blogspot.com/2019/11/servlet-life-cycle.html?utm_source=chatgpt.com)

There are three main lifecycle methods.

## **1. init()**

- Called only once when the Servlet is initialized.
- Used for initialization work.
- Called by the Servlet container.

```
public void init() throws ServletException {

    System.out.println("Servlet initialized");

}
```

## **2. service()**

- Called for each incoming request.
- Handles the request-processing process.
- In `HttpServlet`, it dispatches requests to methods such as `doGet()` and `doPost()`.

```
public void service(ServletRequest request,
                    ServletResponse response)
                    throws ServletException, IOException {

}
```

## **3. destroy()**

- Called when the Servlet is removed from service.
- Called only once during its normal lifecycle.
- Used for cleanup operations.

```
public void destroy() {

    System.out.println("Servlet destroyed");

}
```

### **Lifecycle sequence**

**1. init()**

Called once

**2. service()**

Called for every request

**3. destroy()**

Called before removal

**Important interview point:** The container normally creates one Servlet instance per Servlet declaration and handles multiple requests using multiple threads. Therefore, avoid storing request-specific information in Servlet instance variables.

# **6. GET vs POST**

These are two important HTTP request methods used in Servlets.

## **GET method**

GET is generally used to retrieve information from the server.

Example:

```
protected void doGet(HttpServletRequest request,
                     HttpServletResponse response)
                     throws IOException {

    PrintWriter out = response.getWriter();

    out.println("Welcome to my website");

}
```

Uses:

- Searching information.
- Fetching records.
- Displaying data.
- Opening webpages.

## **POST method**

POST is generally used to submit information to the server.

Example:

```
protected void doPost(HttpServletRequest request,
                      HttpServletResponse response)
                      throws IOException {

    String username = request.getParameter("username");

    PrintWriter out = response.getWriter();

    out.println("Welcome " + username);

}
```

Uses:

- Login forms.
- Registration forms.
- Submitting user information.
- Creating database records.

### **Difference between GET and POST**

| GET                            | POST                                                     |
| ------------------------------ | -------------------------------------------------------- |
| Used to retrieve data          | Used to submit data                                      |
| Parameters often appear in URL | Data is sent in request body                             |
| Can be bookmarked              | Form submission is not normally bookmarked with its data |
| Should not change server state | Can change server state                                  |
| Suitable for fetching data     | Suitable for submitting data                             |

**Interview point:** POST is not automatically secure. HTTPS is required to protect transmitted information.

# **7. HttpServletRequest and HttpServletResponse**

These are two of the most important Servlet interfaces.

## **A. HttpServletRequest**

It represents the request sent by the client.

We use it to retrieve information sent by the browser.

Example:

```
String username = request.getParameter("username");
```

Other useful methods:

| Method                 | Purpose                                   |
| ---------------------- | ----------------------------------------- |
| `getParameter()`       | Retrieves a form parameter                |
| `getParameterValues()` | Retrieves multiple values for a parameter |
| `getMethod()`          | Returns HTTP method                       |
| `getRequestURI()`      | Returns requested URI                     |
| `getHeader()`          | Retrieves request header                  |
| `getSession()`         | Gets or creates a session                 |

## **B. HttpServletResponse**

It represents the response sent back to the client.

Example:

```
response.setContentType("text/html");

PrintWriter out = response.getWriter();

out.println("<h1>Hello World</h1>");
```

Other useful methods:

| Method             | Purpose                       |
| ------------------ | ----------------------------- |
| `setContentType()` | Defines response content type |
| `getWriter()`      | Writes text response          |
| `sendRedirect()`   | Redirects the browser         |
| `setStatus()`      | Sets HTTP status code         |
| `addCookie()`      | Adds a cookie to response     |
| `setHeader()`      | Sets a response header        |

### **Understand the difference**

```
request.getParameter("username");
```

Means: Get the username sent by the client.

```
response.getWriter();
```

Means: Prepare to send output back to the client.

**Easy trick to remember:**

- Request = Incoming information.
- Response = Outgoing information.

# **8. Servlet Mapping**

Servlet mapping tells the server which Servlet should handle a particular URL.

There are two common ways.

## **Method 1: Using @WebServlet annotation**

```
@WebServlet("/login")
public class LoginServlet extends HttpServlet {

}
```

When the client accesses `/login`, Tomcat maps the request to this Servlet.

## **Method 2: Using web.xml**

The `web.xml` file is the deployment descriptor of a web application.

Example:

```
<web-app>

    <servlet>
        <servlet-name>LoginServlet</servlet-name>
        <servlet-class>
            com.demo.LoginServlet
        </servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>LoginServlet</servlet-name>
        <url-pattern>/login</url-pattern>
    </servlet-mapping>

</web-app>
```

### **Difference between annotation and web.xml**

| Annotation                     | web.xml                               |
| ------------------------------ | ------------------------------------- |
| Configuration inside Java code | Configuration in XML file             |
| Uses `@WebServlet`             | Uses servlet and servlet-mapping tags |
| Easy to configure              | Centralized configuration             |
| No XML mapping required        | Requires XML mapping                  |

# **9. Complete Servlet Interview Program – Login Application**

Important – Practice this code

**Interview question: Write a Servlet program to accept username and password from an HTML form and validate the credentials.**

We will use three things:

1. HTML – To collect username and password.
2. Servlet – To process the request.
3. Tomcat – To execute the Servlet.

## **Part A: HTML Login Page**

File name: `index.html`

```

<!DOCTYPE html>
<html>

<head>
    <title>Login Page</title>
</head>

<body>

    <h1>Login Application</h1>

    <form action="LoginServlet" method="post">

        Username:
        <input type="text" name="username" required>

        <br><br>

        Password:
        <input type="password" name="password" required>

        <br><br>

        <input type="submit" value="Login">

    </form>

</body>
</html>

```

### **Understand the important HTML lines**

```
<form action="LoginServlet" method="post">
```

- `action` specifies where the form data should go.
- `method` specifies how the data should be sent.

```
<input type="text" name="username">
```

The `name` attribute is important because the Servlet uses it to retrieve the value.

## **Part B: Java Servlet**

File name: `LoginServlet.java`

```

package com.demo;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/LoginServlet")

public class LoginServlet extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest request,
                          HttpServletResponse response)
                          throws ServletException, IOException {

        // Step 1: Get data from HTML form

        String username = request.getParameter("username");
        String password = request.getParameter("password");

        // Step 2: Set response type

        response.setContentType("text/html");

        // Step 3: Create PrintWriter

        PrintWriter out = response.getWriter();

        // Step 4: Validate credentials

        if ("admin".equals(username) && "1234".equals(password)) {

            out.println("<h1>Login Successful!</h1>");
            out.println("<h2>Welcome " + username + "</h2>");

        }

        else {

            out.println("<h1>Login Unsuccessful!</h1>");

        }

    }

}

```

## **Part C: Understand the complete flow**

**index.html**

User enters username and password

**POST request**

**Apache Tomcat**

Finds the matching URL mapping

**LoginServlet → doPost()**

Reads the form data using getParameter()

**Valid**

Login Successful

**Invalid**

Login Failed

### **How to explain this program in an interview**

You can explain it in these six steps:

1. I created an HTML form to collect username and password.
2. I used POST to submit the form data to the server.
3. I extended `HttpServlet` to create my Login Servlet.
4. I used `@WebServlet` to map the Servlet to the URL.
5. I used `request.getParameter()` to retrieve the form data.
6. I validated the credentials and used `PrintWriter` to send the response.

### **Expected output**

| Username | Password | Result             |
| -------- | -------- | ------------------ |
| admin    | 1234     | Login Successful   |
| user     | 1234     | Login Unsuccessful |
| admin    | 1111     | Login Unsuccessful |

*Note: This is a learning example. Real applications should validate users against a database and store passwords securely using password hashing.*

# **10. Servlet and JDBC Integration**

Since you've already studied JDBC, understand how both concepts work together.

Servlets can communicate with databases using JDBC.

**Example: Login validation using a database**

The process:

1. User enters login details.
2. Servlet receives username and password.
3. Servlet establishes a database connection using JDBC.
4. SQL query checks whether the user exists.
5. Servlet sends the result.

Example JDBC logic inside a Servlet:

```
String sql = "SELECT * FROM users WHERE username = ?";

PreparedStatement ps = con.prepareStatement(sql);

ps.setString(1, username);

ResultSet rs = ps.executeQuery();

if (rs.next()) {
    out.println("User Found");
} else {
    out.println("User Not Found");
}
```

**Important:** Use `PreparedStatement` instead of building SQL queries by joining user input. This helps prevent SQL injection.

In an actual application, database access should generally be placed in a separate DAO or service class instead of putting all JDBC logic directly inside the Servlet.

# **11. Session Management**

HTTP is a stateless protocol.

What does stateless mean?

It means the server does not automatically remember previous requests from the same user.

Example:

1. User logs in.
2. User opens another page.
3. Server needs a way to recognize that the user is already logged in.

We use session management for this.

### **What is HttpSession?**

`HttpSession` is used to store user-specific information across multiple requests.

Example:

```
HttpSession session = request.getSession();

session.setAttribute("username", "admin");
```

To retrieve:

```
String username = (String) session.getAttribute("username");
```

To invalidate:

```
session.invalidate();
```

### **Important session methods**

| Method              | Purpose                   |
| ------------------- | ------------------------- |
| `getSession()`      | Gets or creates a session |
| `setAttribute()`    | Stores information        |
| `getAttribute()`    | Retrieves information     |
| `removeAttribute()` | Removes an attribute      |
| `invalidate()`      | Ends the session          |
| `getId()`           | Returns session ID        |

# **12. Cookies**

A cookie is a small piece of information stored in the user's browser.

Cookies help the server recognize users or remember certain information across requests.

### **Create a cookie**

```
Cookie cookie = new Cookie("username", "admin");

response.addCookie(cookie);
```

### **Read cookies**

```
Cookie[] cookies = request.getCookies();

if (cookies != null) {

    for (Cookie c : cookies) {

        out.println(c.getName());

        out.println(c.getValue());

    }

}
```

### **Delete a cookie**

```
Cookie cookie = new Cookie("username", "");

cookie.setMaxAge(0);

response.addCookie(cookie);
```

### **Session vs Cookie**

| Session                                          | Cookie                                             |
| ------------------------------------------------ | -------------------------------------------------- |
| Data is generally stored on server               | Data is stored in browser                          |
| Identified using session ID                      | Sent through HTTP headers                          |
| Can store Java objects                           | Stores string values                               |
| Can be invalidated on server                     | Can be deleted or expired                          |
| Usually more suitable for server-side user state | Useful for preferences and remembering information |

# **13. Important Servlet Interview Questions and Answers**

These are questions you should prepare for fresher Java developer interviews.

## **Basic interview questions**

**1. What is a Servlet?**

A Servlet is a Java server-side component that receives client requests, processes them, and generates responses.

**2. Why do we use Servlets?**

We use Servlets to create dynamic web applications, process forms, communicate with databases, and manage user sessions.

**3. What is Apache Tomcat?**

Apache Tomcat is a web server and Servlet container that runs Java web applications.

**4. What is a Servlet container?**

A Servlet container is an environment responsible for managing Servlet lifecycle, request handling, response generation, and execution.

**5. What is Servlet API?**

Servlet API is a collection of Java interfaces and classes used to develop Servlet applications.

**6. What is HttpServlet?**

`HttpServlet` is an abstract class that extends `GenericServlet` and provides HTTP-specific request-handling methods.

**7. What is the difference between GenericServlet and HttpServlet?**

| GenericServlet                      | HttpServlet                      |
| ----------------------------------- | -------------------------------- |
| Protocol-independent                | Designed for HTTP                |
| Implements Servlet interface        | Extends GenericServlet           |
| Provides service() method           | Provides doGet(), doPost(), etc. |
| Can be used for different protocols | Used for HTTP requests           |

**8. What is the Servlet lifecycle?**

Servlet lifecycle has three main stages:

- `init()` – Initializes the Servlet.
- `service()` – Handles requests.
- `destroy()` – Cleans up before removal.

**9. How many times is init() called?**

Normally, once for a Servlet instance during its lifecycle.

**10. How many times is service() called?**

It is called for every request dispatched to the Servlet.

## **Intermediate interview questions**

**11. What is the difference between doGet() and doPost()?**

`doGet()` handles GET requests, generally used for retrieving information.

`doPost()` handles POST requests, generally used for submitting information.

**12. What is HttpServletRequest?**

It is an interface that represents the incoming HTTP request. It provides methods to access parameters, headers, sessions, and other request information.

**13. What is HttpServletResponse?**

It is an interface that represents the outgoing HTTP response. It provides methods to send content, set headers, and redirect users.

**14. What is getParameter()?**

It retrieves a single request parameter sent by the client.

Example:

```
String name = request.getParameter("username");
```

**15. What is getWriter()?**

It returns a `PrintWriter` used to send text content to the client.

**16. What is @WebServlet?**

It is an annotation used to configure a Servlet and map it to a URL.

Example:

```
@WebServlet("/login")
```

**17. What is web.xml?**

It is the deployment descriptor used to configure a Java web application, including Servlet mappings and other application settings.

**18. What is the difference between sendRedirect() and forward()?**

| sendRedirect()                   | forward()                         |
| -------------------------------- | --------------------------------- |
| Redirects browser to another URL | Transfers request internally      |
| Browser makes a new request      | Same request is forwarded         |
| URL changes in browser           | URL generally remains unchanged   |
| Can redirect to another website  | Works within the same application |

Example:

```
response.sendRedirect("home.jsp");
```

Forward example:

```
RequestDispatcher rd =
    request.getRequestDispatcher("home.jsp");

rd.forward(request, response);
```

**19. Can a Servlet handle multiple requests?**

Yes. The Servlet container can process multiple requests concurrently using multiple threads.

**20. Is a Servlet thread-safe?**

Not automatically. Multiple threads can access the same Servlet instance. We should avoid shared mutable instance variables or use proper thread-safe handling.

## **Advanced and scenario-based questions**

**21. What is ServletConfig?**

ServletConfig provides configuration information specific to an individual Servlet.

**22. What is ServletContext?**

ServletContext represents the entire web application and allows information to be shared across Servlets.

**23. Difference between ServletConfig and ServletContext?**

| ServletConfig                         | ServletContext                        |
| ------------------------------------- | ------------------------------------- |
| Servlet-specific                      | Application-wide                      |
| One per Servlet declaration           | One per web application               |
| Stores Servlet-specific configuration | Stores shared application information |

**24. What is HttpSession?**

HttpSession is used to maintain user-specific information across multiple HTTP requests.

**25. What is a cookie?**

A cookie is a small piece of data stored in the browser and sent with applicable requests to the server.

**26. What is the difference between session and cookie?**

Session data is generally stored on the server, whereas cookie data is stored in the browser.

**27. What happens if we directly open a Servlet URL when only doPost() is implemented?**

The browser sends a GET request. Since the Servlet does not support GET, the server returns HTTP 405 Method Not Allowed.

**28. Can we use try without catch in Java Servlets?**

Yes. Java allows `try` with `finally` without a `catch` block, and also supports try-with-resources.

**29. What is loadOnStartup in @WebServlet?**

It specifies whether the Servlet should be initialized when the application starts instead of waiting for the first request.

Example:

```
@WebServlet(value = "/login", loadOnStartup = 1)
```

**30. What is the difference between Servlet and JSP?**

| Servlet                                        | JSP                                  |
| ---------------------------------------------- | ------------------------------------ |
| Java class                                     | HTML-oriented page with JSP features |
| Better suited for request processing           | Better suited for presenting views   |
| HTML is written through Java code or templates | HTML can be written directly         |
| Used for controller logic                      | Commonly used for view rendering     |

# **14. Important Servlet Methods – Quick Revision**

| Method             | Purpose                    |
| ------------------ | -------------------------- |
| `init()`           | Initialize Servlet         |
| `service()`        | Process requests           |
| `destroy()`        | Destroy Servlet            |
| `doGet()`          | Handle GET request         |
| `doPost()`         | Handle POST request        |
| `getParameter()`   | Get form data              |
| `getWriter()`      | Send text response         |
| `setContentType()` | Set response content type  |
| `sendRedirect()`   | Redirect browser           |
| `getSession()`     | Get or create session      |
| `setAttribute()`   | Store session/request data |
| `getAttribute()`   | Retrieve stored data       |
| `invalidate()`     | End session                |

# **15. Common Servlet Errors**

These are useful to know because you have already encountered the 405 error.

| Error                                | Possible reason                            | Solution                                       |
| ------------------------------------ | ------------------------------------------ | ---------------------------------------------- |
| 404 Not Found                        | Incorrect URL mapping                      | Check `@WebServlet` and URL                    |
| 405 Method Not Allowed               | Request method not supported               | Implement matching `doGet()` or `doPost()`     |
| 500 Internal Server Error            | Exception inside Servlet                   | Check Eclipse console and stack trace          |
| ClassNotFoundException               | Missing class or dependency                | Check build path and deployment                |
| NoClassDefFoundError                 | Required class unavailable at runtime      | Check server runtime and libraries             |
| Package javax.servlet not accessible | Servlet API dependency/configuration issue | Check Tomcat runtime and project configuration |

**Important version difference:**

| Tomcat     | Servlet API package |
| ---------- | ------------------- |
| Tomcat 9   | `javax.servlet`     |
| Tomcat 10+ | `jakarta.servlet`   |

Do not mix the two APIs in the same application.

# **16. Interview Preparation – What to Practice**

Here is the order I recommend for revising Servlets.

**Servlet revision tracker**

0/10 completed

Understand Servlet architecture and request-response flow

Understand Tomcat and Servlet container

Learn Servlet lifecycle (init, service, destroy)

Practice doGet() and doPost()

Understand HttpServletRequest and HttpServletResponse

Learn Servlet mapping using @WebServlet

Write the complete Login Servlet without looking at notes

Practice Servlet and JDBC integration

Understand HttpSession and Cookies

Revise all 30 interview questions

Reset progress

## **17. Final Interview Self-Test**

Try answering these without looking at the notes.

1. Explain the complete working of a Servlet.
2. Why do we extend HttpServlet?
3. Explain the Servlet lifecycle.
4. What is the difference between GET and POST?
5. What is the purpose of HttpServletRequest?
6. What is the purpose of HttpServletResponse?
7. Why do we need Tomcat?
8. What happens when a user directly opens a Servlet URL?
9. What is the difference between ServletConfig and ServletContext?
10. Write a complete Login Servlet program.

**My advice:** Don't just memorize the 30 answers. Make sure you can write the Login Servlet from memory and explain every line. If you can do that, you will have a solid foundation for basic Servlet interview questions.
