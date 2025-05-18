# Comparison

This is when you are to filter or do some work for every element in a list/array.

We will cover the comparison which is `String.compareTo(object)` in a **Java** or `string.CompareTo(object? value)` in **C#**.

You can add custom comparison in both languages.
Example:

**Java**
```java
public class BankAccount {
    public int balance;

    public BankAccount() {
        balance = 0;
    }

    @Override
    public int compareTo(BankAccount anotherAccount) {
        if (this.balance == anotherAccount.balance) {
            return 0;

        } else if (this.balance < anotherAccount.balance) {
            return -1;

        } else {
            // else if (this.balance > anotherAccount.balance)
            return 1;
        }
    }
}
```

**C#**
```c#
using System;
using System.Collections;

public class Temperature : IComparable
{
    public double temperatureF;

    public Temperature() {
        temperatureF = 0.0;
    }

    public int CompareTo(object obj) {
        if (this.temperatureF == obj.temperatureF) {
            return 0;

        } else if (this.temperatureF < obj.temperatureF) {
            return -1;
            
        } else {
            // else if (this.balance > anotherAccount.balance)
            return 1;
        }
    }
}
```


`-1` is an example and it can be any another negative number as well.

`1` is an example and it can be any another positive number as well.

But `0` is for equality.
