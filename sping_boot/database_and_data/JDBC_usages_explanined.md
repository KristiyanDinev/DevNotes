# JDBC (Java DataBase Connectivity)

what is it? It is a way to connect to the database and execute some queries. This is for relational databases that uses SQL

like syntax. As you can see it is for the **Java** programming language. There we will look at the interfaces and the classes

that are in the `org.springframework.jdbc.core` package if you would like to search them up.

So this would require as any database connector. A database driver which is used to connect and do the

database operations therein.

### JdbcOperations

It is an interface of the operations for the relational database in for the JDBC to use.

### JdbcTemplate

This is a class that implements the **JdbcOperations** to help with the database implementation for JDBC.

### JdbcClient

An interface that provides parametrized statements. The **JdbcTemplate** actually uses prepared statements.

### DefaultJdbcClient

A class that implements **JdbcClient**.
(In Spring Boot) that class uses `JdbcOperations` and `NamedParameterJdbcOperations` 
(which I think is similar to `JdbcOperations`), but it does not implement them by using `implements` keyword.

I am not sure if this class is unique only to the Spring Framework or it is in JDBC dependency as well to be used
independently with Spring Framework. But I think (judging by the package name) it is for the Spring Framework itself.
