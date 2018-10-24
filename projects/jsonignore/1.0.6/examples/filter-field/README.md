# Field filtration
Lorem ipsum

In this examples used additional sample [classes](../example-classes/README.md)

## Example of Rest service without filtration module
In the next example you can see typical implementation of Spring Rest service
We defined signIn method, which requires email and password input params, and response User object

```java
@RestController
public class SessionService {
    @Autowired
    private UserController userController;   
    
    @RequestMapping(value = "/users/signIn",
            params = {"email", "password"}, method = RequestMethod.POST,
            consumes = {MediaType.APPLICATION_FORM_URLENCODED_VALUE},
            produces = {MediaType.APPLICATION_JSON_VALUE})            
    public User signIn(@RequestParam("email") String email, @RequestParam("password") String password) {
        return userController.signInUser(email, password);
    }
}
```


* Service JSON response

```json
{
  "id": 10,
  "email": "janedoe@gmail.com", 
  "fullName": "Jane Doe",
  "password": "12345",
  "secretKey": "54321",
  "address": {
    "id": 15,
    "apartmentNumber": 22,
    "street": {
      "id": 155,
      "streetName": "Bourbon Street",
      "streetNumber": 15
    }
  }
}
```

As you can see some fields could be excess and not required in response. 
It could be "id", "password", "secretKey" and so on.

Also we can use Spring @View annotation for exclude these fields
For more information please follow official Spring documentation [link](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/View.html)
But this solution is not flexible in situations when you need to send in response 
