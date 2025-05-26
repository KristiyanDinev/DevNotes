# Cashing

**Cashing** is just a way to **store data** and later on to **get it**.

This is done to **prevent** the application or server from **continuously** requesting new data from the database. This request can be very expensive for the company. 

"**Expensive**" means that the company is giving up a lot of **resources** just to provide the service to the user.
Like **high latency**, **high load** and **high performance usage**.

In order to **prevent** these things in **large companies**, they use **caching**.

# Store Cache

## In Memory (RAM)

A server can store its **cache** in **RAM** and use it in **runtime**, but once the **server** is **restarted**, that **cache** is gone.

>*Notes: See [Cluster](/DevNotes/system_design/cluster)*

You can use a **cluster** *(multitudes of servers)* to keep the **cache** on **multiple servers** in case one of them **shuts down**. But in order to keep **all servers updated** with their **cache**, this means that every time the **cache is updated or cleared**, then all the **servers** must **sync**. This is the only **drawback** and it may get a **bit expensive**, but not too much, so it will not profit the company.

## Database for caching

The most *commonly* used database for cashing is **Redis**.

**In-memory** database is used as **caching database** and **message broker**. It uses **key-value** pairs *(like JSON)* data and not **SQL** queries.

### Why Redis and not any other database?

Because **Redis** is made in a way that fills the criteria for **caching**. It trades a lot of features for **performance** and **speed**. When the **database** is in **memory** it **reads** and **writes** *faster*.

## Save db/cache to files

**Redis** saves the **in-memory** database in files for some period of time. You can also save the database to files by executing a command inside the **Redis** shell.

**Automatic** save to file:

- **BGSAVE** - *https://redis.io/docs/latest/commands/bgsave/*
- **SAVE** - *https://redis.io/docs/latest/commands/save/*

>*Note: this part may use a lot of RAM.*

*See: https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/*

# Ways to implement caching

The **whole idea** of **caching** is to store *often requested data* and just to **provide** a quick **response** to that *same* request.

- When a user is requesting to visit a user's profile, if that user is **not found** in the **cache**, then the **cache server** will request it from the **database** and then when it gets it, then the **cache** will save it with an **expiration time** and then return the result to that user.


