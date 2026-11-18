# **JDBC – Quick Revision Notes (Java)**

These notes cover the important JDBC concepts, syntax, methods and interview points. You can use them for quick revision before interviews.

## **1. What is JDBC?**

**JDBC = Java Database Connectivity**

- JDBC is a Java API used to connect Java applications with databases.
- It allows Java to execute SQL queries.
- Used for CRUD operations:
  - Create – INSERT
  - Read – SELECT
  - Update – UPDATE
  - Delete – DELETE

**Example:** Connecting a Java application to a MySQL database to store and retrieve student details.

## **2. JDBC Architecture**

[The JDBC Connectivity Model. What is JDBC Connectivity Model and… | by Blogs Craze | Medium](https://images.openai.com/static-rsc-4/B7pYyJoJvfkiCJ7zhso48HS_o1MqqUh5eTD7MiNsmDUcSMaOJLEWMfQkWqpPa36p4m2D0WJl4K_0oDEmk656ctYC8FH2cIPiz1OAVvcI2G0gngm2g4D1xoZ7j9ZpyIc2RmAgKxSUxtQJmAa0NlN4MAhHXj15cKMWHsgVw6I831E?purpose=inline)

[medium.com](https://medium.com/%40blogscraze/the-jdbc-connectivity-model-8f0cb20146d4?utm_source=chatgpt.com)

Communication flow:

1. Java Application sends a request.
2. JDBC API provides common interfaces.
3. JDBC Driver translates and communicates with the database.
4. Database executes SQL and returns results.

**Remember:** JDBC provides the standard API; database vendors provide the drivers.

## **3. JDBC vs JDBC Driver**

| JDBC                            | JDBC Driver                       |
| ------------------------------- | --------------------------------- |
| Java API                        | Database-specific implementation  |
| Provides interfaces and classes | Implements JDBC interfaces        |
| Common for all databases        | Different for different databases |
| Part of Java                    | Provided by database vendors      |

Examples:

- MySQL Connector/J
- Oracle JDBC Driver
- PostgreSQL JDBC Driver

## **4. Important JDBC Classes and Interfaces**

| Component         | Type      | Purpose                            |
| ----------------- | --------- | ---------------------------------- |
| DriverManager     | Class     | Establishes database connection    |
| Connection        | Interface | Represents database connection     |
| Statement         | Interface | Executes static SQL queries        |
| PreparedStatement | Interface | Executes parameterized SQL queries |
| ResultSet         | Interface | Stores SELECT query results        |

### **DriverManager**

Used to establish a database connection.

```
Connection con = DriverManager.getConnection(
    url, username, password
);
```

### **Connection**

- Represents an active database connection.
- Used to create Statement and PreparedStatement.
- Used to manage transactions.

```
Connection con = DriverManager.getConnection(url, user, pass);
```

**Why is Connection an interface?**

Different database drivers implement the same Connection interface. This provides database independence.

### **Statement**

Used to execute static SQL queries.

```
Statement stmt = con.createStatement();

stmt.executeUpdate("INSERT INTO student VALUES(1,'John')");
```

### **PreparedStatement**

Used to execute parameterized SQL queries.

- Uses `?` placeholders.
- Helps prevent SQL injection.
- Useful for dynamic values.

```
PreparedStatement ps = con.prepareStatement(
    "INSERT INTO student VALUES (?, ?)"
);

ps.setInt(1, 1);
ps.setString(2, "John");

ps.executeUpdate();
```

### **ResultSet**

- Stores data returned by SELECT queries.
- Uses a cursor to read records one by one.
- `next()` moves the cursor to the next row.

```
ResultSet rs = stmt.executeQuery("SELECT * FROM student");

while(rs.next()) {
    System.out.println(rs.getInt("id"));
    System.out.println(rs.getString("name"));
}
```

## **5. Statement vs PreparedStatement**

| Statement                          | PreparedStatement                  |
| ---------------------------------- | ---------------------------------- |
| Static SQL                         | Parameterized SQL                  |
| No placeholders required           | Uses `?` placeholders              |
| Values directly included in query  | Values passed using setter methods |
| More vulnerable to SQL injection   | Helps prevent SQL injection        |
| Suitable for simple static queries | Suitable for dynamic queries       |

**Interview point:** Prefer PreparedStatement when passing user input into SQL queries.

## **6. JDBC Execution Methods**

Very important for interviews!

| Method            | Purpose                | Return type |
| ----------------- | ---------------------- | ----------- |
| `executeQuery()`  | SELECT                 | ResultSet   |
| `executeUpdate()` | INSERT, UPDATE, DELETE | int         |
| `execute()`       | General SQL execution  | boolean     |

Examples:

```
// SELECT
ResultSet rs = stmt.executeQuery("SELECT * FROM student");

// INSERT
int rows = stmt.executeUpdate(
    "INSERT INTO student VALUES(1,'John')"
);

// General execution
boolean result = stmt.execute("SELECT * FROM student");
```

**Remember:**

- `executeQuery()` → Retrieves records.
- `executeUpdate()` → Returns affected row count.
- `execute()` → Returns true if the first result is a ResultSet; false otherwise.

## **7. Steps to Establish JDBC Connectivity**

Remember this sequence:

1. **Import JDBC package**

   `import java.sql.*;`
2. **Establish connection**

   `DriverManager.getConnection()`
3. **Create statement**

   `con.createStatement()`
4. **Execute query**

   `executeQuery()` / `executeUpdate()`
5. **Process results**

   `rs.next()`
6. **Close resources**

   Close ResultSet, Statement and Connection.

## **8. SQL Injection**

**Definition:** SQL injection is a security attack where malicious SQL input is used to manipulate database queries.

Example of unsafe code:

```
String query = "SELECT * FROM users WHERE name = '" + name + "'";
```

An attacker could enter SQL syntax as input and change the query's meaning.

**Solution: PreparedStatement**

```
PreparedStatement ps = con.prepareStatement(
    "SELECT * FROM users WHERE name = ?"
);

ps.setString(1, name);
```

PreparedStatement treats the input as a value instead of executable SQL syntax.

## **9. Why close database connections?**

- Releases database resources.
- Prevents connection leaks.
- Frees connections for other users.
- Improves application resource management.

Example:

```
con.close();
```

**Best practice:** Use try-with-resources to automatically close JDBC resources.

```
try (Connection con = DriverManager.getConnection(url, user, pass);
     PreparedStatement ps = con.prepareStatement("SELECT * FROM student");
     ResultSet rs = ps.executeQuery()) {

    while (rs.next()) {
        System.out.println(rs.getString("name"));
    }

} catch (SQLException e) {
    e.printStackTrace();
}
```

## **10. Important JDBC methods – One-page revision**

| Method               | Meaning                              |
| -------------------- | ------------------------------------ |
| `getConnection()`    | Establish database connection        |
| `createStatement()`  | Create Statement object              |
| `prepareStatement()` | Create PreparedStatement object      |
| `executeQuery()`     | Execute SELECT                       |
| `executeUpdate()`    | Execute INSERT, UPDATE, DELETE       |
| `execute()`          | Execute general SQL                  |
| `next()`             | Move to next ResultSet row           |
| `getInt()`           | Retrieve integer column              |
| `getString()`        | Retrieve string column               |
| `setInt()`           | Set integer parameter                |
| `setString()`        | Set string parameter                 |
| `close()`            | Close resource                       |
| `commit()`           | Save transaction changes             |
| `rollback()`         | Undo uncommitted transaction changes |

## **11. Last-minute interview revision**

These are the concepts you should be able to explain without looking at notes:

- What is JDBC?
- JDBC architecture and communication flow.
- JDBC vs JDBC Driver.
- DriverManager and Connection.
- Why Connection is an interface.
- Statement vs PreparedStatement.
- ResultSet and `next()`.
- Three execution methods.
- Six steps of JDBC connectivity.
- SQL injection and its prevention.
- Why closing connections is important.
- Basic CRUD operations.

**Next topic to learn:** JDBC CRUD operations using MySQL. Practice INSERT, SELECT, UPDATE and DELETE with Java code. This will help you understand all the above concepts practically.
