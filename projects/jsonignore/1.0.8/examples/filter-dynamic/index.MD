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
                return new FilterFields(String.class, Arrays.asList("id", "password", "email"));
            } else
                return new FilterFields();
        }
    }
```

