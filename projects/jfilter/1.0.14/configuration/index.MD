---
layout: default
title: JFilter Documentation
description: Version 1.0.14
header_title: Configuration 1.0.14
---

[Main](../../index.MD) > [1.0.14](../index.MD) > Configuration

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
  
      //Add custom Json ObjectMapper
      filterConfiguration.setMapper(MediaType.APPLICATION_JSON, new ObjectMapper());    
      
      //Add custom Json ObjectMapper
      filterConfiguration.setMapper(MediaType.APPLICATION_XML, new XmlMapper());     
  }
    
}
```
If configuration already has ObjectMapper by specified MediaType it will be overridden by new ObjectMapper

## Spring Boot default ObjectMapper
If environment is Spring Boot, you can use default ObjectMapper

```java
@Configuration
@EnableWebMvc
@EnableJsonFilter
public class AppConfig {
  
  @Autowired
  public void configure(FilterConfiguration filterConfiguration, ObjectMapper objectMapper) { 
  
      //Add custom Json ObjectMapper
      filterConfiguration.setMapper(MediaType.APPLICATION_JSON, objectMapper);     
  }
    
}
```
