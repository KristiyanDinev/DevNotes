# JSON vs FormData: Complete Guide for Spring Boot APIs

## Table of Contents
1. [Overview](#overview)
2. [JSON Requests](#json-requests)
3. [FormData Requests](#formdata-requests)
4. [Key Differences](#key-differences)
5. [When to Use Each](#when-to-use-each)
6. [Complete Examples](#complete-examples)
7. [Best Practices](#best-practices)
8. [Common Pitfalls](#common-pitfalls)

## Overview

When building web applications with Spring Boot, you have two primary ways to send data from the frontend to your backend API:

- **JSON (JavaScript Object Notation)**: Structured data format, ideal for REST APIs
- **FormData**: Mimics HTML form submission, great for file uploads and simple forms

Understanding when and how to use each is crucial for building robust web applications.

## JSON Requests

### What is JSON?
JSON is a lightweight, text-based data interchange format. It's language-independent and easy for humans to read and write.

### Content Type
```
Content-Type: application/json
```

### Backend Implementation (Spring Boot)

#### 1. Simple JSON Endpoint
```java
@RestController
@RequestMapping("/api")
public class UserController {
    
    @PostMapping("/user")
    public ResponseEntity<UserResponse> createUser(@RequestBody UserRequest request) {
        // Process the user request
        User user = userService.createUser(request);
        UserResponse response = new UserResponse(user.getId(), "User created successfully");
        return ResponseEntity.ok(response);
    }
}
```

#### 2. Request/Response DTOs
```java
// Request DTO
public class UserRequest {
    @NotBlank(message = "Name is required")
    private String name;
    
    @Email(message = "Invalid email format")
    private String email;
    
    @Min(value = 18, message = "Age must be at least 18")
    private int age;
    
    // Constructors, getters, setters
    public UserRequest() {}
    
    public UserRequest(String name, String email, int age) {
        this.name = name;
        this.email = email;
        this.age = age;
    }
    
    // Getters and Setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
    
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}

// Response DTO
public class UserResponse {
    private Long id;
    private String message;
    private LocalDateTime timestamp;
    
    public UserResponse(Long id, String message) {
        this.id = id;
        this.message = message;
        this.timestamp = LocalDateTime.now();
    }
    
    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    
    public String getMessage() { return message; }
    public void setMessage(String message) { this.message = message; }
    
    public LocalDateTime getTimestamp() { return timestamp; }
    public void setTimestamp(LocalDateTime timestamp) { this.timestamp = timestamp; }
}
```

#### 3. Complex JSON with Nested Objects
```java
@PostMapping("/order")
public ResponseEntity<OrderResponse> createOrder(@RequestBody OrderRequest request) {
    Order order = orderService.createOrder(request);
    return ResponseEntity.ok(new OrderResponse(order.getId(), "Order created"));
}

// Complex Request DTO
public class OrderRequest {
    @NotBlank
    private String customerId;
    
    @NotEmpty(message = "Order must have at least one item")
    @Valid
    private List<OrderItemRequest> items;
    
    @Valid
    private AddressRequest shippingAddress;
    
    // Getters and Setters...
}

public class OrderItemRequest {
    @NotBlank
    private String productId;
    
    @Min(value = 1, message = "Quantity must be at least 1")
    private int quantity;
    
    @DecimalMin(value = "0.01", message = "Price must be positive")
    private BigDecimal price;
    
    // Getters and Setters...
}
```

### Frontend Implementation (HTML + JavaScript)

#### 1. Simple JSON Request
```html
<!DOCTYPE html>
<html>
<head>
    <title>JSON Example</title>
</head>
<body>
    <form id="userForm">
        <input type="text" id="name" placeholder="Name" required>
        <input type="email" id="email" placeholder="Email" required>
        <input type="number" id="age" placeholder="Age" required>
        <button type="submit">Create User</button>
    </form>
    
    <div id="result"></div>
    
    <script>
        document.getElementById('userForm').addEventListener('submit', async function(e) {
            e.preventDefault();
            
            const userData = {
                name: document.getElementById('name').value,
                email: document.getElementById('email').value,
                age: parseInt(document.getElementById('age').value)
            };
            
            try {
                const response = await fetch('/api/user', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(userData)
                });
                
                const result = await response.json();
                
                if (response.ok) {
                    document.getElementById('result').innerHTML = 
                        `<p style="color: green;">Success: ${result.message}</p>`;
                } else {
                    document.getElementById('result').innerHTML = 
                        `<p style="color: red;">Error: ${result.message}</p>`;
                }
            } catch (error) {
                document.getElementById('result').innerHTML = 
                    `<p style="color: red;">Network error: ${error.message}</p>`;
            }
        });
    </script>
</body>
</html>
```

#### 2. Complex JSON with Nested Data
```javascript
// Complex order creation
const orderData = {
    customerId: "customer123",
    items: [
        {
            productId: "prod1",
            quantity: 2,
            price: 29.99
        },
        {
            productId: "prod2",
            quantity: 1,
            price: 15.50
        }
    ],
    shippingAddress: {
        street: "123 Main St",
        city: "Anytown",
        state: "CA",
        zipCode: "12345"
    }
};

fetch('/api/order', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify(orderData)
});
```

## FormData Requests

### What is FormData?
FormData represents form fields and their values, which can be sent using `multipart/form-data` encoding. It's especially useful for file uploads.

### Content Type
```
Content-Type: multipart/form-data
```
*Note: When using FormData, don't set Content-Type manually - the browser sets it automatically with the boundary.*

### Backend Implementation (Spring Boot)

#### 1. Simple FormData Endpoint
```java
@RestController
@RequestMapping("/api")
public class FormController {
    
    @PostMapping("/user-form")
    public ResponseEntity<String> createUserForm(
            @RequestParam("name") String name,
            @RequestParam("email") String email,
            @RequestParam("age") int age) {
        
        // Process form data
        User user = new User(name, email, age);
        userService.save(user);
        
        return ResponseEntity.ok("User created successfully");
    }
}
```

#### 2. File Upload with FormData
```java
@PostMapping("/upload")
public ResponseEntity<UploadResponse> uploadFile(
        @RequestParam("file") MultipartFile file,
        @RequestParam("description") String description,
        @RequestParam("category") String category) {
    
    if (file.isEmpty()) {
        return ResponseEntity.badRequest()
            .body(new UploadResponse(false, "No file selected"));
    }
    
    try {
        // Save file
        String filename = fileService.saveFile(file);
        
        // Save metadata
        FileMetadata metadata = new FileMetadata(filename, description, category);
        fileService.saveMetadata(metadata);
        
        return ResponseEntity.ok(
            new UploadResponse(true, "File uploaded successfully", filename)
        );
    } catch (IOException e) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
            .body(new UploadResponse(false, "File upload failed"));
    }
}

// Response DTO
public class UploadResponse {
    private boolean success;
    private String message;
    private String filename;
    
    public UploadResponse(boolean success, String message) {
        this.success = success;
        this.message = message;
    }
    
    public UploadResponse(boolean success, String message, String filename) {
        this.success = success;
        this.message = message;
        this.filename = filename;
    }
    
    // Getters and Setters...
}
```

#### 3. Mixed Data Types
```java
@PostMapping("/profile")
public ResponseEntity<String> updateProfile(
        @RequestParam("userId") Long userId,
        @RequestParam("name") String name,
        @RequestParam("email") String email,
        @RequestParam(value = "avatar", required = false) MultipartFile avatar,
        @RequestParam(value = "preferences", required = false) String preferences) {
    
    // Update user profile
    User user = userService.findById(userId);
    user.setName(name);
    user.setEmail(email);
    
    // Handle file upload
    if (avatar != null && !avatar.isEmpty()) {
        String avatarPath = fileService.saveAvatar(avatar, userId);
        user.setAvatarPath(avatarPath);
    }
    
    // Handle JSON preferences (sent as string)
    if (preferences != null) {
        try {
            ObjectMapper mapper = new ObjectMapper();
            UserPreferences prefs = mapper.readValue(preferences, UserPreferences.class);
            user.setPreferences(prefs);
        } catch (JsonProcessingException e) {
            return ResponseEntity.badRequest().body("Invalid preferences format");
        }
    }
    
    userService.save(user);
    return ResponseEntity.ok("Profile updated successfully");
}
```

### Frontend Implementation (HTML + JavaScript)

#### 1. Simple FormData Request
```html
<!DOCTYPE html>
<html>
<head>
    <title>FormData Example</title>
</head>
<body>
    <form id="userFormData">
        <input type="text" id="name" placeholder="Name" required>
        <input type="email" id="email" placeholder="Email" required>
        <input type="number" id="age" placeholder="Age" required>
        <button type="submit">Create User</button>
    </form>
    
    <div id="result"></div>
    
    <script>
        document.getElementById('userFormData').addEventListener('submit', async function(e) {
            e.preventDefault();
            
            const formData = new FormData();
            formData.append('name', document.getElementById('name').value);
            formData.append('email', document.getElementById('email').value);
            formData.append('age', document.getElementById('age').value);
            
            try {
                const response = await fetch('/api/user-form', {
                    method: 'POST',
                    body: formData  // Don't set Content-Type header
                });
                
                const result = await response.text();
                
                if (response.ok) {
                    document.getElementById('result').innerHTML = 
                        `<p style="color: green;">${result}</p>`;
                } else {
                    document.getElementById('result').innerHTML = 
                        `<p style="color: red;">Error: ${result}</p>`;
                }
            } catch (error) {
                document.getElementById('result').innerHTML = 
                    `<p style="color: red;">Network error: ${error.message}</p>`;
            }
        });
    </script>
</body>
</html>
```

#### 2. File Upload with FormData
```html
<!DOCTYPE html>
<html>
<head>
    <title>File Upload Example</title>
</head>
<body>
    <form id="uploadForm">
        <input type="file" id="fileInput" accept=".jpg,.png,.pdf" required>
        <input type="text" id="description" placeholder="Description" required>
        <select id="category">
            <option value="document">Document</option>
            <option value="image">Image</option>
            <option value="other">Other</option>
        </select>
        <button type="submit">Upload File</button>
    </form>
    
    <div id="uploadResult"></div>
    
    <script>
        document.getElementById('uploadForm').addEventListener('submit', async function(e) {
            e.preventDefault();
            
            const formData = new FormData();
            const fileInput = document.getElementById('fileInput');
            
            formData.append('file', fileInput.files[0]);
            formData.append('description', document.getElementById('description').value);
            formData.append('category', document.getElementById('category').value);
            
            try {
                const response = await fetch('/api/upload', {
                    method: 'POST',
                    body: formData
                });
                
                const result = await response.json();
                
                if (result.success) {
                    document.getElementById('uploadResult').innerHTML = 
                        `<p style="color: green;">${result.message}</p>`;
                } else {
                    document.getElementById('uploadResult').innerHTML = 
                        `<p style="color: red;">${result.message}</p>`;
                }
            } catch (error) {
                document.getElementById('uploadResult').innerHTML = 
                    `<p style="color: red;">Upload failed: ${error.message}</p>`;
            }
        });
    </script>
</body>
</html>
```

#### 3. Mixed Data Types (FormData + JSON)
```javascript
// Profile update with file and JSON data
const profileData = new FormData();
profileData.append('userId', '123');
profileData.append('name', 'John Doe');
profileData.append('email', 'john@example.com');

// Add file if selected
const avatarFile = document.getElementById('avatar').files[0];
if (avatarFile) {
    profileData.append('avatar', avatarFile);
}

// Add JSON as string
const preferences = {
    theme: 'dark',
    notifications: true,
    language: 'en'
};
profileData.append('preferences', JSON.stringify(preferences));

fetch('/api/profile', {
    method: 'POST',
    body: profileData
});
```

## Key Differences

### Data Structure
| Aspect | JSON | FormData |
|--------|------|----------|
| **Structure** | Hierarchical, nested objects | Flat key-value pairs |
| **Data Types** | String, Number, Boolean, Object, Array, null | String, File, Blob |
| **Nesting** | Native support | Requires serialization |

### Content Type
| Type | Content-Type Header | Set By |
|------|-------------------|---------|
| **JSON** | `application/json` | Developer (manual) |
| **FormData** | `multipart/form-data; boundary=...` | Browser (automatic) |

### Backend Handling
| Aspect | JSON | FormData |
|--------|------|----------|
| **Spring Annotation** | `@RequestBody` | `@RequestParam` |
| **Validation** | Bean validation annotations | Parameter validation |
| **File Upload** | Not supported | Native support |
| **Type Safety** | Strong (DTOs) | Weak (strings) |

### Frontend Usage
| Aspect | JSON | FormData |
|--------|------|----------|
| **Creation** | `JSON.stringify(object)` | `new FormData()` |
| **Data Addition** | Object properties | `formData.append()` |
| **File Support** | No | Yes |
| **Browser Support** | Universal | Universal |

## When to Use Each

### Use JSON When:
- ✅ Building REST APIs
- ✅ Working with complex, nested data structures
- ✅ Need strong type safety and validation
- ✅ Integrating with frontend frameworks (React, Vue, Angular)
- ✅ Building mobile app APIs
- ✅ Want consistent API design
- ✅ No file uploads required

### Use FormData When:
- ✅ Uploading files
- ✅ Working with traditional HTML forms
- ✅ Simple key-value data
- ✅ Mixed content (text + files)
- ✅ Legacy system integration
- ✅ Progressive enhancement approach

## Complete Examples

### Example 1: User Registration (JSON)
```java
// Backend
@PostMapping("/api/register")
public ResponseEntity<RegistrationResponse> register(
        @Valid @RequestBody RegistrationRequest request) {
    
    // Validate email uniqueness
    if (userService.existsByEmail(request.getEmail())) {
        return ResponseEntity.badRequest()
            .body(new RegistrationResponse(false, "Email already exists"));
    }
    
    // Create user
    User user = new User(request.getName(), request.getEmail(), request.getPassword());
    userService.save(user);
    
    return ResponseEntity.ok(
        new RegistrationResponse(true, "Registration successful")
    );
}
```

```javascript
// Frontend
const registrationData = {
    name: 'John Doe',
    email: 'john@example.com',
    password: 'securePassword123',
    confirmPassword: 'securePassword123',
    acceptTerms: true
};

const response = await fetch('/api/register', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify(registrationData)
});
```

### Example 2: Document Upload (FormData)
```java
// Backend
@PostMapping("/api/documents")
public ResponseEntity<DocumentResponse> uploadDocument(
        @RequestParam("document") MultipartFile file,
        @RequestParam("title") String title,
        @RequestParam("category") String category,
        @RequestParam("isPublic") boolean isPublic) {
    
    // Validate file
    if (file.isEmpty()) {
        return ResponseEntity.badRequest()
            .body(new DocumentResponse(false, "No file provided"));
    }
    
    // Save document
    String filename = documentService.saveDocument(file, title, category, isPublic);
    
    return ResponseEntity.ok(
        new DocumentResponse(true, "Document uploaded successfully", filename)
    );
}
```

```javascript
// Frontend
const formData = new FormData();
formData.append('document', fileInput.files[0]);
formData.append('title', 'Annual Report');
formData.append('category', 'financial');
formData.append('isPublic', 'false');

const response = await fetch('/api/documents', {
    method: 'POST',
    body: formData
});
```

## Best Practices

### JSON Best Practices
1. **Use DTOs for type safety**
   ```java
   @PostMapping("/api/user")
   public ResponseEntity<UserResponse> createUser(@Valid @RequestBody UserRequest request) {
       // Type-safe access to request.getName(), request.getEmail(), etc.
   }
   ```

2. **Implement proper validation**
   ```java
   public class UserRequest {
       @NotBlank(message = "Name is required")
       @Size(min = 2, max = 50, message = "Name must be between 2 and 50 characters")
       private String name;
       
       @Email(message = "Please provide a valid email")
       private String email;
   }
   ```

3. **Handle errors gracefully**
   ```java
   @ExceptionHandler(MethodArgumentNotValidException.class)
   public ResponseEntity<ErrorResponse> handleValidationExceptions(
           MethodArgumentNotValidException ex) {
       Map<String, String> errors = new HashMap<>();
       ex.getBindingResult().getAllErrors().forEach((error) -> {
           String fieldName = ((FieldError) error).getField();
           String errorMessage = error.getDefaultMessage();
           errors.put(fieldName, errorMessage);
       });
       return ResponseEntity.badRequest().body(new ErrorResponse("Validation failed", errors));
   }
   ```

### FormData Best Practices
1. **Validate file uploads**
   ```java
   @PostMapping("/api/upload")
   public ResponseEntity<String> uploadFile(@RequestParam("file") MultipartFile file) {
       // Check file size
       if (file.getSize() > 10 * 1024 * 1024) { // 10MB
           return ResponseEntity.badRequest().body("File too large");
       }
       
       // Check file type
       String contentType = file.getContentType();
       if (!Arrays.asList("image/jpeg", "image/png", "application/pdf").contains(contentType)) {
           return ResponseEntity.badRequest().body("Invalid file type");
       }
   }
   ```

2. **Configure multipart settings**
   ```properties
   # application.properties
   spring.servlet.multipart.max-file-size=10MB
   spring.servlet.multipart.max-request-size=10MB
   ```

3. **Handle file storage securely**
   ```java
   @Service
   public class FileService {
       public String saveFile(MultipartFile file) throws IOException {
           // Generate secure filename
           String filename = UUID.randomUUID().toString() + "_" + file.getOriginalFilename();
           
           // Save to secure location
           Path uploadPath = Paths.get("uploads").resolve(filename);
           Files.copy(file.getInputStream(), uploadPath, StandardCopyOption.REPLACE_EXISTING);
           
           return filename;
       }
   }
   ```

## Common Pitfalls

### JSON Pitfalls
1. **Forgetting Content-Type header**
   ```javascript
   // ❌ Wrong - missing Content-Type
   fetch('/api/user', {
       method: 'POST',
       body: JSON.stringify(userData)
   });
   
   // ✅ Correct
   fetch('/api/user', {
       method: 'POST',
       headers: {
           'Content-Type': 'application/json',
       },
       body: JSON.stringify(userData)
   });
   ```

2. **Using wrong Spring annotation**
   ```java
   // ❌ Wrong - @RequestParam for JSON
   @PostMapping("/api/user")
   public ResponseEntity<String> createUser(@RequestParam String name) {
       // Won't work with JSON
   }
   
   // ✅ Correct - @RequestBody for JSON
   @PostMapping("/api/user")
   public ResponseEntity<String> createUser(@RequestBody UserRequest request) {
       // Works with JSON
   }
   ```

### FormData Pitfalls
1. **Setting Content-Type manually**
   ```javascript
   // ❌ Wrong - don't set Content-Type for FormData
   fetch('/api/upload', {
       method: 'POST',
       headers: {
           'Content-Type': 'multipart/form-data',  // Don't do this!
       },
       body: formData
   });
   
   // ✅ Correct - let browser set Content-Type
   fetch('/api/upload', {
       method: 'POST',
       body: formData
   });
   ```

2. **Using @RequestBody for FormData**
   ```java
   // ❌ Wrong - @RequestBody doesn't work with FormData
   @PostMapping("/api/upload")
   public ResponseEntity<String> upload(@RequestBody MultipartFile file) {
       // Won't work
   }
   
   // ✅ Correct - use @RequestParam
   @PostMapping("/api/upload")
   public ResponseEntity<String> upload(@RequestParam("file") MultipartFile file) {
       // Works correctly
   }
   ```

3. **Forgetting to handle empty files**
   ```java
   // ❌ Dangerous - doesn't check for empty files
   @PostMapping("/api/upload")
   public ResponseEntity<String> upload(@RequestParam("file") MultipartFile file) {
       String filename = file.getOriginalFilename();  // Could be null
       // Process file without validation
   }
   
   // ✅ Safe - proper validation
   @PostMapping("/api/upload")
   public ResponseEntity<String> upload(@RequestParam("file") MultipartFile file) {
       if (file.isEmpty()) {
           return ResponseEntity.badRequest().body("No file provided");
       }
       // Safe to process
   }
   ```

## Conclusion

Both JSON and FormData have their place in modern web development:

- **Choose JSON** for structured data, REST APIs, and type-safe operations
- **Choose FormData** for file uploads, simple forms, and mixed content types

Understanding these differences will help you build more robust and maintainable Spring Boot applications. The key is to match the right tool to the right use case and follow established best practices for each approach.