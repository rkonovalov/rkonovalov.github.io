---
layout: default
title: JFilter Documentation
description: Version 1.0.18
menu_title: Whole Spring Controller filtration
---

[Main](../../../index.MD) > [1.0.18](../../index.MD) >  [Examples](../index.MD) > Whole Spring Controller filtration

# Whole Spring REST Web Service controller filtration
If you don't want to add filter annotations to each Service response, you can add annotation on whole Service
Let's look at the next example

```java
@FileFilterSetting(fileName = "filter_configuration.xml")

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

In this example we specified FileFilterSetting annotation and configuration of "filter_configuration.xml" file will be applied on whole Service

* List of valid values: 
  * [FieldFilterSetting](../filter-field/index.MD)
  * [SessionStrategy](../filter-strategy/index.MD) 
  * [FileFilterSetting](../filter-file/index.MD)



