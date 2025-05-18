# package-info.java file

`package-info.java` is new in **JDK 5.0**, and is preferred over `package.html`.

It is a way to generate a **Javadoc** for this package.

Example:
```java
/**
 * This module is about impact of the final keyword on performance
 * <p>
 * This module explores if there are any performance benefits from
 * using the final keyword in our code. This module examines the performance
 * implications of using final on a variable, method, and class level.
 * </p>
 *
 * @since 1.0
 * @author baeldung
 * @version 1.1
 */
@NonNullApi
@NonNullFields
package com.baeldung.nullibility;

import org.springframework.lang.NonNullApi;
import org.springframework.lang.NonNullFields;
```

Here in the `/* */` comments you can write out the **Javadoc**.

You can continue with **annotations** that apply to the classes in that single package.

The imports are for the **annotations'** sake.

- **@NonNullApi** - Requires not null values for **parameters** and **return values**.

- **@NonNullFields** - Requires not null values for **fields** in a java class.

- **@since** - In what version (when?) did this feature came to the project.

- **@author** - Who created the package/project.

- **@version** - Current version of that **Javadoc**.

*See https://www.baeldung.com/java-package-info*
