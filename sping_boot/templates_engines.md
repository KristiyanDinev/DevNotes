# Template Engines

Templates engines are just a HTML rendering engine, which takes the HTML template you wrote and the `param`/`model`/`the Java code your want to put in your HTML` and it renders it to a plain HTML content *(AKA String)* and sends it back to the client.

## Thymeleaf

**Example**
```html
<html>
        <head>
            <meta charset="ISO-8859-1" />
            <title>User Registration</title>
        </head>
        <body>
            <form action="#" th:action="@{/register}" 
            th:object="${user}" method="post">
                Email:<input type="text" th:field="*{email}" />
                Password:<input type="password" th:field="*{password}" />
                <input type="submit" value="Submit" />
            </form>
        </body>
</html>
```

## FreeMarker

**Example:**
```html
<#import "/spring.ftl" as spring/>
<html>
    <head>
        <meta charset="ISO-8859-1" />
        <title>User Registration</title>
    </head>
    <body>
        <form action="register" method="post">
            <@spring.bind path="user" />
            Email: <@spring.formInput "user.email"/>
            Password: <@spring.formPasswordInput "user.password"/>
            <input type="submit" value="Submit" />
        </form>
    </body>
</html>
```

## JSP (Java Server Pages)

**Example:**

```html
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form"%>
<html>
    <head>
        <meta http-equiv="Content-Type" 
        content="text/html; charset=ISO-8859-1">
        <title>User Registration</title>
    </head>
    <body>
        <form:form method="POST" modelAttribute="user">
            <form:label path="email">Email: </form:label>
            <form:input path="email" type="text"/>
            <form:label path="password">Password: </form:label>
            <form:input path="password" type="password" />
            <input type="submit" value="Submit" />
        </form:form>
    </body>
</html>
```

## Groovy Markup

**Example:**

```groovy
yieldUnescaped '<!DOCTYPE html>'                                                    
html(lang:'en') {                                                                   
    head {                                                                          
        meta('http-equiv':'"Content-Type" ' +
          'content="text/html; charset=utf-8"')      
        title('User Registration')                                                            
    }                                                                               
    body {                                                                          
        form (id:'userForm', action:'register', method:'post') {
            label (for:'email', 'Email')
            input (name:'email', type:'text', value:user.email?:'')
            label (for:'password', 'Password')
            input (name:'password', type:'password', value:user.password?:'')
            div (class:'form-actions') {
                input (type:'submit', value:'Submit')
            }                             
        }
    }                                                                               
}
```

## Jade4j

**Example:**

```jade
doctype html
html
  head
    title User Registration
  body
    form(action="register" method="post" )
      label(for="email") Email:
      input(type="text" name="email")
      label(for="password") Password:
      input(type="password" name="password")
      input(type="submit" value="Submit")
```

## JTE

*My fav. It is a lot like ASP.NET Razor Pages and it is a lot easier to learn and code with it. Less code, modern look and more readable.*

**Example:**

```html
@import com.baeldung.spring.domain.Message
@param Message message

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Baeldung - Duke Page</title>
</head>
<body>
    <form action="/welcome" method="post">
        Name: <input type="text" name="text" value="${message.getName()}">
        Message: <input type="submit" value="${message.getText()}">
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```