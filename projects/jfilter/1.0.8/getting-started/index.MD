---
layout: default
title: JFilter Documentation
description: Version 1.0.8
header_title: Getting started 1.0.8
---

[Main](../../index.MD) > [1.0.8](../index.MD) > Getting Started

# Getting started
For activation of JFilter module just add next annotations

## Component scan and enable filter
```text
@ComponentScan({"com.jfilter.components"})
@EnableJsonFilter
```

### Example 
```java
@Configuration
@EnableWebMvc

@ComponentScan({"com.jfilter.components"})
@EnableJsonFilter
public class AppConfig extends WebMvcConfigurerAdapter {
    
}
```

## Description
* *ComponentScan*  annotation indicates Spring Framework on which folder scan components
* *EnableJsonFilter* annotation enables JFilter filter. Without this annotation module won't process Rest responses 

## XML schema based configuration
When using XML Schema-based configuration, just next component scan configuration

### Example
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans">
       
       
    <context:component-scan base-package="com.jfilter.components"/>
    
</beans>
```