# Cashing

**Cashing** is just a way to **store data** and later on to **get it**.

This is done to **prevent** the application or server to **continuously request new data** from the database.
This request can be very expensive for the company. 

"**Expensive**" means that the company is giving up a lot of **resources** just to provide the service to the user.
Like **high latency**, **high load** and **high performance usage**.

In order to **prevent** these things in **large companies**, they use **caching**.

# In Memory (RAM)

A server can store their **cache** in **RAM** and use it in **runtime**, but once the **server** is **restarted** that **cache** is gone.

>*Notes: See [Cluster](/system_design/cluster.md)*

You can use a **cluster** *(multitudes of servers)* to keep the **cache** on **multiple servers** in case one of them **shuts down**. 
But in order to keep **all servers updated**  with their **cache** this means that every time the **cache is updated or cleared**, then all the **servers** must **sync**. This is the only **drawback** and it may get a **bit expensive**, but not too much, so it will not profit the company.

# Ways to implement caching

The **whole idea** of **caching** is to store *often requested data* and just to **provide** a quick **response** to that *same* request.

- When user is requesting to visit a user's profile, if that user is **not found** in the **cache**, then the **cache server** will request it from the **database** and then when it gets it, then the **cache** will save it with an **expiration time** and then return the result to that user.


