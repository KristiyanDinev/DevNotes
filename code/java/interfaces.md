# Interfaces in Java

In Java, **interfaces** provide a way to define a contract that classes can implement. They are an essential part of **abstraction** and **polymorphism** in Java.

## What is an Interface?

An **interface** is like a *blueprint* for classes. It contains.

* **Abstract methods** *(no body in older Java versions).*
* **Default methods** *(with a body, since Java 8).*
* **Static methods** *(with a body, since Java 8).*
* **Constants** *(`public static final` fields).*
* **Properties** *(only if you give them a value).*

## The `interface` Keyword

To define an **interface**, use the `interface` keyword.

```java
public interface Animal {
    void makeSound();   // abstract method
}
```

## Abstract Methods in Interfaces

* **Declaration:**

  ```java
  void doSomething();
  ```
* **No body:** Abstract methods **do not have a body** *(up to Java 7).*
* **Public & abstract by default:** No need to declare them with these modifiers explicitly.

---

## Default and Static Methods

Since **Java 8**, interfaces can have **methods** with a body.

### Default Methods

* Use the `default` keyword.
* Can be overridden by **implementing** classes.

```java
default void sleep() {
    System.out.println("Sleeping...");
}
```

### Static Methods

* Use the `static` keyword.
* Belong to the **interface** itself *(cannot be overridden)*.

```java
static void greet() {
    System.out.println("Hello!");
}
```

---

## Key Differences

| Feature            | Abstract Method     | Default Method      | Static Method       |
| ------------------ | ------------------- | ------------------- | ------------------- |
| Body?              | ❌ No                | ✅ Yes               | ✅ Yes               |
| Can be overridden? | ✅ Must              | ✅ Can               | ❌ Cannot            |
| Access Modifier    | `public` by default | `public` by default | `public` by default |

---

## Implementing Interfaces

To implement an interface, a class uses the `implements` keyword:

```java
class Dog implements Animal {  // can have multiple implements separated by `,`
    @Override
    public void makeSound() {   // must override abstract method
        System.out.println("Woof!");
    }
}
```

## Example

```java
interface Animal {
    void makeSound();  // abstract method
    default void sleep() {
        System.out.println("Sleeping...");
    }
    static void greet() {
        System.out.println("Hello!");
    }
}

class Cat implements Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow!");
    }
}

public class Test {
    public static void main(String[] args) {
        Cat cat = new Cat();
        cat.makeSound();  // Meow!
        cat.sleep();      // Sleeping...
        Animal.greet();   // Hello!
    }
}
```
