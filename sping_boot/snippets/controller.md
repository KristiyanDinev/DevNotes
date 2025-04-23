# Controller Snippets

*Examples form my GitHub and/or AI and/or blogs and posts*

## Configuring MockMVC

```java
@SpringBootTest
@AutoConfigureMockMvc
public class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    ...

}
```

*Yeah. Actually we can autoconfigure it and the framework takes care of it.*

## For getting a JSON post/delete/put request in body

*Note: All of the properties of the model must match with the Json sent.*
*If you have done any configuration for the Json to the properties of that model*
*then they would also apply here. For example the `ObjectMapper` will be used to*
*Convert the Json in the body to the Java class you have given. So configuration*
*According to the Jackson library should be used in here if you plan on configuring.*

**Important: Use all arguments constructor and no arguments constructor and getters and setters.**
**Which can be found in the lombok library. Otherwise the mapping may not work.**

```maven
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
	<groupId>org.projectlombok</groupId>
	<artifactId>lombok</artifactId>
	<scope>provided</scope>
</dependency>
```

An example from "PhoneSystem" repository.
```java
@NoArgsConstructor
@AllArgsConstructor
@Data
@Getter
@Setter
@Builder
public class LoginUserModel { ... }
```

```java
@PostMapping("/login")
public ModelAndView login(@RequestBody LoginUserModel loginUserModel, ...) {}
```

## For MockMVC tests for that controller/endpoint

```java
LoginUserModel loginUserModel = new LoginUserModel("john@example.com", "123");

mockMvc.perform(post("/login")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(loginUserModel)))
.andDo(MockMvcResultHandlers.log())
.andExpect(status().isOk());
```


## For getting a form submission from urlencoding

*The same thing applies here for the properties as well.*
*Like the request has to match the properties accordingly,*
*even with the specified Jackson configuration.*

```java
@PostMapping(value = "/login", consumes = APPLICATION_FORM_URLENCODED_VALUE)
public ModelAndView login(@ModelAttribute LoginUserModel loginUserModel, ...) {}
```

## For MockMVC tests for that endpoint with form submission

```java
LoginUserModel loginUserModel = new LoginUserModel("john@example.com", "123");

mockMvc.perform(post("/login")
                .contentType(MediaType.APPLICATION_FORM_URLENCODED_VALUE)
                .param("email", loginUserModel.email)
                .param("password", loginUserModel.password))
.andDo(MockMvcResultHandlers.print())
.andExpect(status().isOk());
```

## Keeping the Cookie for the next requests in MockMVC

```java
MvcResult mvcResult = mockMvc.perform(
    get("/login")
).andDo(
    MockMvcResultHandlers.log()
).andExpect(
    status().isOk()
).andReturn();

Cookie sessionCookie = mvcResult.getResponse().getCookie("SESSION");

LoginUserModel loginUserModel = new LoginUserModel("john2@example.com", "123");

mockMvc.perform(post("/login")
                .contentType(MediaType.APPLICATION_FORM_URLENCODED_VALUE)
                .param("email", loginUserModel.email)
                .param("password", loginUserModel.password)
                .cookie(sessionCookie)
                .accept(MediaType.ALL))
.andDo(MockMvcResultHandlers.log())
.andExpect(status().isOk());
```

I think by default MockMvc would keep the cookie for future request in the same testcase `@Test`.

So that if the client gets `Set-Cookie`, then I think the `sessionCookie` may change as well, but

if the test doesn't work, then you can update the cookie by yourself.
