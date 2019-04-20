---
layout: default
title: JFilter Documentation
description: Version 1.0.12
header_title: Configuration 1.0.12
---

[Main](../../index.MD) > [1.0.12](../index.MD) > Configuration

# Configuration
If you need to enable/disable of module or extend list of ObjectMapper serializers, you can fallow next steps

## Enabling/disabling module
If you need to disable module you can comment EnableJsonFilter annotation
```java
//@EnableJsonFilter
```
But, if you need to control state of module programmatically, you can use nex example:

```java
@Configuration
@EnableWebMvc
@EnableJsonFilter
public class AppConfig {
  
  @Autowired
  public void configure(FilterConfiguration filterConfiguration) {      
      //Enabling module
      filterConfiguration.setEnabled(true);
      
      //disabling module
      filterConfiguration.setEnabled(false);
  }
    
}
```

## Extending serializer list
By default module uses Json and XML ObjectMappers for serialize Spring HTTP messages
If you need add custom Object mapper you can use next example
```java
@Configuration
@EnableWebMvc
@EnableJsonFilter
public class AppConfig {
  
  @Autowired
  public void configure(FilterConfiguration filterConfiguration) { 
  
      //Add custom ObjectMapper
      filterConfiguration.setMapper(MediaType.APPLICATION_JSON, new ObjectMapper());        
  }
    
}
```
If configuration already has ObjectMapper by specified MediaType it will be overridden by new ObjectMapper
