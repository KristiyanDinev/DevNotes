# Web Security: CORS, CORP and CSP

## Cross-Origin Resource Sharing (CORS)
CORS is a security feature that controls which websites can access resources on your server.

It uses HTTP headers to allow or deny access when a site from one origin tries to request resources from a different origin. CORS helps protect against cross-site request forgery attacks while allowing legitimate cross-origin requests. It extends the Same-Origin Policy (SOP) that normally prevents JavaScript from making requests to domains other than the one that served the original web page.

Example: Imagine you have a restaurant (your website) that needs ingredients (data) from a neighboring grocery store (another website's API). Without CORS, your chef (JavaScript) can only get ingredients from your own kitchen, not from the grocery store across the street. CORS acts like an agreement between your restaurant and the grocery store, allowing your chef to go there and pick up specific ingredients under certain conditions.

**Client Code Examples**

```javascript
// JavaScript
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

```java
// Java (using HttpClient from Java 11+)
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
        .uri(URI.create("https://api.example.com/data"))
        .build();

client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
      .thenApply(HttpResponse::body)
      .thenAccept(System.out::println)
      .exceptionally(e -> {
          System.err.println("Error: " + e.getMessage());
          return null;
      });
```

```python
# Python (using requests)
import requests

try:
    response = requests.get('https://api.example.com/data')
    response.raise_for_status()  # Raise exception for 4XX/5XX responses
    data = response.json()
    print(data)
except requests.exceptions.RequestException as e:
    print(f"Error: {e}")
```

```csharp
// C# (using HttpClient)
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        using var client = new HttpClient();
        try 
        {
            var response = await client.GetStringAsync("https://api.example.com/data");
            Console.WriteLine(response);
        }
        catch (HttpRequestException e)
        {
            Console.WriteLine($"Error: {e.Message}");
        }
    }
}
```

**Server Code Examples**

```java
// Java (using Spring Boot)
@RestController
public class ApiController {
    
    @GetMapping("/data")
    @CrossOrigin(origins = "https://yourwebsite.com")  // Allows requests only from this origin
    public ResponseEntity<YourData> getData() {
        return ResponseEntity.ok(new YourData());
    }
}
```

```javascript
// Node.js (Express)
const express = require('express');
const cors = require('cors');
const app = express();

// Allow specific origin
app.use(cors({
  origin: 'https://yourwebsite.com'
}));

app.get('/data', (req, res) => {
  res.json({ message: 'This is your data' });
});

app.listen(3000);
```

```python
# Python (Flask)
from flask import Flask
from flask_cors import CORS

app = Flask(__name__)
CORS(app, resources={r"/data": {"origins": "https://yourwebsite.com"}})

@app.route('/data')
def get_data():
    return {"message": "This is your data"}

if __name__ == '__main__':
    app.run()
