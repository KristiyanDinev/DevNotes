# Prepared and Parametrized statements

The database management system (DBMS) can use prepared statements and
parameterized statements.

## Prepared
When an SQL query is executed it goes through the DBMS. That system breaks the
query down to smaller parts and understands what it is trying to do.
Then after the system understands what the query wants. It delivers it to the
user as a result. You may get an error if the query can not be broken down to smaller
pieces.

### Performance
A prepared statement is like a query template, which is ready to be used again without
the need of the database to understand it all over again, but it has remembered it and stores
it in the server memory or in the place it is told to by the configuration or the software itself. 

That is one of the ways it works. There are many ways that a prepared statement is used, but 
this is what I know it is used so far. The prepared statement can work in a single connection and
not be saved for any future uses.
This really depends on the framework or library you use and what you have it configured.

Do you have to memories every configuration and library? No, but it is good to know
how they work and how to configure them. I am not talking about to know every single
detail, but rather the general idea of how they work and how can you use them when you need them.

The prepared statement needs somewhere to be remembered, so it takes some of memory.
A single prepared statement is like a single query.
As you know a prepared statement is not always remembered by the database or connection forever.

This can be useful to remember complex queries and reuse them, rather then recreating them (if remembered).

### Use cases
This is done in case you will execute the same query structure 
(without the custom values like usernames, IDs, passwords, dates etc.) all over again and again.

But in case you will not going to repeat your query structures 
(without the custom values like usernames, IDs, passwords, dates etc.) and almost every time
you would use differed queries, then you may not have need of prepared statements, because they
might not profit you.

### Security
The values that we insert into the prepared query/statement are escaped in a way that it can
prevent sql injections.


### Examples from the internet including AI
```java
String sql = "INSERT INTO users (name, email) VALUES (?, ?)";
PreparedStatement pstmt = connection.prepareStatement(sql);
pstmt.setString(1, "John");
pstmt.setString(2, "john@example.com");
pstmt.executeUpdate();
```

<a href="https://en.wikipedia.org/wiki/Prepared_statement">Wiki</a>

>In database management systems **(DBMS)**, a prepared statement, parameterized statement, 
>(not to be confused with parameterized query) is a feature where the database pre-compiles 
>SQL code and stores the results, separating it from data.


## Parametrized

It is like a prepared statement and it is more popular to use prepared statement.
Yeah. The whole idea is the usage of parameters which is also in the prepared statements
and it can also be turned into a prepared statement.

I mean the google search results for parametrized statement give the definition of the prepared statements.
