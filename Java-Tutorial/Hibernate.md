# **Hibernate – Quick Revision Notes (Java + PostgreSQL)**

These notes cover the important Hibernate concepts, configuration, annotations, CRUD operations, and interview questions. They are written for quick revision, assuming you're learning Hibernate using plain Java before moving to Spring Boot.

# **1. What is Hibernate?**

**Hibernate is an ORM (Object Relational Mapping) framework for Java.**

It helps Java applications communicate with databases without writing SQL for every operation.

For example:

- Java class → Database table
- Java object → Database row
- Java variable → Database column

**Example:**

Java class:

```
public class User {
    private int id;
    private String name;
    private String email;
}
```

Hibernate maps it to a database table:

| id | name | email           |
| -- | ---- | --------------- |
| 1  | John | john\@gmail.com |
| 2  | Alex | alex\@gmail.com |

### **Why use Hibernate?**

1. Reduces manual SQL writing.
2. Automatically maps Java objects to database records.
3. Supports CRUD operations.
4. Provides transaction management.
5. Supports relationships between tables.
6. Supports database-independent code through dialects.

### **JDBC vs Hibernate**

| JDBC                   | Hibernate                    |
| ---------------------- | ---------------------------- |
| We write SQL manually  | Generates SQL automatically  |
| We handle `ResultSet`  | Maps results to Java objects |
| More boilerplate code  | Less boilerplate code        |
| Manual object mapping  | Automatic object mapping     |
| Low-level database API | ORM framework built on JDBC  |

**Remember:** Hibernate internally uses JDBC to communicate with the database.

# **2. Hibernate Architecture**

