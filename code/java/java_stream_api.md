# Java Stream API

This is a better way of doing loops and collections, if you have them so nested.
Streams are like lists, but with different functions.

The operations which you use in your stream are only executed 
when you call a function that returns something different from `Stream<T>`.

## Parallel Stream

Executes the operations for the stream API in threads.

```java
List<String> names = List.of("John", "Peter", "Bob");

names = names.parallelStream()
    .filter(name -> name.length() == 3)
    .toList();
```

## Map

You can start working with this property throughout the whole stream.
Basically, it works with the result from the `map`.

```java
class Person {
    public String name;
    public int age;

    public Person() {}
}

List<Person> persons = List.of(new Person(), new Person(), new Person());

persons = persons.stream()
    .map(p -> p.age)
    .filter(age -> age > 10)
    .toList();
```

Or you can just extract all the values from the properties

```java
class Person {
    public String name;
    public int age;

    public Person() {}
}

List<Person> persons = List.of(new Person(), new Person(), new Person());

List<Integer> ages = persons.stream()
    .map(p -> p.age)
    .toList();
```

Or this:
```java
class Person {
    public String name;
    public int age;

    public Person() {}
}

List<Person> persons = List.of(new Person(), new Person(), new Person());

List<Integer> ages = persons.stream()
    .map(p -> "Name: "+p.name)
    .toList();
```

## FlatMap

You can work with more than one property at a time while each property will have its own element in the result list.

Good way to remember it: **Flattens out the result**

```java
class Person {
    public String name;
    public int age;
    public int date;

    public Person() {}
}

List<Person> persons = List.of(new Person(), new Person(), new Person());

// age, date, age, date, etc.
List<Integer> ages_and_dates = persons.stream()
    .flatMap(p -> Stream.of(p.age, p.date))
    .toList();
```

## Filter

Acts as an if statement.
Takes a lambda `Predicate<? super T>` as an argument with one parameter which is the object in the stream.

Returns the filtered stream based on the applied filter.

`names` will have only one element: "Bob"
```java
List<String> names = List.of("John", "Peter", "Bob");

names = names.stream()
    .filter(name -> name.length() == 3)
    .toList();
```

or 

`names` will have only one element: "John"
```java
List<String> names = List.of("John", "Peter", "Bob");

names = names.stream()
    .filter(name -> {
        return name.equals("John");
    })
    .toList();
```

## Collect

Uses a `Collector` to collect all the elements to a more complex return type.

Like `groupingBy`, `toMap`, `toList` etc.

## Max

Returns the Optional for that Type. It compares each value to get the maximum of them all.

```java
List<String> names = List.of("John", "Peter", "Bob");

Optional<String> maxLength = names.stream().max(
    new Comparator<String>() {

        @Override
        public int compare(String name1, String name2) {
            return Integer.compare(name1.length(), name2.length());
        }
    });
```

## Min

Returns the Optional for that Type. It compares each value to get the minimal of them all.

```java
List<String> names = List.of("John", "Peter", "Bob");

Optional<String> minLength = names.stream().min(
    new Comparator<String>() {

        @Override
        public int compare(String name1, String name2) {
            return Integer.compare(name1.length(), name2.length());
        }
    });
```


## AnyMatch

Returns a `boolean` value.

`true` if there is any matches in the stream for that condition.
`false` if there is no matches for any elements in the stream.

```java
List<String> names = List.of("John", "Peter", "Bob");

// returns true, because of the element "Bob"
boolean anyMatch = names.stream()
    .anyMatch(name -> name.length() == 3);
```

## AllMatch

Returns a `boolean` value.

Basically the opposite of `noneMatch`.

`true` if all of the elements match that condition.
`false` if only one element does not match the condition.

```java
List<String> names = List.of("John", "Peter", "Bob");

// returns true, because of the element "Bob"
boolean allMatch = names.stream()
    .allMatch(name -> name.length() == 3);
```

## NoneMatch

Returns a `boolean` value.

Basically the opposite of `allMatch`.

`true` if none of the elements match the condition.
`false` if only one element matches the condition.

```java
List<String> names = List.of("John", "Peter", "Bob");

// returns true, because of the element "Bob"
boolean noneMatch = names.stream()
    .noneMatch(name -> name.length() == 3);
```

## Count

Counts the amount of elements in the stream. Returns `long`

```java
List<String> names = List.of("John", "Peter", "Bob");

long size = names.stream().count();
```

Almost the same:

```java
int size = List.of("John", "Peter", "Bob").size();
```

## Distinct

Removes the duplicates in the stream.


```java
List<String> names = List.of("John", "John", "Peter", "Bob");

names = names.stream().distinct().toList();
```

## Sorted

It just sorts the elements in the stream with default prams.

```java
List<String> names = List.of("John", "Peter", "Bob");

names = names.stream().sorted().toList();
```

Or with comparator:

A comparator uses *0, positive and negative numbers* for comparing.

```java
List<String> names = List.of("John", "Peter", "Bob");

names = names.stream().sorted(
    new Comparator<String>() {

        @Override
        public int compare(String name1, String name2) {
            return name1.compareTo(name2);
        }

    }).toList();
```

## Limit

Returns the first `n` elements of the stream.

`n` is **int**.

```java
List<String> names = List.of("John", "Peter", "Bob");

names = names.stream().limit(2).toList();
```

