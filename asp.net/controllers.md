# ASP.NET Controllers

This is part of the **MVC** (Model View Controller).
The controller is like a middleman between the database and the client.
Every controller should be **stateless**.

## Stateful VS Stateless

**Stateless** is the condition of not keeping a state or not keeping track of anything in memory **(RAM)**. The **database** is the one which keeps the *data/state/track* of things.

**Stateful** is the opposite of **stateless**. It is the condition of holding and keeping track of data in memory **(RAM)**.

Example:

**Stateful**
```csharp
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;

[ApiController]
[Route("[controller]")]
public class StatefulController : ControllerBase
{
    private static List<string> names = new List<string>();

    [HttpGet("{index}")]
    public IActionResult Get(int index)
    {
        if (index < 0 || index >= names.Count)
            return NotFound();
        return Ok(names[index]);
    }

    [HttpPost("{name}")]
    public IActionResult Post(string name)
    {
        names.Add(name);
        return Ok();
    }
}
```

**Stateless**
```csharp
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("[controller]")]
public class StatelessController : Controller
{
    private readonly DatabaseContext _db;

    public StatelessController(DatabaseContext db)
    {
        _db = db;
    }

    [HttpGet("{name}")]
    public IActionResult Get(string name)
    {
        var result = _db.Names.FirstOrDefault(n => n == name);
        if (result == null)
            return NotFound();
        return Ok(result);
    }

    [HttpPost("{name}")]
    public IActionResult Post(string name)
    {
        _db.Names.Add(name);
        _db.SaveChanges();
        return Ok();
    }
}
```

>**Important!**
>
>**Stateful** doesn't always mean it is stored in memory. 
Look at the **cookies**. They hold a **state**, but without the **cookies**, managing/holding a **state** will be a bit more difficult.

**Why the controller should be stateless?**

Because the controller acts as an anonymous middleman. If you need to scale and add more machines to run your web server, then guess what. These two or **more machines** which run your server will have different data in them, because they will each use their **own memory** to store and keep track of data. This is why they should use one database, so the data they work with may be the same to every server. This also applies to **configuration** *(**Kubernetes** or **Docker Swarm**)* and **cache**.

## Minimal API

It doesn't require a whole class to register an endpoint, but it can't use **Views *(return View("ViewName", Model))*** or **RazorPages**, but it can render a normal **HTML** website and return status codes.

You have to return **IResult**.

Example:

```csharp
app.MapGet("/names/{name}", (string name, DatabaseContext db) =>
{
    var result = db.Names.FirstOrDefault(n => n == name);
    if (result == null) {
        return Results.NotFound();
    }
    return Results.Ok(result);
});

app.MapPost("/names/{name}", (string name, DatabaseContext db) =>
{
    db.Names.Add(name);
    db.SaveChanges();
    return Results.Ok();
});
```

Use it when things are simple, and you don't need a lot of features and also want to keep stuff nice and clean.

### Settings

- **.DisableAntiforgery()** - **Disables** the check for **Anti-forgery Token**, which is **CSRF Token**.

- **.DisableRateLimiting()** - **Disables** the rate limiter.

- **.RequireRateLimiting("policyName")** - **Enables** that rate limit for this *endpoint*.

- **.DisableRequestTimeout()** - **Disables** the *timeout* of that endpoint. **Timeout** is when the server **doesn't respond** in time and throws this *timeout exception*. This disables the throwing of that *exception*, so the client has to do this timeout *exception throwing*.

- **.Accepts("contentType", "another content type")** - Accepts such *content types* of that *endpoint*. It helps with **OpenAPI/Swagger documentation** generation.

- **.RequireCors()** - **Enables** *CORS (Cross-Origin Resource Sharing)* with default policy **__DefaultCorsPolicy**.

- **.RequireCors(...)** - **Enables** *CORS (Cross-Origin Resource Sharing)* with that policy.

- **.RequireHost(...)** - **Limits** the allowed hosts, which can *access* the endpoint.

## Class Controller

*Note: You can put whatever **annotations** you want on top of the controller, so you won't have to put them for every **endpoint** in that **controller**.*

Example:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.RateLimiting;

namespace Project.Controllers {


    [ApiController]
    [EnableRateLimiting("fixed")]
    public class UserController : Controller {

        public UserController() { }

        [HttpGet]
        [Route("/")]
        [Route("/login")]
        public IActionResult Login()
        {
            return View();
        }


        [HttpGet]
        [Route("/register")]
        public IActionResult Register()
        {
            return View();
        }


        [HttpPost]
        [Route("/login")]
        [IgnoreAntiforgeryToken]
        public async Task<IActionResult> Login([FromForm] LoginModel login)
        {
            if (!ModelState.IsValid)
            {
                return RedirectToAction("Login", login);
            }

            // do login logic here

            return RedirectToAction("Index", "Home");
        }
    }
}
```

As you see, you have to return **IActionResult** and not **IResult**. 

Here you can use **RazorPages** and **Views**.

### Annotations

- **[ApiController]** - Marks it as a controller if it has not yet been registered.

- **[EnableRateLimiting("fixed")]** - **Enables** the custom configured **rate limiter**.

- **[DisableRateLimiting]** - **Disables** the rate limiter.

- **[HttpGet]** - Makes the **endpoint** to be **HTTP GET**.

- **[HttpPost]** - Makes the **endpoint** to be **HTTP POST**.

- **[Route("/login")]** - Responds to the **endpoint /login**.

- **[IgnoreAntiforgeryToken]** - **Disables** the check for **Anti-forgery Token**, which is **CSRF Token**.

- **[AutoValidateAntiforgeryToken]** - **Enables** the check for **Anti-forgery Token**, which is **CSRF Token**.

- **[DisableRequestSizeLimit]** - **Disables** the limitation of the **body size**.

- **[RequestSizeLimit(1234)]** - Sets **body limit** to the given **size in long**. See *int* and *long* data types.

- **[EnableCors]** - **Enables** *CORS (Cross-Origin Resource Sharing)* with default policy **__DefaultCorsPolicy**.

- **[EnableCors("policyName")]** - **Enables** *CORS (Cross-Origin Resource Sharing)* with that policy.

- **[DisableCors]** - **Disables** any *CORS (Cross-Origin Resource Sharing)*.

- **[RequireHttps]** - **Only** executes, when the request is **HTTPS**.

- **[RequestFormLimits]** - Sets **default Form Limitations** like **Keys**, **Values**, **Body Size**, **Count** etc.

- **[RequestFormLimits(...)]** - Sets **custom Form Limitations** like **Keys**, **Values**, **Body Size**, **Count** etc.

- **[Consumes("content type", "additional content type")]** - Accepts such *content types* of that *endpoint*. It helps with **OpenAPI/Swagger documentation** generation.

## Redirection

Redirect only for the **GET** request, otherwise the header **Location**, which is sent by the server, will have no effect, and it will return a status code of **200** if the redirection was successful, but you don't need that and this will only confuse the developers.

Let the **JS (JavaScript)** or the client side take care of all the redirection.

**POST**, **PATCH**, **DELETE** etc. should be **stateless**. This is why we return a status code and not a redirection.

### Class Controllers (returns IActionResult)

You can return **Ok()** or **BadRequest()**.

### Minimal API (returns IResult)

You can return **Results.Ok()** or **Results.BadRequest()**.
