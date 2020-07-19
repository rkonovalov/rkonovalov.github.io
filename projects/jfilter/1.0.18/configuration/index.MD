---
layout: default
title: JFilter Documentation
description: Version 1.0.18
header_title: Configuration 1.0.18
menu_title: Configuration
---

[Main](../../index.MD) > [1.0.18](../index.MD) > Configuration

# Configuration
If you need to enable/disable of JFilter library or extend list of ObjectMapper serializers, you can fallow next steps

## Enabling/disabling JFilter library
If you need to disable JFilter library you can comment EnableJsonFilter annotation
```java
//@EnableJsonFilter
```
But, if you need to control state of JFilter library programmatically, you can use nex example:

```java
@Configuration
@EnableWebMvc
@EnableJsonFilter
public class AppConfig {
  
  @Autowired
  public void configure(FilterConfiguration filterConfiguration) {      
      //Enabling JFilter library
      filterConfiguration.setEnabled(true);
      
      //Disabling JFilter library
      filterConfiguration.setEnabled(false);
  }
    
}
```

## Extending serializer list
By default JFilter library uses Json and XML ObjectMappers for serialize Spring HTTP messages
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
If configuration already has **ObjectMapper** by specified MediaType it will be overridden by new ObjectMapper

## Spring Boot default ObjectMapper
If environment is **Spring Boot**, you can use default **ObjectMapper**

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

## Using default Spring Message Converters
Also you can use default Spring message converters **MappingJackson2HttpMessageConverter** and **MappingJackson2XmlHttpMessageConverter** instead of **FilterConverter**
```java
@Configuration
@EnableWebMvc
@EnableJsonFilter
public class AppConfig {
  
  @Autowired
  public void configure(FilterConfiguration filterConfiguration) {   
      //Use default converters
      filterConfiguration.setUseDefaultConverters(true);   
  }
    
}
```
JFilter will use default message converters with **FilterObjectMapper** and **FilterXmlMapper** instead of **ObjectMapper** and **XmlMapper**

## Using custom Spring Message Converters
You can use your own configured Message Converter

```java
@Configuration
@EnableWebMvc
@EnableJsonFilter
public class AppConfig {

    @Autowired
    public void configure(FilterConfiguration filterConfiguration) {

        //Add custom Json converter
        filterConfiguration.withCustomConverter(new MappingJackson2HttpMessageConverter(new FilterObjectMapper(filterConfiguration)));

        //Add custom XML converter
        filterConfiguration.withCustomConverter(new MappingJackson2XmlHttpMessageConverter(new FilterXmlMapper(filterConfiguration)));
    }

}
```
* **Note**: ObjectMapper in custom Message Converter should be or extended from **FilterObjectMapper** or **FilterXmlMapper**. Otherwise JFilter library will raise exception

## Configuring exist ObjectMappers
You can modify exist ObjectMappers

Configure all ObjectMappers of JFilter library
```java
@Configuration
@EnableAsync
@EnableJsonFilter
public class AppConfig {

    @Autowired
    public void configure(FilterConfiguration filterConfiguration) {
        //Disable WRITE_DATES_AS_TIMESTAMPS feature in all ObjectMappers
        filterConfiguration.findObjectMapper((mapper) -> mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS));
    }
}
```

Or configure specified ObjectMapper of JFilter library by MediaType
```java
@Configuration
@EnableAsync
@EnableJsonFilter
public class AppConfig {

    @Autowired
    public void configure(FilterConfiguration filterConfiguration) {
          //Disable WRITE_DATES_AS_TIMESTAMPS feature in ObjectMapper which serializes objects for messages with JSON MediaType
          filterConfiguration.findObjectMapper(MediaType.APPLICATION_JSON, (mapper) -> mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS));
    }
}
```