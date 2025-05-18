# DTO (Data Transfer Object)

This is mainly used when you receive a urlencoded or JSON data from the client into your controller 

and you want to turn this data into a Java object *class*. For example: LoginUserDTO or RegisterUserDTO.

In both request you would receive the *password* and the *email (if you login with email and not with username)*

and in only registering you will receive the *username*. Like we talk about the user in both classes, but in

different requests.

# DAO (Data Access Object)

This is where you would do the server-side operations from the data you have.

For example you may have a class called UserDAO which has methods for updating 

a profile image *(pfp)* or changing the username of that user or doing a subscription 

plan canceling or renewing.

**Also** You may see it to implement database queries or database related operations.

# Entity (from the JPA library)

This is one way of saying "This java class will turn into a row from a table.".

Example:
```java
@Entity
public class UserEntity {
    ...
}
```

If you add the `@Table(name="users")` or whatever name you want to give it. It will turn into a table.

`spring.jpa.hibernate.ddl-auto=value` in `application.properties`

```yaml
spring:
  jpa:
    hibernate:
      ddl-auto: value
```
in `application.yaml`

#### Values
- **create** - It only creates the tables from the classes and does not update them if you change the class properties.

- **create-drop** - It drops the tables and recreates them every time you run the application.

- **validate** - It checks if the database has the correct tables according to the entity classes. It is like a integration test or mock/junit test.

- **update** - It doesn't drop the table, but it alters it when it detects changes in the Java class *(with @Entity and @Table annotations)*.

tells Spring Boot JPA's library to set the execution of `ddl` *(Data Definition Language)* script

which will update the database based on your value. it is `ddl-auto`, because it executes the `ddl`

automatically on startup. This `ddl` is constructed by your `@Entity` and/or `@Table` classes.

Not sure if you need them both or only one so it can work, but in any case use them both. It is

a good practice to specify it.

# Repository (from the JPA library)

A repository is a JPA's way of communicating with the database by providing a simple way to use

CRUD *(Create Read Update Delete)* operations for a table. For example to find all the rows

matching a criteria or to updata a row with specific values or to delete a row that is expired or has

the matching `id`. In Spring Boot there are some interfaces from which you can choose to use. You

don't need an class to implement all of them, but rather just an interface and Spring Boot will 

create a class `SimpleJpaRepository` which handles the interfaces and the implementations thereof.

Example:
```java
@Repository
@Transactional
public interface IUserRepository extends JpaRepositoryImplementation<UserEntity, Integer> {
    // nothing more here, unless you want to add extra stuff.
}
```

This will use the JPA's built-in repository and `SimpleJpaRepository` which will do the job for you.

`JpaRepositoryImplementation<T, ID>` is the interface. `T` represents the entity class which has the 

`@Entity` on it and `ID` represents the wrapper data type class for the `id` for that row. Like here

the row will have an `id` which is of type `Integer` not null. You may use `String`, `UUID`,

`Long` or any other data type for the `id`, but it needs to match with the entity Java class you have given.