```

```csharp
// C# (ASP.NET Core)
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors(options =>
    {
        options.AddPolicy("AllowSpecificOrigin",
            builder => builder.WithOrigins("https://yourwebsite.com"));
    });
    
    services.AddControllers();
}

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // Other middleware
    
    app.UseCors("AllowSpecificOrigin");
    
    app.UseRouting();
    app.UseEndpoints(endpoints => 
    {
        endpoints.MapControllers();
    });
}
```

### CORS with Preflight Requests

When you make a complex request (like using a DELETE method or custom headers), your browser first sends a "preflight" request using the **OPTIONS** method to check if the actual request is allowed. This is like calling a restaurant before visiting to see if they can accommodate your dietary restrictions.

**Client Code Examples**

```javascript
// JavaScript
fetch('https://api.example.com/users/123', {
  method: 'DELETE',
  headers: {
    'Content-Type': 'application/json',
    'X-Custom-Header': 'value'
  }
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

```java
// Java
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
        .uri(URI.create("https://api.example.com/users/123"))
        .header("Content-Type", "application/json")
        .header("X-Custom-Header", "value")
        .DELETE()
        .build();

client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
      .thenApply(HttpResponse::body)
      .thenAccept(System.out::println)
      .exceptionally(e -> {
          System.err.println("Error: " + e.getMessage());
          return null;
      });
```

```python
# Python
import requests

headers = {
    'Content-Type': 'application/json',
    'X-Custom-Header': 'value'
}

try:
    response = requests.delete('https://api.example.com/users/123', headers=headers)
    response.raise_for_status()
    data = response.json()
    print(data)
except requests.exceptions.RequestException as e:
    print(f"Error: {e}")
```

```csharp
// C#
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        using var client = new HttpClient();
        var request = new HttpRequestMessage
        {
            Method = HttpMethod.Delete,
            RequestUri = new Uri("https://api.example.com/users/123")
        };
        
        request.Headers.Add("X-Custom-Header", "value");
        
        try 
        {
            var response = await client.SendAsync(request);
            response.EnsureSuccessStatusCode();
            var content = await response.Content.ReadAsStringAsync();
            Console.WriteLine(content);
        }
        catch (HttpRequestException e)
        {
            Console.WriteLine($"Error: {e.Message}");
        }
    }
}
```

**Server Code Examples**

```java
// Java (Spring Boot)
@RestController
public class UserController {
    
    @DeleteMapping("/users/{id}")
    @CrossOrigin(
        origins = "https://yourwebsite.com",
        allowedHeaders = {"Content-Type", "X-Custom-Header"},
        methods = {RequestMethod.DELETE, RequestMethod.OPTIONS}
    )
    public ResponseEntity<String> deleteUser(@PathVariable String id) {
        // Delete user logic
        return ResponseEntity.ok("User deleted");
    }
}
```

```javascript
// Node.js (Express)
const express = require('express');
const cors = require('cors');
const app = express();

app.use(cors({
  origin: 'https://yourwebsite.com',
  methods: ['GET', 'POST', 'DELETE', 'OPTIONS'],
  allowedHeaders: ['Content-Type', 'X-Custom-Header']
}));

app.delete('/users/:id', (req, res) => {
  // Delete user logic
  res.json({ message: 'User deleted' });
});

app.listen(3000);
```

```python
# Python (Flask)
from flask import Flask, jsonify
from flask_cors import CORS

app = Flask(__name__)
CORS(app, 
     resources={r"/users/*": {
         "origins": "https://yourwebsite.com",
         "methods": ["DELETE", "OPTIONS"],
         "allow_headers": ["Content-Type", "X-Custom-Header"]
     }})

@app.route('/users/<string:user_id>', methods=['DELETE'])
def delete_user(user_id):
    # Delete user logic
    return jsonify({"message": "User deleted"})

if __name__ == '__main__':
    app.run()
```



## Cross-Origin Resource Policy (CORP)
CORP is a security mechanism that lets servers prevent their resources from being loaded by other origins.

It uses a simple HTTP response header to indicate whether a resource can be shared cross-origin.
CORP is more restrictive than CORS and focuses on protecting resources from being embedded or loaded by unauthorized origins.

## Content Security Policy (CSP)
CSP is a defense mechanism against code injection attacks like Cross-Site Scripting (XSS).

It uses HTTP headers to declare which content sources are trusted and allowed to execute on your page.










## CORS (Cross-Origin Resource Sharing)



#### Scenario 3: CORS with Credentials

**Non-Code Explanation:**
Sometimes you need to include cookies or authentication with your cross-origin requests. This is like visiting that restaurant across the street and needing to bring your membership card to get service. Both your website and the API must explicitly allow credentials for this to work.

**Code Example:**

```javascript
// JavaScript
fetch('https://api.example.com/profile', {
  credentials: 'include'  // This sends cookies with the request
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

```java
// Java
HttpClient client = HttpClient.newBuilder()
        .cookieHandler(new CookieManager())
        .build();
        
HttpRequest request = HttpRequest.newBuilder()
        .uri(URI.create("https://api.example.com/profile"))
        .build();

client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
      .thenApply(HttpResponse::body)
      .thenAccept(System.out::println)
      .exceptionally(e -> {
          System.err.println("Error: " + e.getMessage());
          return null;
      });
```

```python
# Python
import requests

try:
    response = requests.get('https://api.example.com/profile', cookies=cookies_jar)
    response.raise_for_status()
    data = response.json()
    print(data)
except requests.exceptions.RequestException as e:
    print(f"Error: {e}")
```

```csharp
// C#
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        var handler = new HttpClientHandler
        {
            UseCookies = true
        };
        
        using var client = new HttpClient(handler);
        try 
        {
            var response = await client.GetStringAsync("https://api.example.com/profile");
            Console.WriteLine(response);
        }
        catch (HttpRequestException e)
        {
            Console.WriteLine($"Error: {e.Message}");
        }
    }
}
```

**Server Configuration:**

```java
// Java (Spring Boot)
@RestController
public class ProfileController {
    
