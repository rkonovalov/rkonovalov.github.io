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
But this solution is not flexible in situations when you need to send in response.
So you can use Json Ignore for fields filtration. 


## Filtration of fields on specified class
For fields filtration of specified class you need just add next annotation

```java
 @FieldFilterSetting(className = User.class, fields = {"id", "password", "secretKey"})
```

In next example we will try to filter fields: *"id", "password" and "secretKey"* in User class

* Example

```java

    @FieldFilterSetting(className = User.class, fields = {"id", "password", "secretKey"})
    @RequestMapping(value = "/users/signIn",
            params = {"email", "password"}, method = RequestMethod.POST,
            consumes = {MediaType.APPLICATION_FORM_URLENCODED_VALUE},
            produces = {MediaType.APPLICATION_JSON_VALUE})            
    public User signIn(@RequestParam("email") String email, @RequestParam("password") String password) {
        return userController.signInUser(email, password);
    }

```

* Service JSON response:
```json
{ 
  "email": "janedoe@gmail.com", 
  "fullName": "Jane Doe",
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

## Filtration of fields on class and subclasses
If you need to filter some fields in all classes and subclasses you need add FieldFilterSetting annotation without className

* Example

```java

    @FieldFilterSetting(fields = {"id"})
    @RequestMapping(value = "/users/signIn",
            params = {"email", "password"}, method = RequestMethod.POST,
            consumes = {MediaType.APPLICATION_FORM_URLENCODED_VALUE},
            produces = {MediaType.APPLICATION_JSON_VALUE})            
    public User signIn(@RequestParam("email") String email, @RequestParam("password") String password) {
        return userController.signInUser(email, password);
    }

```

* Service JSON response:
```json
{ 
  "email": "janedoe@gmail.com", 
  "fullName": "Jane Doe",
  "password": "12345",
  "secretKey": "54321",
  "address": {  
    "apartmentNumber": 22,
    "street": {    
      "streetName": "Bourbon Street",
      "streetNumber": 15
    }
  }
}
```

As you can see all fields with name *"id"* has been removed from service response

## Multiple field filtration
If you need to specify filtration of fields of each class separately you need to add and configure FieldFilterSetting annotation
for each required class. Let's look to the next example

* Example

```java
 @FieldFilterSetting(className = User.class, fields = {"id", "password", "secretKey"})
 @FieldFilterSetting(className = Address.class, fields = {"apartmentNumber"})
 @FieldFilterSetting(className = Street.class, fields = {"streetNumber"})
 ...
```

* Service JSON response

```json
{
  "email": "janedoe@gmail.com", 
  "fullName": "Jane Doe",
  "address": {
    "id": 15,
    "street": {
      "id": 155,
      "streetName": "Bourbon Street"
    }
  }
}
```

As you can see fields *"id", "password", "secretKey"* has been removed from User class, *"apartmentNumber"* from Address class 
and *"streetNumber"* field from Street class.

Thus, you can filtrate each response of service.