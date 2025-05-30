# Clean Code

Check out your **folder** and **file names**. 

Here are the most **commonly** used **folders**, so that you may use them as well:

## Folders

>*Note: In **Java** you write your **folder** names in all **lower** case and **file** names as shown below. In **C#** the files can be the same as the **Java** files, but the **folders** and **namespaces** are written with a **first** letter capital, and then the rest of the name. Like: **/Controllers**, but in **Java** it is **/controllers**.**

**1. /Controllers** - Used in web **APIs** or **webservers** for handling **endpoints** and **requests**.

- The file names would look like that: **UserController.java**, **OrdersController.java** or **PaymentController.java**.

**2. /Models** - All the **models** the project is using. A **model** is a class which represents specific **data** in its *properties* or *fields*.

- The file names would look like that: **UserModel.java**, **OrdersModel.java** or **PaymentModel.java**.

**3. /Views** - For all the front-end code, usually templates. In **Spring Boot** you may find the templates in **src/main/resources/templates/** folder, if your project is using **Thymeleaf** as a template engine.

In **ASP.NET** you will see this exact structure for **Views** like: **/Views/Home/Index.cshtml**.

- The programming/code file names would look like that, if you will have any: **UserView.java**, **OrdersView.java** or **PaymentView.java**.

**4. /Utilities** - All the **Utilities**, which the code uses for basic or complex code operations, which as algorithms or conversions. You may also find the name **/Utils**, but it is short for **Utilities**. Use words like **Utilities**, rather than **Utils**, because that word exists in the English dictionary, rather than **Utils**, but in any case, if you feel like **Utils** would be a better name, then go for it.

- The file names would look like that: **UserUtility.java**, **OrdersUtility.java** or **PaymentUtility.java**.

**5. /Services** - All the **Services** which are used to make life easier for the developers. This folder can be in both **ASP.NET** and **Spring Boot**. A **Service** is a class that has important classes for the functionality of the whole project. That class is special, because **Services** are normally managed and handled by the **framework** itself and the **framework** knows where to use them and where not.

#### Can you register the Utility class as a Service class?

Yes you can, but please make sure that you keep the **Single Responsibility** principle. In order to keep it, please take a look at the rest of the **Services** and **Utilities** classes and see how you can adapt your class to it. Maybe you have to make some changes to the class, so it may adapt to the rest of the classes, or maybe to remove or rename something. If you still don't know what to do, please contact the Manager or a co-worker for extra guidance.

- The file names would look like that: **UserService.java**, **OrdersService.java** or **PaymentService.java**.

**6. /Database** - Guess what. This has all the classes *related* or *dedicated* to the **database** the project is using. You may see multiple **database** implementations if your project requires you to support them all.

- The file names would look like that: **DatabaseManager.java**, **DatabaseContext.java** or **ConnectionHandler.java**.

**7. /Middlewares** - 

**8. /Config**

- In a **webserver** or web **API** project. You may find yourself using the same *algorithm* or function over and over again. Whether it be in different classes or **controllers** etc. You are using it. If that function is only for **database** stuff, then you can put it in **Services**, or if it handles the *requests*, you can put it in **Utilities**. Remember to keep your file names good, for *readable* and *cleaner code*.

## Data Structure

If you have **nested** variables with these types: `List`, `Hash`, `Array`, `Dictionary`, `Map` etc. 

Then consider creating a **class** to minimize confusion for other developers.

Example:

**Java**
```java
// key: int user id, value: list of usernames
Map<Integer, List<String>> users = new Map<Integer, List<String>>();
```

To

```java
public class Users {
    public int id;
    public List<String> names;
    // ...
}
```

## File Paths

- `/folder` - **Absolute Path** *(basically the full/whole path for that folder starting from the ***root***).*

- `folder/` - **Relative Path** *(adds to your ***current*** path).*

### Web Urls

- `https://example.com/folder` - Could be a *file* or (directory)
- `https://example.com/folder/` - Definitely a *directory*, will show index page