    @GetMapping("/profile")
    @CrossOrigin(
        origins = "https://yourwebsite.com",
        allowCredentials = "true"  // This is crucial for allowing credentials
    )
    public ResponseEntity<UserProfile> getProfile() {
        // Get profile logic
        return ResponseEntity.ok(new UserProfile());
    }
}
```

```javascript
// Node.js (Express)
const express = require('express');
const cors = require('cors');
const app = express();

app.use(cors({
  origin: 'https://yourwebsite.com',
  credentials: true  // This is crucial for allowing credentials
}));

app.get('/profile', (req, res) => {
  // Get profile using session cookie
  res.json({ name: 'User Name', email: 'user@example.com' });
});

app.listen(3000);
```

## CORP (Cross-Origin Resource Policy)

### What is CORP?

CORP is a newer security mechanism that allows web servers to prevent their resources from being loaded by other origins, offering an additional layer of protection against certain types of attacks like Spectre. While CORS controls access to the response data, CORP controls whether the resource can be loaded at all.

### Non-Code Explanation

If CORS is like a restaurant deciding which customers from other neighborhoods can order takeout, CORP is like deciding whether anyone outside your own neighborhood can even see your menu. It's a stricter boundary that helps prevent your resources from being used in ways you didn't intend.

### Common CORP Scenarios

#### Scenario 1: Same-Origin Only

**Non-Code Explanation:**
You want to ensure a sensitive resource (like an internal API endpoint) can only be loaded by your own website and not embedded or used by any other site.

**Server Configuration:**

```java
// Java (Servlet Filter)
@WebFilter("/*")
public class CORPFilter implements Filter {
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) 
        throws IOException, ServletException {
        
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        httpResponse.setHeader("Cross-Origin-Resource-Policy", "same-origin");
        
        chain.doFilter(request, response);
    }
}
```

```javascript
// Node.js (Express)
app.use((req, res, next) => {
  res.setHeader('Cross-Origin-Resource-Policy', 'same-origin');
  next();
});
```

```python
# Python (Flask)
@app.after_request
def add_corp_header(response):
    response.headers['Cross-Origin-Resource-Policy'] = 'same-origin'
    return response
