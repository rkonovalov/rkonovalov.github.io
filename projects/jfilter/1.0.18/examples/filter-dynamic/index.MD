---
layout: default
title: JFilter Documentation
description: Version 1.0.18
menu_title: Dynamic filter
---

[Main](../../../index.MD) > [1.0.18](../../index.MD) >  [Examples](../index.MD) > Simple field filter

# Dynamic filter
Using this type of filters you can create your own filter with custom behaviour.
All dynamic filters should implement DynamicFilterEvent.class also filter should be 
annotated by DynamicFilterComponent annotation. 

Next example illustrates typical implementation

```java
    @DynamicFilterComponent
    public class DemoIdFilter implements DynamicFilterEvent {
         @Override
            public void onRequest(Comparator<RequestSession, FilterFields> comparator) {    
            //If request has SOME_KEY in request params then try to filter fields "id", "password", "email" from object
            comparator.compare((request -> request.getRequest().getParameterMap().containsKey("SOME_KEY")), 
                            (result -> FilterFields.getFieldsBy(Arrays.asList("id", "password", "email"))));
          
        }
    }
```
In this example we created DemoIdFilter which attempts to find attribute *SOME_KEY* in Http request params.
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

# Multiple conditions filter example
In this example you can see 3 different conditions

```java
    @DynamicFilterComponent
    public class DemoIdFilter implements DynamicFilterEvent {
         @Override
            public void onRequest(Comparator<RequestSession, FilterFields> comparator) {    
            
            //If request has SOME_KEY in request params then try to filter fields "id", "password", "email" from object
            comparator.compare((request -> request.getRequest().getParameterMap().containsKey("SOME_KEY")), 
                            (result -> FilterFields.getFieldsBy(Arrays.asList("id", "password", "email"))))
                            
            //If request has SOME_KEY2 in request params then try to filter field "password" from object
                      .compare((request -> request.getRequest().getParameterMap().containsKey("SOME_KEY2")), 
                            (result -> FilterFields.getFieldsBy(Arrays.asList("password"))))
                            
            //If session has SOME_KEY3 in session params then try to filter field "email" from object
                      .compare((request -> request.getSession().getAttribute(SOME_KEY3)), 
                             (result -> FilterFields.getFieldsBy(Arrays.asList("email"))));
          
        }
    }
```
* For additional information about using **Comparator** please follow next link: https://github.com/rkonovalov/comparator

# Passing filter fields through controller
If you need to pass filter fields in REST Web Service controller, you can do it by using **DynamicSessionFilter** and **ATTRIBUTE_FILTER_FIELDS** session attribute.
```java
    @DynamicFilter(DynamicSessionFilter.class)
    @RequestMapping(value = "/signInUser",
            params = {"email", "password"}, 
			method = RequestMethod.POST,
            consumes = {MediaType.APPLICATION_FORM_URLENCODED_VALUE},
            produces = {MediaType.APPLICATION_JSON_VALUE})
    public User signInUser(HttpSession session,  @RequestParam("email") String email, @RequestParam("password") String password) {
        
	//Set configured FilterFields into session's ATTRIBUTE_FILTER_FIELDS attribute		
	FilterUtil.useFilter(session, FilterFields.getFieldsBy(Arrays.asList("id", "password", "email")));
		
	//DynamicSessionFilter will check ATTRIBUTE_FILTER_FIELDS in session attributes and if attribute has FilterFields
	//it will filter current response
        return databaseService.getUser(email, password);
    }
```

Also instead using **session** you can use **request** for passing filter

```java
    @DynamicFilter(DynamicSessionFilter.class)
    @RequestMapping(value = "/signInUser",
            params = {"email", "password"}, 
			method = RequestMethod.POST,
            consumes = {MediaType.APPLICATION_FORM_URLENCODED_VALUE},
            produces = {MediaType.APPLICATION_JSON_VALUE})
    public User signInUser(HttpServletRequest request,  @RequestParam("email") String email, @RequestParam("password") String password) {
        
	//Set configured FilterFields into session's ATTRIBUTE_FILTER_FIELDS attribute		
	FilterUtil.useFilter(request, FilterFields.getFieldsBy(Arrays.asList("id", "password", "email")));
		
	//DynamicSessionFilter will check ATTRIBUTE_FILTER_FIELDS in request attributes and if attribute has FilterFields
	//it will filter current response
        return databaseService.getUser(email, password);
    }
```

# Spring Security filter example
If your web service configured with Spring Security you can create you own filter as an example below

```java
    @DynamicFilterComponent
    public class DynamicSecurityFilter implements DynamicFilterEvent {
        //Don't hide any fields from admin
        private static FilterFields adminFilterFields = FilterFields.EMPTY_FIELDS;
        
        //Hide some sensitive fields from user
        private static FilterFields userFilterFields = FilterFields.getFieldsBy(Arrays.asList("email", "password"));
    
        @Autowired
        HttpServletRequest request;
    
        @Override
        public void onRequest(Comparator<RequestSession, FilterFields> comparator) {
            comparator
                .compare((request -> request.getRequest().isUserInRole("ADMIN")), result -> adminFilterFields)
                .compare((request -> request.getRequest().isUserInRole("USER")), result -> userFilterFields);
        }
    }
```
* Make sure your Spring Security configuration has **ADMIN** and **USER** roles as shown in example


# Whole Spring REST Web Service controller filtration
If you don't want to add filter annotations to each Web Service response, you can add annotation on whole Web Service
Let's look at the next example

```java
@DynamicFilter(DynamicSessionFilter.class)

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

# Combining Dynamic with other types filtration
From 1.0.18 version you can use dynamic filter along with other type of filtration

```java
    @DynamicFilter(DemoIdFilter.class)
    @FieldFilterSetting(fields = {"id", "password"})
    ...
    @RequestMapping(value = "/users/signIn",
            params = {"email", "password"}, method = RequestMethod.POST,
            consumes = {MediaType.APPLICATION_FORM_URLENCODED_VALUE},
            produces = {MediaType.APPLICATION_JSON_VALUE})            
    public User signIn(@RequestParam("email") String email, @RequestParam("password") String password) {
        return userController.signInUser(email, password);
    } 
```

# In conclusion
With this filter you have a huge space of REST Web Service Response filter customization.