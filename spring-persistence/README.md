# Spring Boot Persistence Cheat Sheet

This cheat sheet is broadly applicable, but assumes that you're using MySQL 5.x, Spring Boot, and Gradle.

## Using MySQL

Using a local MySQL database will allow your data to persist between application shutdown and startup. An easy way to install MySQL on Mac and Windows is via [MAMP](https://www.mamp.info/en/).

### Dependencies

Add the following to the `dependencies` section of your `build.gradle`:
```nohighlight
compile('org.springframework.boot:spring-boot-starter-data-jpa')
compile('mysql:mysql-connector-java')
```

The `spring-boot-starter-data-jpa` artifact includes the Hibernate ORM framework. We won't talk explicitly about Hibernate since we won't need to work with it directly in most situations. We'll primarily work with Java Persistence API annotations, but you should know that Hibernate provides the implementation for most of these behaviors behind the scenes.

### `application.properties`

The database connection needs to be configured via `src/main/resources/application.properties`.

These properties use `property=value` syntax. For many, there are alternative values listed in the Spring documentation, but these are good for most purposes. The database user specified should have all privileges on the given database.

`spring.datasource.url` - of the form `jdbc:mysql://localhost:8889/db` for a local database `db` on the default MySQL port 8889

`spring.datasource.username` - Username to use when connecting to the db

`spring.datasource.password` - Password to use when connecting to the db

`spring.jpa.database = MYSQL` - specifies that you'll be using a MySQL database

`spring.jpa.hibernate.ddl-auto = update` - Spring should attempt to update the database schema upon restart to reflect any changes to persistent model classes

`spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5Dialect` - Specifies that we're using MySQL 5.x

The database engine must be running when you start up your application.

## Java Persistence API (JPA)

### Class Annotations

`@Entity` - Required for Hibernate to store/retrieve instances of the class to/from the database. Every field of the class not marked `@Transient` will be stored in the database (see below for more on `@Transient`).

`@Transactional` - Most often used on `CrudRepository` interfaces. Specifies that if any data operation within the class throws a runtime exception, all associated database queries should be rolled back (that is, undone). This can prevent data corruption caused by exceptions.

`@Table` - *Optional*. Specifies the name of the table in the given database used to store the objects properties. If omitted, the table name is generated via the default rules.

### Setting up your class

Each persistent class -- that is, each class that you want to be managed by Hibernate and stored in the database -- needs to follow a couple of important rules. These rules will allow Hibernate to properly create objects when pulling data from the database. If you don't follow these rules, you'll see Hibernate-related exceptions upon starting your application.

### No-arg constructor

**Every Hibernate-managed class needs a public constructor that takes no arguments** (aka a "no-arg constructor"). For example, if we're dealing with a class named `User` then it should have a constructor that looks like:
```java
public User() {}
```
This allows Hibernate to create a new instance of your object, no matter what properties it may have. Instance properties won't be initialized by this constructor, obviously, which is why we need the next rule.

### Field Annotations

These annotations are applicable within a class annotated by `@Entity`. They determine how class fields are persisted in a database.

These annotations can, in most cases, be applied to fields directly, but they can also be applied to getter methods. We recommend applying them to fields. Doing so means you don't have to expose a public setter for the given property, which can break encapsulation (think of an `id` field with a public setter...not good). [Read about][field-vs-property] how these two approaches are different, and why you might choose one over the other.

`@NotNull` - This is not actually a JPA annotation -- it is part of the `javax.validation.constraints` package -- but Hibernate enforces it as well. Specifies that the field may not be null.

`@Column` - Specifies the name of the column in the given table used to store the field. If omitted, the column name is generated via the configured or default naming convention. There are several attributes -- such as `nullable` and `unique` -- that result in database-level restrictions being applied (i.e. in database constraints rather than Hibernate constraints).

*Note*: `@NotNull` and `nullable = false` have similar but slightly different effects. [Read more](http://stackoverflow.com/questions/7439504/confusion-notnull-vs-columnnullable-false) about how they differ.

`@Id` - The field should be considered an identifier for the class. In particular, the corresponding column in the database should be a primary key. A field with this annotation must one of: a primitive type, a primitive wrapper type (e.g. `Integer` or `Float`), `String`, `java.util.Date`, or a few others (see documentation link below).

`@GeneratedValue` - Specifies that the field should be generated by Hibernate. This annotation can be configured to use different value generation strategies.

`@OneToMany` - Specifies a field that is a collection of other persistent objects.

`@ManyToOne` - The inverse (or "other side") of the `@OneToMany` annotation. This is used within a class that is "owned" by another class, to annotate a field that points back to the "owner". For example, with `Item` and `Category` classes (where `Item` is contained in a `Category`, that is, a `Category` has many `Item` objects), the `Item` class can have a `category` field that points to the object that owns it.

`@JoinColumn` - Typically used in conjunction with `@OneToMany`, this annotation specifies the column on the "owned" object's table that should be used to identify it's owner. In the `Item`/`Category` example, the value of the `category_id` column specifies the `id` of the `Category` that owns the `Item`.

`@Transient` - Specifies that Hibernate *should not* store the field in the database.

## Additional References

- [Spring Data JPA Reference](http://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
- [Object Relational Mapping (ORM) in Spring](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/orm.html)
- [JPA Annotation Reference](http://www.objectdb.com/api/java/jpa/annotations)

[field-vs-property]: http://stackoverflow.com/questions/594597/hibernate-annotations-which-is-better-field-or-property-access