```

```csharp
// C# (ASP.NET Core middleware)
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    app.Use(async (context, next) =>
    {
        context.Response.Headers.Add("Cross-Origin-Resource-Policy", "same-origin");
        await next();
    });
    
    // Other middleware and configurations
}
```

#### Scenario 2: Same-Site Policy

**Non-Code Explanation:**
You want to allow resources to be loaded by any subdomains of your main domain (e.g., allowing app.example.com to access resources from api.example.com), but not by any other unrelated sites.

**Server Configuration:**

```java
// Java
@WebFilter("/*")
public class CORPFilter implements Filter {
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) 
        throws IOException, ServletException {
        
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        httpResponse.setHeader("Cross-Origin-Resource-Policy", "same-site");
        
        chain.doFilter(request, response);
    }
}
```

```javascript
// Node.js (Express)
app.use((req, res, next) => {
  res.setHeader('Cross-Origin-Resource-Policy', 'same-site');
  next();
});
```

#### Scenario 3: Cross-Origin Policy

**Non-Code Explanation:**
You have public resources (like images or public APIs) that you want to be embeddable anywhere on the web.

**Server Configuration:**

```java
// Java
@WebFilter("/public/*")
public class PublicResourceCORPFilter implements Filter {
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) 
        throws IOException, ServletException {
        
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        httpResponse.setHeader("Cross-Origin-Resource-Policy", "cross-origin");
        
        chain.doFilter(request, response);
    }
}
```

```javascript
// Node.js (Express)
app.use('/public', (req, res, next) => {
  res.setHeader('Cross-Origin-Resource-Policy', 'cross-origin');
  next();
});
```

## CSP (Content Security Policy)

### What is CSP?

Content Security Policy is a powerful security feature that helps prevent Cross-Site Scripting (XSS) attacks by controlling which resources can be loaded and executed on your web page. It works by specifying which domains are trusted sources of executable scripts, stylesheets, images, fonts, etc.

### Non-Code Explanation

Think of CSP as a security guard for your website that checks the ID of every resource trying to run on your page. You give the guard a list of trusted sources, and anything not on that list gets blocked. This prevents attackers from injecting and executing malicious code on your website, even if they find an XSS vulnerability.

### Common CSP Scenarios

#### Scenario 1: Restricting Script Sources

**Non-Code Explanation:**
You want to ensure that your website only executes JavaScript from your own domain and a few trusted CDNs like Google and Cloudflare, blocking all other script sources to prevent XSS attacks.

**Server Configuration:**

```java
// Java (Servlet Filter)
@WebFilter("/*")
public class CSPFilter implements Filter {
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) 
        throws IOException, ServletException {
        
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        httpResponse.setHeader("Content-Security-Policy", 
            "default-src 'self'; " +
            "script-src 'self' https://cdn.jsdelivr.net https://ajax.googleapis.com; " +
            "style-src 'self' https://fonts.googleapis.com; " +
            "img-src 'self' data:; " +
            "font-src 'self' https://fonts.gstatic.com;");
        
        chain.doFilter(request, response);
    }
}
```

```javascript
// Express.js
const helmet = require('helmet');

app.use(helmet.contentSecurityPolicy({
  directives: {
    defaultSrc: ["'self'"],
    scriptSrc: ["'self'", 'cdn.jsdelivr.net', 'ajax.googleapis.com'],
    styleSrc: ["'self'", 'fonts.googleapis.com'],
    imgSrc: ["'self'", 'data:'],
    fontSrc: ["'self'", 'fonts.gstatic.com']
  }
}));
```

```python
# Python (Flask)
@app.after_request
def add_csp_header(response):
    csp = (
        "default-src 'self'; "
        "script-src 'self' https://cdn.jsdelivr.net https://ajax.googleapis.com; "
        "style-src 'self' https://fonts.googleapis.com; "
        "img-src 'self' data:; "
        "font-src 'self' https://fonts.gstatic.com;"
    )
    response.headers['Content-Security-Policy'] = csp
    return response
```

```csharp
// C# (ASP.NET Core)
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    app.Use(async (context, next) =>
    {
        context.Response.Headers.Add(
            "Content-Security-Policy",
            "default-src 'self'; " +
            "script-src 'self' https://cdn.jsdelivr.net https://ajax.googleapis.com; " +
            "style-src 'self' https://fonts.googleapis.com; " +
            "img-src 'self' data:; " +
            "font-src 'self' https://fonts.gstatic.com;"
        );
        await next();
    });
    
    // Other middleware and configurations
}
```

#### Scenario 2: Implementing a Strict CSP

**Non-Code Explanation:**
You're building a banking application and want the strictest possible security. You want to disable inline scripts completely, require subresource integrity for third-party resources, and report any violations to your security team.

**Server Configuration:**

```java
// Java
@WebFilter("/*")
public class StrictCSPFilter implements Filter {
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) 
        throws IOException, ServletException {
        
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        httpResponse.setHeader("Content-Security-Policy", 
            "default-src 'self'; " +
            "script-src 'self'; " + // No inline scripts allowed
            "object-src 'none'; " + // No plugins like Flash or Java
            "base-uri 'self'; " + // Restricts use of <base> tag
            "frame-ancestors 'none'; " + // Prevents site from being framed
            "form-action 'self'; " + // Forms can only submit to same origin
            "upgrade-insecure-requests; " + // Upgrades HTTP to HTTPS
            "block-all-mixed-content; " + // Blocks mixed content
            "report-uri https://example.com/csp-report"); // Reports violations
        
        chain.doFilter(request, response);
    }
}
```

```javascript
// Node.js (Express with Helmet)
const helmet = require('helmet');

