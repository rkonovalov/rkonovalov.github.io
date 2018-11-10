---
layout: default
title: Json Ignore Documentation
description: Version 1.0.8
---

[Main](../../../index.MD) > [1.0.8](../../index.MD) >  [Examples](../index.MD) > Simple field filter

# Dynamic filter
Using this type of filters you can create your own filter with custom behaviour.
All dynamic filters should implement DynamicFilterEvent.class also filter should be 
annotated by DynamicFilterComponent annotation. 

Next example illustrates typical implementation

```java
    @DynamicFilterComponent
    public class DemoIdFilter implements DynamicFilterEvent {
        @Override
        public FilterFields onGetFilterFields(MethodParameter methodParameter, RequestSession request) {

            if(request.getSession().getAttribute("SOME_VALUE") != null) {
                return new FilterFields(User.class, Arrays.asList("id", "password", "email"));
            } else
                return new FilterFields();
        }
    }
```
In this example we created DemoIdFilter which attempts to find attribute *SOME_VALUE* in Http Session.
If attribute exists event *onGetFilterFields* returns configured FilterFields. As you can see, FilterFields contains filter for User.class
If Response Body of Spring Service contains object with class name User.class it will be filtered and fields "id", "password", "email" will
be removed from response.

Where
  * **DynamicFilterComponent** annotation indicates that DemoIdFilter is dynamic filter.
  Also it indicates that this class is Spring Component. 
  It means this class will be instantiated by Spring Bean Processor.
  Therefore you can autowire other components of your project.
  
  * Also filter implements **DynamicFilterEvent**. And you should to implement your logic in onGetFilterFields event.
  
  * **onGetFilterFields** should to return FilterFields object.

# Using dynamic filter
So, we created DemoIdFilter and let's see how we can use it. Next example illustrates typical usage of filter

```java
    @DynamicFilter(DemoIdFilter.class)
    @RequestMapping(value = "/users/signIn",
            params = {"email", "password"}, method = RequestMethod.POST,
            consumes = {MediaType.APPLICATION_FORM_URLENCODED_VALUE},
            produces = {MediaType.APPLICATION_JSON_VALUE})            
    public User signIn(@RequestParam("email") String email, @RequestParam("password") String password) {
        return userController.signInUser(email, password);
    }    
```

With this filter you get huge space of Service Response customization.