[Overview, Architecture, and Configuration of Hibernate](https://images.openai.com/static-rsc-4/8QLWoAQ-aVn7Z7CTBbF68ZICYBjXKKriQARgkcCcNn4L9QX8Se7qiDKMYU-rlsEzPwvopUDY1f6OHX-mReqeZHCZxo-7icYusIDTTtcX2HuZcCtjGCRKGvv1z4YZFaTA4sTkkMy7s60Rgv1tmamhHfGS50W7uXqr0GcId3FtgV0?purpose=inline)

[javadoubts.com](https://javadoubts.com/hibernate-overview-architecture-configuration/?utm_source=chatgpt.com)

The flow is:

1. Java application sends an operation.
2. Hibernate processes the operation.
3. Hibernate generates SQL.
4. JDBC driver sends SQL to PostgreSQL.
5. PostgreSQL executes the query.
6. Hibernate converts the result into Java objects.

# **3. Important Hibernate Objects**

These are the most important objects you should remember.

| Object           | Purpose                       |
| ---------------- | ----------------------------- |
| `Configuration`  | Reads Hibernate configuration |
| `SessionFactory` | Creates Session objects       |
| `Session`        | Performs database operations  |
| `Transaction`    | Commits or rolls back changes |
| `Query`          | Executes HQL or other queries |

### **3.1 Configuration**

Used to load Hibernate configuration.

```
Configuration configuration = new Configuration();
configuration.configure("hibernate.cfg.xml");
```

It reads database properties, Hibernate settings, and entity mappings.

### **3.2 SessionFactory**

- Creates Session objects.
- Heavyweight object.
- Usually created only once per database configuration.
- Thread-safe.
- Should be closed when the application shuts down.

```
SessionFactory factory = new Configuration()
        .configure("hibernate.cfg.xml")
        .buildSessionFactory();
```

### **3.3 Session**

- Represents a connection between Java application and database operations.
- Used to save, retrieve, update, and delete objects.
- Lightweight.
- Not thread-safe.
- Should be opened when needed and closed afterward.

```
Session session = factory.openSession();
```

### **3.4 Transaction**

A transaction ensures database operations are completed successfully.

```
Transaction tx = session.beginTransaction();

tx.commit();
```

If something goes wrong:

```
tx.rollback();
```

**Remember:** Always use transactions for database modifications.

# **4. Hibernate Project Configuration**

### **Maven dependencies**

Add these dependencies to `pom.xml`:

```
<dependencies>

    <dependency>
        <groupId>org.hibernate.orm</groupId>
        <artifactId>hibernate-core</artifactId>
        <version>6.6.15.Final</version>
    </dependency>

    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <version>42.7.7</version>
    </dependency>

</dependencies>
```

### **Hibernate configuration file**

File name: `hibernate.cfg.xml`

Place it inside:

`src/main/resources/hibernate.cfg.xml`

```
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>

    <session-factory>

        <property name="hibernate.connection.driver_class">
            org.postgresql.Driver
        </property>

        <property name="hibernate.connection.url">
            jdbc:postgresql://localhost:5432/hibernate_demo
        </property>

        <property name="hibernate.connection.username">
            postgres
        </property>

        <property name="hibernate.connection.password">
            your_password
        </property>

        <property name="hibernate.dialect">
            org.hibernate.dialect.PostgreSQLDialect
        </property>

        <property name="hibernate.hbm2ddl.auto">
            update
        </property>

        <property name="hibernate.show_sql">
            true
        </property>

        <property name="hibernate.format_sql">
            true
        </property>

        <mapping class="org.example.User"/>

    </session-factory>

</hibernate-configuration>
```

### **Important properties**

| Property                  | Meaning                                        |
| ------------------------- | ---------------------------------------------- |
| `connection.driver_class` | Database driver                                |
| `connection.url`          | Database URL                                   |
| `connection.username`     | Database username                              |
| `connection.password`     | Database password                              |
| `hibernate.dialect`       | Helps Hibernate generate database-specific SQL |
| `hbm2ddl.auto`            | Controls table creation and updates            |
| `show_sql`                | Displays generated SQL                         |
| `format_sql`              | Formats generated SQL                          |

### `hbm2ddl.auto`**&#x20;values**

| Value         | Meaning                                                       |
| ------------- | ------------------------------------------------------------- |
| `create`      | Creates tables; existing tables can be dropped                |
| `update`      | Updates schema without intentionally dropping existing tables |
| `create-drop` | Creates tables and drops them when SessionFactory closes      |
| `validate`    | Checks whether the schema matches the entities                |
| `none`        | Does not automatically manage the schema                      |

**Interview tip:** `update` is convenient during learning. For production, database migrations are generally preferred.

# **5. Entity Class and Annotations**

An **Entity** is a Java class that Hibernate maps to a database table.

Example:

```
package org.example;

import jakarta.persistence.*;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    @Column(name = "user_name")
    private String name;

    @Column(unique = true)
    private String email;

    private int age;

    public User() {
    }

    public User(String name, String email, int age) {
        this.name = name;
        this.email = email;
        this.age = age;
    }

    // Getters and setters
}
```

### **Important annotations**

| Annotation        | Purpose                                    |
| ----------------- | ------------------------------------------ |
| `@Entity`         | Marks the class as an entity               |
| `@Table`          | Specifies the table name                   |
| `@Id`             | Marks the primary key                      |
| `@GeneratedValue` | Automatically generates primary key values |
| `@Column`         | Configures a database column               |
| `@Transient`      | Excludes a field from database mapping     |
| `@Temporal`       | Used for legacy `Date`/`Calendar` mapping  |

### **Primary key generation strategies**

```
@GeneratedValue(strategy = GenerationType.IDENTITY)
```

| Strategy   | Meaning                                                    |
| ---------- | ---------------------------------------------------------- |
| `AUTO`     | Hibernate chooses a strategy                               |
| `IDENTITY` | Database generates the ID, commonly using identity columns |
| `SEQUENCE` | Uses a database sequence                                   |
| `TABLE`    | Uses a separate table to generate IDs                      |

**Important:** Entity classes should have a no-argument constructor. Hibernate needs it to create entity objects.

# **6. HibernateUtil – Creating SessionFactory**

Instead of creating SessionFactory repeatedly, we create a utility class.

```
package org.example;

import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HibernateUtil {

    private static final SessionFactory sessionFactory;

    static {
        try {
            sessionFactory = new Configuration()
                    .configure("hibernate.cfg.xml")
                    .buildSessionFactory();

        } catch (Throwable ex) {
            System.err.println("SessionFactory creation failed: " + ex);
            throw new ExceptionInInitializerError(ex);
        }
    }

    public static SessionFactory getSessionFactory() {
        return sessionFactory;
    }
}
```

### **Understand the important lines**

- `static`: Executes when the class is loaded.
- `final`: The SessionFactory reference cannot be reassigned.
- `configure()`: Reads the XML configuration.
- `buildSessionFactory()`: Creates the SessionFactory.
- `getSessionFactory()`: Returns the already-created object.

**Remember:** One SessionFactory, multiple Sessions.

# **7. CRUD Operations in Hibernate**

CRUD means:

- C – Create
- R – Read
- U – Update
- D – Delete

Let's use the same `User` entity.

## **7.1 CREATE – Insert a record**

```
SessionFactory factory = HibernateUtil.getSessionFactory();

Session session = factory.openSession();

Transaction tx = session.beginTransaction();

User user = new User(
        "John",
        "john@gmail.com",
        22
);

session.persist(user);

tx.commit();

session.close();
```

**What happens?**

1. Open a Session.
2. Start a transaction.
3. Create a Java object.
4. `persist()` makes the object persistent.
5. Commit the transaction.
6. Hibernate executes the necessary INSERT.

## **7.2 READ – Retrieve a record**

```
Session session = factory.openSession();

User user = session.find(User.class, 1);

if (user != null) {
    System.out.println(user.getName());
} else {
    System.out.println("User not found");
}

session.close();
```

`find()` takes two arguments:

```
session.find(EntityClass.class, primaryKey);
```

It returns the entity if found, otherwise `null`.

## **7.3 UPDATE – Update a record**

```
Session session = factory.openSession();

Transaction tx = session.beginTransaction();

User user = session.find(User.class, 1);

if (user != null) {
    user.setName("Alex");
    user.setAge(25);
}

tx.commit();

session.close();
```

**Important concept: Dirty Checking**

Hibernate automatically tracks changes to managed entities.

When you modify an entity and commit the transaction, Hibernate detects the changes and generates an UPDATE query.

You don't necessarily need to call an update method.

## **7.4 DELETE – Delete a record**

```
Session session = factory.openSession();

Transaction tx = session.beginTransaction();

User user = session.find(User.class, 1);

if (user != null) {
    session.remove(user);
}

tx.commit();

session.close();
```

`remove()` marks the managed entity for deletion. The DELETE is executed when Hibernate flushes the changes.

# **8. HQL – Hibernate Query Language**

HQL stands for Hibernate Query Language.

It looks similar to SQL, but it works with **Java entity class names and field names**, not database table and column names.

### **SQL**

```
SELECT * FROM users WHERE age > 20;
```

### **HQL**

```
List<User> users = session.createQuery(
        "FROM User WHERE age > 20",
        User.class
).list();
```

### **More HQL examples**

**Get all users:**

```
List<User> users = session.createQuery(
        "FROM User",
        User.class
).list();
```

**Find by email:**

```
User user = session.createQuery(
        "FROM User WHERE email = :email",
        User.class
)
.setParameter("email", "john@gmail.com")
.uniqueResult();
```

**Count users:**

```
Long count = session.createQuery(
        "SELECT COUNT(u) FROM User u",
        Long.class
).uniqueResult();
```

**Remember:** Use entity names and Java property names in HQL.

# **9. Entity Lifecycle**

This is an important Hibernate interview topic.

[Hibernate Lifecycle | States in Hibernate: Transient, Persistent, Detached, Removed | by Nikhil Sukhani | Medium](https://images.openai.com/static-rsc-4/FnhO1jqnfVU_u9t9o2pE_sMYUvsQCzvvsmW8xg1RguANSQBEmQGx8NKs22uB7p3H1np2PGyZ1-3nmX1ah5ArvvCkcFC5IjClSo8LzGxY34vu4mjAxI91vGbmWRcG0LIgQyy4C_xgS_mdSuGf3ICq8NIiURSZ_5n_Ro1P2d-Xuco?purpose=inline)

[nikhilsukhani.medium.com](https://nikhilsukhani.medium.com/hibernate-lifecycle-states-in-hibernate-transient-persistent-detached-removed-40ba2f689b07?utm_source=chatgpt.com)

An entity has four important states.

| State      | Meaning                                                  |
| ---------- | -------------------------------------------------------- |
| Transient  | New Java object, not associated with a Session           |
| Persistent | Object is managed by a Session                           |
| Detached   | Was managed, but Session is closed or object is detached |
| Removed    | Marked for deletion                                      |

### **Example**

```
User user = new User("John", "john@gmail.com", 22);
```

At this point, the object is **Transient**.

```
session.persist(user);
```

Now it is **Persistent**.

```
session.close();
```

Now it becomes **Detached**.

```
session.remove(user);
```

A managed entity becomes **Removed**.

# **10. Session vs SessionFactory**

| SessionFactory   | Session                      |
| ---------------- | ---------------------------- |
| Heavyweight      | Lightweight                  |
| Created once     | Created when needed          |
| Thread-safe      | Not thread-safe              |
| Creates Sessions | Performs database operations |
| Long-lived       | Short-lived                  |

Interview question: Why should SessionFactory be created only once?

Answer: Because it is heavyweight and stores the configuration and resources required to create Sessions. Recreating it repeatedly is expensive.

# **11. First-Level Cache and Second-Level Cache**

Hibernate supports caching to reduce database calls.

### **First-Level Cache**

- Available by default.
- Associated with a Session.
- Cannot be shared between Sessions.
- Automatically enabled.

Example:

```
User user1 = session.find(User.class, 1);
User user2 = session.find(User.class, 1);
```

If the first result is still in the Session cache, Hibernate can return the same managed object without another SELECT query.

### **Second-Level Cache**

- Associated with SessionFactory.
- Can be shared between Sessions.
- Needs configuration and a cache provider.
- Useful for frequently accessed data.

| First-Level Cache          | Second-Level Cache            |
| -------------------------- | ----------------------------- |
| Session-level              | SessionFactory-level          |
| Enabled by default         | Requires configuration        |
| Not shared across Sessions | Can be shared across Sessions |

# **12. Important Hibernate Methods**

| Method               | Purpose                                            |
| -------------------- | -------------------------------------------------- |
| `openSession()`      | Opens a new Session                                |
| `beginTransaction()` | Starts a transaction                               |
| `persist()`          | Makes a new entity persistent                      |
| `find()`             | Retrieves an entity by ID                          |
| `get()`              | Legacy API; avoid in new Hibernate 6 code          |
| `remove()`           | Marks an entity for deletion                       |
| `merge()`            | Copies detached entity state into a managed entity |
| `flush()`            | Synchronizes pending changes with the database     |
| `clear()`            | Detaches all entities from the Session             |
| `close()`            | Closes the Session                                 |
| `commit()`           | Commits a transaction                              |
| `rollback()`         | Rolls back a transaction                           |

# **13. Best Practice – Handle Transactions Properly**

A safer pattern is to use try-with-resources for the Session and roll back if an error occurs.

```
try (Session session = HibernateUtil.getSessionFactory().openSession()) {

    Transaction tx = session.beginTransaction();

    try {
        User user = new User("John", "john@gmail.com", 22);

        session.persist(user);

        tx.commit();

    } catch (Exception e) {
        if (tx.isActive()) {
            tx.rollback();
        }

        e.printStackTrace();
    }

} catch (Exception e) {
    e.printStackTrace();
}
```

This helps ensure the Session closes even if an exception occurs.

# **14. Common Hibernate Interview Questions**

**1. What is Hibernate?**

Hibernate is a Java ORM framework that maps Java objects to database tables and simplifies database operations.

**2. What is ORM?**

ORM stands for Object Relational Mapping. It connects object-oriented programming concepts with relational database tables.

**3. What is the difference between JDBC and Hibernate?**

JDBC requires more manual SQL and result mapping. Hibernate provides automatic object mapping and generates SQL.

**4. What is SessionFactory?**

It is a heavyweight, thread-safe object used to create Session objects.

**5. What is Session?**

It is an object used to perform database operations and manage entity objects.

**6. What is a transaction?**

A transaction is a group of database operations that should succeed together or be rolled back.

**7. What is dirty checking?**

Hibernate automatically detects changes made to managed entities and generates UPDATE statements when needed.

**8. What is HQL?**

HQL is an object-oriented query language that works with entity names and their properties.

**9. What is the difference between&#x20;**`persist()`**&#x20;and&#x20;**`merge()`**?**

- `persist()` makes a new entity persistent.
- `merge()` copies the state of an entity into a managed instance and returns that managed instance.

**10. What is the difference between&#x20;**`find()`**&#x20;and HQL?**

- `find()` retrieves an entity using its primary key.
- HQL allows queries using conditions, sorting, joins, and other query operations.

**11. What is lazy loading?**

Lazy loading means related data is fetched only when it is accessed, rather than immediately.

**12. What is the N+1 query problem?**

It happens when Hibernate executes one query to fetch a list of records and then additional queries for each record's related data.

# **15. Quick Revision – Things to Remember**

**Hibernate flow**

Java Entity

Session

Hibernate ORM

JDBC Driver

**PostgreSQL Database**

**Must remember**

- `@Entity` → Java class becomes an entity.
- `@Id` → Primary key.
- `SessionFactory` → Creates Sessions.
- `Session` → Performs database operations.
- `Transaction` → Commit or rollback.
- `persist()` → Insert.
- `find()` → Read.
- Dirty checking → Update.
- `remove()` → Delete.
- HQL → Queries using entity names.

# **16. Test Your Hibernate Knowledge**

Try answering these without looking at the notes.

**Hibernate mini quiz**

0/6 answered

**1. Which Hibernate object creates Session objects?**

Session

SessionFactory

Transaction

Configuration

**2. Which annotation identifies the primary key?**

@Entity

@Column

@Id

@Table

**3. What does dirty checking do?**

Deletes records

Automatically detects changes to managed entities

Creates database connections

Validates XML

**4. Which language uses entity names instead of table names?**

SQL

HQL

HTML

JDBC

**5. Which method marks an entity for deletion?**

persist()

find()

remove()

merge()

**6. Which cache is enabled by default?**

Second-level cache

First-level cache

Query cache

None

Check answers

**What to learn next:** Once you're comfortable with these basics, move on to Hibernate relationships (`@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany`), lazy vs eager loading, cascading, and pagination. These are important for real-world projects and interviews.
