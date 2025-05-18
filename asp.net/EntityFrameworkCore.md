# Entity Framework Core (EF Core)

Manages the database with **entities**, which are *C# classes*, which represent a *tables* in the database. Supports all **CRUD** operations.

**Entity Framework Core** takes care of the correct *sql syntax and query* and you just take care of *implementing* it.
Don't worry. You still can execute plain **SQL queries**.

If you come from *Java*, then the best way to explain it is: **JPA**.

- Start tutorial: *https://learn.microsoft.com/en-us/ef/core/*

## EF Core VS EF6

Basically **EF Core** is the newer version and **EF6** is the older.
**EF Core** is recommended.

*See https://learn.microsoft.com/en-us/ef/efcore-and-ef6/*

- If you want to change from **EF6** to **EF Core** *https://learn.microsoft.com/en-us/ef/efcore-and-ef6/porting/*.

## Example Models

`null!` tells the compiler that you will provide for SURE the object. **Entity Framework Core** automatically provides the object for you.

*See https://learn.microsoft.com/en-us/ef/core/modeling/relationships/one-to-many*

- **Navigation property** is a property, which helps **Entity Framework Core** to build the correct relationship between the tables. It is not a column, but a relationship.

**User Model**

Example:

```csharp
namespace Project.Models {
    public class User {
        public int Id { get; set; }

        // ... the rest of the properties.

        public int LaptopId { get; set; }
        public Laptop Laptop { get; set; } = null!;

        public int CompanyId { get; set; }
        public Company Company { get; set; } = null!;

        public ICollection<UserRole> UserRoles { get; set; } = new List<UserRole>();
    }
}
```

**Company Model**

Example:

```csharp
namespace Project.Models {
    public class Company {
        public int Id { get; set; }

        // ... the rest of the properties.

        // navigation property
        public ICollection<User> Users { get; set; } = new List<User>();
    }
}
```

**Laptop Model**

Example:

```csharp
namespace Project.Models {
    public class Laptop {
        public int Id { get; set; }

        // ... the rest of the properties.

        public int UserId { get; set; }
        public User User { get; set; } = null!;
    }
}
```

**Role Model**

Example:

```csharp
namespace Project.Models {
    public class Role {
        public int Id { get; set; }

        // ... the rest of the properties.

        // navigation property
        public ICollection<UserRole> UserRoles { get; set; } = new List<UserRole>();
    }
}
```

**UserRole Model**

Example:

```csharp
namespace Project.Models {
    public class UserRole {
        public int Id { get; set; }

        // ... the rest of the properties.

        public int UserId { get; set; }
        public User User { get; set; } = null!;

        public int RoleId { get; set; }
        public Role Role { get; set; } = null!;
    }
}
```

## Database Context

The `DbContext` is the main class that manages your entities and database connection.

You define your tables as `DbSet<T>` properties.

*See dependency_injection.md in design_pattern*

Example:

```csharp
using Microsoft.EntityFrameworkCore;
using Project.Models;

namespace Project {

    // That will be your main class for the database you will be using
    public class DatabaseContext : DbContext
    {
        // DbSet<User> Users - represents the table of model User.
        // The same with:  Roles and UserRoles.
        public DbSet<User> Users { get; set; }
        public DbSet<Laptop> Laptops { get; set; }
        public DbSet<Company> Companies { get; set; }
        public DbSet<Role> Roles { get; set; }
        public DbSet<UserRole> UserRoles { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // don't forget to use this, so the model building to continue to the end.
            base.OnModelCreating(builder);
            // Fluent API.
        }
    }


    public class Program
    {
        public static void Main(string[] args)
        {

            // Like that the class DatabaseContext becomes a Service and it can be used in dependency injection
            builder.Services.AddDbContext<DatabaseContext>(options =>
            {
                /* This will use the Postgres engine (Npgsql dependency)
                and the connection string from the configuration appsettings.json 
                
                {
                    "ConnectionString": "Host=...;Port=...;"
                }

                */

                options.UseNpgsql(builder.Configuration.GetValue<string>("ConnectionString"));
            });
        }
    }
}
```

## One to One

Example:

**Fluent API**
```csharp
builder.Entity<User>()
    .HasKey(user => user.Id);

builder.Entity<User>()
    .HasOne(user => user.Laptop)
    .WithOne(laptop => laptop.User)
    .HasForeignKey(user => user.LaptopId);
```

**Annotations**
```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

public class User
{
    [Key]
    public int Id { get; set; }

    public int LaptopId { get; set; }

    [ForeignKey("LaptopId")]
    public Laptop Laptop { get; set; }
}

public class Laptop
{
    [Key]
    public int Id { get; set; }

    public int UserId { get; set; }

    [ForeignKey("UserId")]
    public User User { get; set; }
}
```

## One to Many

Example:

**Fluent API**
```csharp
builder.Entity<User>()
    .HasKey(user => user.Id);

builder.Entity<User>()
    .HasOne(user => user.Company)
    .WithMany(company => company.Users)
    .HasForeignKey(user => user.CompanyId);
```

**Annotations**
```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

public class User
{
    [Key]
    public int Id { get; set; }

    public int CompanyId { get; set; }

    [ForeignKey("CompanyId")]
    public Company Company { get; set; } = null!;
}

public class Company
{
    [Key]
    public int Id { get; set; }

    // Navigation property
    public ICollection<User> Users { get; set; } = new List<User>();
}
```

## Many to Many

You need a **third model** to represent the **many-to-many** relationship.

In this example **UserRole** is the model, which represents the **third model**.

**Fluent API**
```csharp
builder.Entity<UserRole>()
    .HasKey(userRoles => userRoles.Id);

builder.Entity<UserRole>()
    .HasOne(userRoles => userRoles.User)
    .WithMany(user => user.UserRoles)
    .HasForeignKey(userRoles => userRoles.UserId);

builder.Entity<UserRole>()
    .HasOne(userRoles => userRoles.Role)
    .WithMany(role => role.UserRoles)
    .HasForeignKey(userRoles => userRoles.RoleId);
```

**Annotations**
```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

public class User
{
    [Key]
    public int Id { get; set; }

    // navigation property
    public ICollection<UserRole> UserRoles { get; set; } = new List<UserRole>();
}

public class Role
{
    [Key]
    public int Id { get; set; }

    // navigation property
    public ICollection<UserRole> UserRoles { get; set; } = new List<UserRole>();
}

public class UserRole
{
    [Key]
    public int Id { get; set; }

    public int UserId { get; set; }

    [ForeignKey("UserId")]
    public User User { get; set; } = null!;

    public int RoleId { get; set; }

    [ForeignKey("RoleId")]
    public Role Role { get; set; } = null!;
}
```


## Dotnet EF (Entity Framework Core)

Install globally

```bash
dotnet tool install --global dotnet-ef
```

Update globally

```bash
dotnet tool update --global dotnet-ef
```

Add the package

```bash
dotnet add package Microsoft.EntityFrameworkCore.Design
```

Verify

```bash
dotnet ef
```


*See https://learn.microsoft.com/en-us/ef/core/cli/dotnet*

