# JDK (Java Development Kit)

It has all java development tools, such as:

- `javac` - Java compiler - compiles **.java** to **.class** files.
- `javadoc` - Generates HTML documentation from Java source code comments.
- `javap` - Class file disassembler - view bytecode of compiled classes.
- `jdb` - Java debugger.
- `jar` - Package Java classes and resources into **JAR** files.
- `jarsigner` - Sign and verify **JAR** files.
- `jlink` - Create a custom runtime image with only required modules *(since ***Java 9***, modular)*.
- `jdeps` - Dependency analysis tool - shows package/class dependencies.
- `jhsdb` - HotSpot Debugger for heap and core dump analysis.
- `jmap` - Memory map tool - heap dump, histogram, etc.
- `jstack` - Stack trace of threads in a **JVM**.
- `jstat` - **JVM** statistics monitoring tool.
- `jstatd` - Daemon for `jstat` for remote monitoring.
- `jconsole` - GUI for monitoring **JVM** performance *(Java Management Extensions, JMX)*.
- `jmc` - Java Mission Control - advanced profiling & diagnostics *(included in Oracle JDK 7u40+)*.
- `jshell` - Interactive Java REPL *(Read-Eval-Print Loop)* introduced in **Java 9**.
- `serialver` - Return **serialVersionUID** for serializable classes.

These are not part of a standalone **JRE**. You only get them with the **JDK**.

The entire **JRE** is included in **JDK** to run the application.

>*Note: You can't install standalone ***JRE*** from ***Java 11*** included and upwards. ***Java 8*** That is why it is so loved, because it has its own standalone ***JRE***, which eliminates the need of extra ***JDK*** tooling, just to run a simple java application and it has long term support.*

# JRE (Java Runtime Environment)

This includes libraries and classes to load the application.

>*Note: You can find the `rt.jar` in the installation folder of ***JRE*** under `lib/` folder (depending on the OS). ONLY IF YOU HAVE LOWER THAN ***JAVA 11*** VERSION, because `rt.jar` is separated ***JRE***.*

The entire **JVM** is included in **JRE** to run the application.

# JVM (Java Virtual Machine)

The actual environment where the java program will execute the **bytecode**.

*See: https://www.geeksforgeeks.org/differences-jdk-jre-jvm/*
