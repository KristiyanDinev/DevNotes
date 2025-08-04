# Workflow Orchestration Engine

*Analogy: This is the bigger brother of REST APIs*

>*Article: https://dev.to/rock_win_c053fa5fb2399067/spring-boot-microservice-orchestration-with-temporal-hnp*
>
>*Website: https://temporal.io/*

## What is it?

A way of simplifying long and complex tasks

## How does it do that?

By using *workflows* and *actions*

- *workflow* - It is a series of steps to take to complete a single task. *Think of it as a function in programming (it is a whole class in the SDK, but for analogy's sake).*

- *action* - It is that single step to prefrom in that *workflow*. It is very similar to a simple RESTful API's endpoint.


## REST API vs Workflow Orchestration Engine

**REST API**

- Simple for small tasks

- Example: Login endpoint `/login`
    - Reads the database
    - Writes to the database
    - Maybe set a cookie



**Workflow Orchestration Engine**

- Good to maintain and keep in order larger and more complex tasks

- Example: Order endpoint `/order?products=...&region=....&country=...` *(`/order` can also be done in two or more steps if it is not an enterprise or a company, but a simple side project or  a hobby)*
    - Read the database
    - Write to the database
    - Use a thrid-pary service
    - Subscribe to a real-time data provider (event based or a websocket connection)
    - Send notification to the user that his order is accepted
    - Complete the payment
    - Register this order ready for shipment
    - etc.

