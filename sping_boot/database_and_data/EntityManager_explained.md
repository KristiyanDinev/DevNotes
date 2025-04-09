*All of them are part of the JPA library and are made for databases.*

# EntityManager

*If you have a provided Repository as a class or just an interface with @Repository annotation,*
*then EntityManager will also become a @Bean if you don't even create it.*
*You can also create the repository as @Bean in a @Configuration class.*

At this point it is like abstraction upon abstraction, but not *TOO MUCH* abstraction,
but only enough to handle everything without you to implement it every time yourself.

# EntityManagerFactory

# CriteriaBuilder