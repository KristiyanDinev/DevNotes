# MVC (Model View Controller)

## Model

Think of this as the **data** which the **server** has for the **user**.

Example:
```java
class UserModel {
    public int id;
    public String name;
    public int age;
}
```

## View

It is the **front-end** or the *"how the user sees that data or model"*. It is typically a **web page**, a **mobile**, **desktop app** or a **PDF** or some other **document** from which the **user** see the **data**.

You can also add **styles** to it to make it look **good**, but I don't think it is **required**.

## Controller

This **controls** which **model** with what **view** to respond to the **client's request**. 
Sometimes you **respond** with a **document** or just **JSON** without a **front-end**. 

For **example**, you will need a **View** if you work with **webpages** or something that will **display** the **data**.
The **controller** can also execute some **business logic** behind the scenes which can also **modify** the **model**, before **sending** it with the **view**.

