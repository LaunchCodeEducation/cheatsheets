# Spring Boot Persistence Cheat Sheet

This cheat sheet is broadly applicable, but assumes that you're using MySQL 5.x, Spring Boot, and Maven.

## POM Dependencies

Add the following to your `pom.xml` (listed here as GroupID / ArtifactID):
    - `org.springframework.boot` / `spring-boot-starter-jdbc`
    - `org.springframework.boot` / `spring-boot-starter-data-jpa` (includes Hibernate as a dependency)
    - `mysql` / `mysql-connector-java`

## `application.properties`

These properties use `property=value` syntax. For many, there are alternative values listed in the Spring documentation, but these are good for most purposes. The database user specified usually needs all privileges on the given database.

`spring.datasource.url` - of the form `jdbc:mysql://localhost:3306/db` for a local database `db` on the default MySQL port 3306

`spring.datasource.username` - Username to use when connecting to the db

`spring.datasource.password` - Password to use when connecting to the db

`spring.jpa.database = MYSQL` - specifies that you'll be using a MySQL database

`spring.jpa.hibernate.ddl-auto = update` - Spring should attempt to update the database schema upon restart to reflect any changes to persistent model classes

`spring.jpa.hibernate.naming-strategy = org.hibernate.cfg.ImprovedNamingStrategy` - Spring will use underscored conventions to create table and column names. So, for instance, a field named `firstName` will correspond to a column named `first_name`

`spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5Dialect` - Specifies that we're using MySQL 5.x

## Java Persistence API (JPA)

### Class Annotations

`@Entity` - Required for Hibernate to store/retrieve instances of the class to/from the database.

`@Table` - *Optional*. Specifies the name of the table in the given database used to store the objects properties. If ommitted, the table name is generated via the configured naming convention.

### Setting up your class

Each persistent class -- that is, each class that you want to be managed by Hiberante and stored in the database -- needs to follow a couple of important rules. These rules will allow Hibernate to properly create objects when pulling data from the database. If you don't follow these rules, you'll see Hibernate-related exceptions upon starting your application.

### No-arg constructor

**Every Hibernate-managed class needs a public constructor that takes no arguments** (aka a "no-arg constructor"). For example, if we're dealing with a class named `User` then it should have a constructor that looks like:
```java
public User() {}
```
This allows Hibernate to create a new instance of your object, no matter what properties it may have. Instance properties won't be initialized by this constructor, obviously, which is why we need the next rule.

### Public getters and setters

**Every Hibernate-managed class needs public getters and setters for each of its persistent properties.** This will allow to set the properties of your persistent object after creating it with the no-arg constructor.

*Note:* There are settings in which this rulee isn't strictly necessary -- e.g. if you annotate fields instead of getters, as discussed below -- but for the purpose of the method we're using to set up Hibernate in this guide, they must be followed.

### Property Annotations

These are annotations are applicable within a class annotated by `@Entity`. They determine how object properties are persisted in a database.

These annotations can, in most cases, be applied to object fields directly, or to their getters. We recommend applying them to getters, which means that Hibernate will access the properties via the getters, rather than accessing them directly. [Read about][field-vs-property] why you might choose annotating the getter methods of a class rather than the fields.

`@NotNull` - This is not actually a JPA annotation -- it is part of the `javax.validation.constraints` package -- but Hibernate enforces it as well. Specifies that the property may not be null.

`@Column` - Specifies the name of the column in the given table used to store the property. If ommitted, the column name is generated via the configured naming convention. There are several attributes -- such as `nullable` and `unique` -- that result in database-level restrictions being applied (i.e. in database constraints rather than Hibernate constraints).

*Note*: `@NotNull` and `nullable = false` have similar but slightly different effects. [Read more](http://stackoverflow.com/questions/7439504/confusion-notnull-vs-columnnullable-false) about how they differ.

`@Id` - The property should be considered an identifier for the class. In particular, the corresponding column in the database should be a primary key. A field with this annotation must one of: a primitive type, a primitive wrapper type (e.g. `Integer` or `Float`), `String`, `java.util.Date`, or a few others (see documentation link below).

`@GeneratedValue` - Specifies that the field should be generated by Hibernate. This annotation can be configured to use different value generation strategies.

`@OneToMany` - Specifies a property that is a collection of other persistent objects.

`@ManyToOne` - The inverse (or "other side") of the `@OneToMany` annotation. This is used within a class that is "owned" by another class, to annotate a field that points back to the "owner". For example, with `User` and `Post` classes (where `Post` represents a blog post, of which a `User` can have many), the `Post` class can have an `author` field that points to the object that owns it.s

`@JoinColumn` - Typically used in conjunction with `@OneToMany`, this annotation specifies the column on the "owned" object's table that should be used to identify it's owner. In the blog example, the value of the `author_uid` column specifies the `uid` of the `User` that owns the `Post`.

`@Transient` - Specifies that Hibernate *should not* store the property in the database.

## Additional References

- [Spring Data JPA Reference](http://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
- [Object Relational Mapping (ORM) in Spring](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/orm.html)
- [JPA Annotation Reference](http://www.objectdb.com/api/java/jpa/annotations)

[field-vs-property]: http://stackoverflow.com/questions/594597/hibernate-annotations-which-is-better-field-or-property-access
