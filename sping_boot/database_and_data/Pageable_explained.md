# Pageable (interface from SpringBoot)

Pageable is an interface and there are classes which represent Pageable in different ways.

- PageRequest
- AbstractPageRequest
- QPageRequest
- Unpaged

For example the class `PageRequest` is a simple class which handles pages and their content very reasonable. But the rest may be used for some specific cases in your application.

The JSON output of a Pageable looks something like this.

Example:

```json
{
  "content": [
    {
      "id": 1,
      "firstName": "Bruno",
      "lastName": "Affeldt"
    },
    {
      "id": 2,
      "firstName": "Cassia",
      "lastName": "Cunha"
    },
    {
      "id": 4,
      "firstName": "Gregorio",
      "lastName": "Oliveira"
    }
  ],
  "pageable": {
    "pageNumber": 0,
    "pageSize": 3,
    "sort": {
      "empty": false,
      "sorted": true,
      "unsorted": false
    },
    "offset": 0,
    "unpaged": false,
    "paged": true
  },
  "last": false,
  "totalPages": 3,
  "totalElements": 7,
  "size": 3,
  "number": 0,
  "sort": {
    "empty": false,
    "sorted": true,
    "unsorted": false
  },
  "numberOfElements": 3,
  "first": true,
  "empty": false
}
```

By default Spring Boot should use Pageable, but you may run into so issues at first.

## Snippets

Here is a snippet for controller that uses Pageable.

Example:

```java
@RestController
@EnableWebMvc
public class UserController {

    @GetMapping("/")
    public ModelAndView index(HttpSession session, Pageable pageable) {
        ...
    }

    ...
}
```
- `@RestController` and `@Controller` are almost the same, but `@RestController` uses `@Controller` and `@ResponseBody` while `@Controller` doesn't use `@ResponseBody`. The `@Controller` uses `@Component` which means `@Controller` and `@RestController` will be a bean/component.


The simplest way to create sorted pages.

Example:

```java
Pageable pageable = PageRequest.of(sizePerPage, pageNumber, Sort.by(sortRepresentedByString));
```

The simplest way to create unsorted pages.

Example:

```java
Pageable pageable = PageRequest.of(sizePerPage, pageNumber);
```
