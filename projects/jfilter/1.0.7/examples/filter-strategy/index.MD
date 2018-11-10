---
layout: default
title: JFilter Documentation
description: Version 1.0.7
---

[Main](../../../index.MD) > [1.0.7](../../index.MD) >  [Examples](../index.MD) > Session strategy filter

# Session strategy filter
Since you have read about simple field filtration [link](../filter-field/index.MD) we can continue.
During the development process you may encounter a situation when you need to send in service's response with 
some fields whose depends from HTTP Session parameters.

As example we have "signIn" method which responses User class which contains "password" field.
And we have two types users with privileges/roles called "USER" or "ADMIN". For users with role "ADMIN" we should to send "password" field,
for users with role "USER" should not. Take a look at the following examples.


* Example Service JSON response for users with role "ADMIN"

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

* Example Service JSON response for users with role "USER"

```json
{
  "id": 10,
  "email": "janedoe@gmail.com", 
  "fullName": "Jane Doe",
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

As you can see, this is two different responses which sends "signIn" method of the Service.
Let's take a closer look at issue solutions.

## Session strategy annotation
For using session strategy filtration wee need to add SessionStrategy annotation

```java
@SessionStrategy(attributeName = "ROLE", attributeValue = "USER", ignoreFields = {
            @FieldFilterSetting(className = User.class, fields = {"password"})
})
```
Where
  * **attributeName** - attribute name in Http Session attributes
  * **attributeValue** - attribute value of found attribute with attributeName
  
Session strategy algorithm will try to find in Http Session attribute with name "ROLE" and value equal "USER".
If a similar attribute is found, session strategy will apply configured FieldFilterSetting with "password" filtration.
Otherwise, field filtering will not be applied.  

* Example

```java
    ...
   @SessionStrategy(attributeName = "ROLE", attributeValue = "USER", ignoreFields = {
               @FieldFilterSetting(className = User.class, fields = {"password"})
  
    @RequestMapping(value = "/users/signIn",
             params = {"email", "password"}, method = RequestMethod.POST,
             consumes = {MediaType.APPLICATION_FORM_URLENCODED_VALUE},
             produces = {MediaType.APPLICATION_JSON_VALUE})            
     public User signIn(@RequestParam("email") String email, @RequestParam("password") String password) {
         return userController.signInUser(email, password);
     }
```

* Service JSON response

```json
{
  "id": 10,
  "email": "janedoe@gmail.com", 
  "fullName": "Jane Doe",
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

## Multiple Session strategy annotation
If you need add different strategies you just need add it as in the example

* Example

```java
    ...
    @SessionStrategy(attributeName = "ROLE", attributeValue = "USER", ignoreFields = {
            @FieldFilterSetting(className = User.class, fields = {"id", "password"})
    })   
    
    @SessionStrategy(attributeName = "ROLE", attributeValue = "ADMIN", ignoreFields = {
                @FieldFilterSetting(className = User.class, fields = {"id"})
    })   
  
    @RequestMapping(value = "/users/signIn",
            params = {"email", "password"}, method = RequestMethod.POST,
            consumes = {MediaType.APPLICATION_FORM_URLENCODED_VALUE},
            produces = {MediaType.APPLICATION_JSON_VALUE})            
    public User signIn(@RequestParam("email") String email, @RequestParam("password") String password) {
        return userController.signInUser(email, password);
    }
}
```