app.use(helmet.contentSecurityPolicy({
  directives: {
    defaultSrc: ["'self'"],
    scriptSrc: ["'self'"], // No inline scripts
    objectSrc: ["'none'"], // No plugins 
    baseUri: ["'self'"], // Restricts <base> tag
    frameAncestors: ["'none'"], // No framing
    formAction: ["'self'"], // Forms only submit to same origin
    upgradeInsecureRequests: [], // Upgrade HTTP to HTTPS
    blockAllMixedContent: [], // Block mixed content
    reportUri: ['https://example.com/csp-report'] // Report violations
  }
}));
```

#### Allowing Inline Scripts with Nonces

You can use nonces (random one-time tokens) to allow specific inline scripts while blocking all others scripts in the HTML without the nonces.

Fun fact: It is close to a **none sense** in terms of pronunciation.

*See https://content-security-policy.com/nonce/#:~:text=Using%20a%20nonce%20is%20one,denote%20a%20random%20nonce%20value.*

**Server Configuration & Implementation:**

```java
// Java (Spring Boot)
@Controller
public class HomeController {
    
    @GetMapping("/")
    public String home(Model model) {
        String nonce = generateRandomNonce(); // Generate a random string
        model.addAttribute("nonce", nonce);
        
        response.setHeader("Content-Security-Policy", 
            "default-src 'self'; " +
            "script-src 'self' 'nonce-" + nonce + "';");
        
        return "home";
    }
    
    private String generateRandomNonce() {
        byte[] nonceBytes = new byte[16];
        new SecureRandom().nextBytes(nonceBytes);
        return Base64.getEncoder().encodeToString(nonceBytes);
    }
}
```

In your HTML template:

```html
<!-- Thymeleaf template -->
<script nonce="${nonce}">
    // This inline script will be allowed because it has the correct nonce
    document.getElementById('greeting').textContent = 'Welcome!';
</script>
```

```javascript
// Node.js (Express)
app.use((req, res, next) => {
  // Generate a random nonce
  const nonce = Buffer.from(crypto.randomBytes(16)).toString('base64');
  
  // Set CSP header with the nonce
  res.setHeader('Content-Security-Policy',
    `default-src 'self'; script-src 'self' 'nonce-${nonce}'`);
  
  // Make nonce available to templates
  res.locals.nonce = nonce;
  next();
});
```

In your EJS template:

```html
<!-- EJS template -->
<script nonce="<%= nonce %>">
    // This inline script will be allowed
    document.getElementById('greeting').textContent = 'Welcome!';
</script>
```


### How These Security Mechanisms Work Together

- **CORS** - controls which origins can access your API or server resources
- **CORP** - controls whether your resources can be loaded or embedded by other origins
- **CSP** - controls what content can be loaded and executed on your page

### Implementation Checklist

1. **For APIs or servers:**
   - Configure CORS to allow only trusted origins
   - Set appropriate CORP headers based on how your resources should be used
   - Consider different settings for different endpoints (public vs. private)

2. **For frontend applications:**
   - Implement a strong CSP to prevent XSS attacks
   - Only allow the minimal required external sources in your CSP
   - Use nonces or hashes if inline scripts are necessary
   - Consider using CSP reporting to detect any violations

### Common Mistakes to Avoid

- Don't use `Access-Control-Allow-Origin: *` in production unless your API is truly public
- Don't disable CSP entirely or use unsafe-inline without nonces/hashes
- Don't forget to handle preflight requests in CORS configurations
- Test thoroughly with real browsers - many security issues only appear in real environments
