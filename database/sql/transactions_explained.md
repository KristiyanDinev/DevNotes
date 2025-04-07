# Transactions

A transaction is just a way to modify the database in a testing environment
where you don't interact directly with the real database, but with a copy.

And that copy is temporary and working until you save all of your changes.
Can I still interact directly with the database even if there is a transaction going on?

Yes you can. A transaction can be represented either as a sql query or in a JDBC connection, which also runs queries.
Nowadays the transactions and prepared statements are getting more and more common and used in databases.
And in most of the applications which are of companies, they use frameworks which by default handle
the transactions and prepared statements. Actually some frameworks don't have this setup by default, so
you may have to look up their configuration and documentation to enable this implementation.

So you can either save the transaction or not.
What you can do is: commits, rollbacks, save-points, cancel and etc.

- **Commit** is another term of "saving". You save your copy of the database (the transaction)
 to the real database. Then the transaction will be emptied.

- **Rollback** is the action of undoing the sql queries in the transaction (not yet saved).
- **Save Points** create a checkpoint in your transaction as your current state of the transaction.
Let me give you an example:

You make the following change in the database using transaction.    
*UPDATE users SET username = 'Bob' WHERE id = 1;*

Then you save that point in your transaction.
After that you execute another SQL query.
*INSERT INTO users (username) VALUES ('John');*

But then you get an error in your code and you decide to rollback your query.
So you rollback and what is the state of the transaction?
Now the transaction only has recorded the *UPDATE* query without
the *INSERT* query.

- **Cancel**: this cancels the whole transaction and any changes to the database in that transaction
are not applied to the actual database.

As you can see that a transaction can handle a state of a database. 
From a temporary state to applying the changes made to the database.

Transactions are used when you have more then one query to execute, but I am not sure if that is the case
with some frameworks. Another explanation can be that a single transaction is like a shadow of the original database,
 but when you commit your changes, then it becomes part of the original database.
