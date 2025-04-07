# SQL Builder Problem
Let's say that we have a class that solves a specific problem.
Let's just say it is a database class that handles the execution of the queries.
Then we have a service class which handles the usage of the database class to do
What it is designed to do.

Now we come to the conclusion that either the service class will build the actual query string.
To be executed by the database or the database will require the paragraphs,
So it can execute the built query, which will be built into the database class.
Now, good query builders will make it possible that you can build a query string with all kinds of settings.
And then use that string where you need it.
Keep in mind that it is good practice not to write embedded (into the source code directly) strings that
Represent sql queries.

Because we need to write an adaptable application, and so, if the company changes their sql server, then you
Will have to rewrite maybe almost all the queries and make a whole new release, while if the queries were
Automatically built or in a config file, then you wouldn't have to worry about rewriting the queries again.
When a company has a product that needs to change in the database for some random reason, then you will have to
Be ready to act upon it fast. If the company loses reputation or money or something else, just because of that
Slow database migration. Then guess what? You may have to look for another job, but there is still hope.

OK. What is such a tool that can automatically build the correct query for the database that we use?
Good question. I may know some query builders, but some of them require a lot of code just to create a table or
Execute a register or a login query. Shouldn't the libraries and frameworks already have provided us with such?
Query builder? Yes, but some of those are limited to customizations, and so if the company has a big complex
A database that has custom functionality, then you will find yourselves lacking SQL query builder features.

# Solution
I think as of now, if you don't have a good sql query builder in your application that would support almost every
The features of the SQL provider (the database) and your application are not a hobby or just something small to work with.
(without big plans for the future), then I think a custom SQL builder would be nice. This is why I think a custom
One will be better. It will meet your requirements, and may help someone else who is having the same problem.

It can also be used as a library which you may put as a dependency on your projects, but since it is a custom sql
Query builder. Not many developers will be familiar with it, but as long as you keep the names simple and self-explained,
And put good, well-formatted documentation. I think it will be a piece of cake for the new developers. Like you can put
There is already known terminology from SQL, so when they write it, it is self-explanatory.

Because of this, I was thinking about creating a SQL query builder for myself and using it in my projects. But this is kinda
Selfish, so I may have to write it and make it in multiple programming languages, so that other platforms may take the
Same advantage. But as of now, I don't see any other way to make it easier for the developers to implement new features.

And to also cover the lack of features already in the sql query builder of the framework or library. But this means that
There have to be maintainers of the custom SQL query builder. But as long as you support the basic syntax of the sql of that
Database, then you don't really have to change anything else. And please don't use custom regex for the sql query builders,
Because there may be exceptions and with sql queries we need what we expect to get from the sql query and not a
Deleted databases or tables by SQL injection.

Now there are ways to implement custom features for the desired SQL query, but they may overcomplicated the project and may require more.
Coding. Just that there may be a better and easier way, but with some trade-offs.
