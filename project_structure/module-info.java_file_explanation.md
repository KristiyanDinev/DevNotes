# module-info.java file

To control the loading of packages by specifying which one to load.

Creates the `hello` module with `com.package` package.
```java
module hello {
    exports com.package;
}
```

Adds `hello` module to your project as class path.
```java
module hello2 {
    require hello;
}
```

## Project Structure

```
/project/com/package
/project/module-info.java
```
