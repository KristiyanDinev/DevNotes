# Transactions

A **transaction** is just a way to **modify** the database in a **testing** environment
where you **don't interact** directly with the **real database**, but with a **copy**.

And that **copy** is *temporary* and working until you **save** all of your **changes**.

You can still work with the **real database** while a **transaction** is *activated*.
A **transaction** can be represented either as a **sql** query or in a **JDBC** connection, which also runs queries.

Nowadays, the **transactions** and **prepared statements** are becoming more and more common and used in **databases**.
And in most of the **applications** which are of companies, they use **frameworks** which by default handle 
the **transactions** and **prepared statements**. 

Actually, some **frameworks** don't have this setup by default, so
you may have to look up their *configuration* and *documentation* to **enable** this implementation.

So you can either **save** the **transaction** or *not*.
What you can do is: **commits**, **rollbacks**, **save-points** and **cancel**.

- **Commit** is another term of *"**saving**"*. You **save** your **copy** of the database *(the transaction)*
 to the **real database**. Then the **transaction** will be emptied.

- **Rollback** is the action of **undoing** the **sql** queries in the **transaction** *(not yet saved)*.

- **Save Points** create a **checkpoint** in your **transaction** as your *current state* of the **transaction**.


Example:

You make the following change in the **database** using **transaction**.    
```sql
UPDATE users SET username = 'Bob' WHERE id = 1;
```

Then you **save** that **point** in your **transaction**.
After that, you execute another **SQL** query.
```sql
INSERT INTO users (username) VALUES ('John');
```

But then you get an error in your code, and you decide to **rollback** your **query**.
So you **rollback** and what is the state of the **transaction**?
Now the **transaction** has only recorded the ***UPDATE*** query without
the ***INSERT*** query.

- **Cancel**: this **cancels** the whole **transaction** and any *changes* to the database in that ***transaction***
are **not applied** to the **actual database**.

As you can see that a **transaction** can handle a *state* of a **database**. 
From a **temporary** state to *applying* the *changes* made to the **database**.

Another explanation could be that a **single transaction** is like a **shadow** of the original **database**, 
but when you **commit** your **changes**, then it becomes part of the **original database**.
