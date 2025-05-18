# Java Persistence API (JPA)

A library to help with making databases.

## EntityManager

If you have a provided Repository as a class or just an interface with @Repository annotation, then EntityManager will also become a @Bean.

You can also create the repository as @Bean in a @Configuration class.

EntityManager aims to minsize the need of CRUD operations which we have to code every time we start a new project for every model/entity.

## EntityManagerFactory

Think of it as the class which creates the **EntityManager** object when it is needed.
It has it's own benefits to have a factory. Like you can better manage the usage of the object for
that class, but factories are not always needed. You got to see in which situations are helpful
and are not just eating performance.

For example factories are normally used for large scale projects that require management of often used classes.

## CriteriaBuilder

In general it is the built-in sql builder for JPA.

Get the **CriteriaBuilder** from **EntityManager** or **EntityManagerFactory**
```java
CriteriaBuilder criteriaBuilder = entityManager.getCriteriaBuilder();
```

Here we tell the **CriteriaBuilder** that we will work on the **UserEntity** JPA class, 
which it has its own table in the database.

**UserEntity.class**
```java
@Entity
@Table(name = "users")
public class UserEntity {
    ...
}
```

This is used for `select` sql queries
```java
CriteriaQuery<UserEntity> criteriaQuery = criteriaBuilder.createQuery(UserEntity.class);
```

If you want **Update** or **Delete** queries. Use these:

**Update**
```java
CriteriaUpdate<UserEntity> criteriaQuery = criteriaBuilder.createCriteriaUpdate(UserEntity.class);
```

**Delete**
```java
CriteriaDelete<UserEntity> criteriaQuery = criteriaBuilder.createCriteriaDelete(UserEntity.class);
```

This will do `select * form users;` sql.
```java
Root<UserEntity> root = criteriaQuery.from(UserEntity.class);
```

The `Root<UserEntity>` will act as the access to the `@Entity` class, when building the sql query.
You will see the usage later on.

**Where**

*Assuming that you have `UserEntity` object by which you will provide the information required.*

Here the sql would look like `select * from users where users.name = 'example_name';`

```java
criteriaQuery = criteriaQuery.where(
    criteriaBuilder.equal(root.get("name"), theUserObject.name)
    );
```

**Query**

This would build the sql query, ready for parameters and/or execution.

```java
TypedQuery<UserEntity> typedQuery = entityManager.createQuery(criteriaQuery);
```

Set parameters. Example:

SQL: `select * from users where name = :name`
```java
typedQuery = typedQuery.setParameter(":name", theUserObject.name);
```

**Execution**

Gets all the rows from the result of that sql query as list of objects.
Each object is one row.

```java
List<UserEntity> userEntities = typedQuery.getResultList();
```

Or you can just get a single row if you expect a single result.

```java
UserEntity user = typedQuery.getSingleResult()'
```

Or if you want to work with streams.

```java
Stream<UserEntity> resultStream = typedQuery.getResultStream();
```

