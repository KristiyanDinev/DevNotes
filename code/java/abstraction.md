# Abstract Classes in Java

In Java, **abstract classes** provide a way to define a class that **cannot be instantiated** and may contain abstract methods that must be implemented by subclasses.

## What is an Abstract Class?

An **abstract class** is a class that is declared using the `abstract` keyword. It can contain:

* **Abstract methods** *(methods without a body).*
* **Concrete methods** *(methods with a body).*
* **Properties** *(like normal classes).*

## Abstract Methods

* **Declaration:**

  ```java
  abstract void doSomething();
  ```

* **No body:** Abstract methods **do not have a body**. They end with a semicolon (`;`).

* **Purpose:** They define a contract that **subclasses** must follow.

## Concrete (Non-Abstract) Methods

* **Abstract** classes can also contain **concrete methods** that **have a body**.

* Example:

  ```java
  void concreteMethod() {
      System.out.println("This is a concrete method.");
  }
  ```

## Overriding Abstract and Concrete Methods

| Type of Method | Required to Override in Subclass?                        |
| -------------- | -------------------------------------------------------- |
| **Abstract**   | ✅ Yes, must override in the first non-abstract subclass. |
| **Concrete**   | ❌ No, can override if desired (not required).            |



## Example

```java
abstract class Animal {
    abstract void makeSound();  // abstract method - no body

    void sleep() {              // concrete method - has a body
        System.out.println("Sleeping...");
    }
}

class Dog extends Animal {      // can extend only one class
    @Override
    void makeSound() {          // must override abstract method
        System.out.println("Woof!");
    }
}
```
