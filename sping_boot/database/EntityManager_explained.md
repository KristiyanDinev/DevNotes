# EntityManager

*If you have a provided Repository as a class or just an interface with @Repository annotation,*
*then EntityManager will also become a @Bean if you don't even create it.*
*You can also create the repository as @Bean in a @Configuration class.*

This represents the database, but in JPA format. And JPA uses JDBC.

At this point it is like abstraction upon abstraction, but not *TOO MUCH* abstraction,
but only enough.

# EntityManagerFactory

# CriteriaBuilder