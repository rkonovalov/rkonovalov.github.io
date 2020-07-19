---
layout: default
title: JFilter Documentation
description: Version 1.0.18
header_title: Getting started 1.0.18
menu_title: Getting started
---

[Main](../../index.MD) > [1.0.18](../index.MD) > Getting Started

# Getting started
For activation of JFilter library just add next annotation

## Component scan and enable filter
```text
@EnableJsonFilter
```

### Example 
```java
@Configuration
@EnableWebMvc
@EnableJsonFilter
public class AppConfig extends WebMvcConfigurerAdapter {
    
}
```

## Description
* *EnableJsonFilter* annotation enables JFilter filter. Without this annotation JFilter library won't process REST Web Service responses 

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