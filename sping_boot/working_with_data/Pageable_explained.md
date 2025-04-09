# Pageable (interface from SpringBoot)



The simplest way to create sorted pages.
```java
Pageable pageable = PageRequest.of(sizePerPage, pageNumber, Sort.by(sortRepresentedByString));
```

The simplest way to create unsorted pages.
```java
Pageable pageable = PageRequest.of(sizePerPage, pageNumber);
```