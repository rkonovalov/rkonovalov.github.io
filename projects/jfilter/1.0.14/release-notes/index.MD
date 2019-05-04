---
layout: default
title: JFilter Documentation
description: Version 1.0.14
header_title: Release notes 1.0.14
---

[Main](../../index.MD) > [1.0.14](../index.MD) > Release notes

# Release notes

## Version 1.0.14
    * Refactored FilterConverter class for inheriting AbstractHttpMessageConverter instead of HttpMessageConverter
    * Added Comparator class for simplify if...else branching. Including in dynamic filter components
    * Refactored DynamicSessionFilter component

## Version 1.0.12
    * Added ObjectMapperCache for caching of ObjectMapper from each method in Spring controller
    * Added FilterConfiguration for extending of exist list of ObjectMappers and enabling/disabling of module
    * Refactored code mechanizm of serializing Spring http messages

## Version 1.0.11
    * Added HttpServletRequest in RequestSession for using in DynamicFilterComponent Case #6 
    * Added FilterBehaviour for keep/hide filtering fields Case #7 
    
## Version 1.0.10
    * Fixed bug in integration with Swagger UI Case #3
    
## Version 1.0.9
    * Fixed bug "No converter found for return value of type" Case #1
    * Added little features from Case #4, Case #5 

## Version 1.0.8
    * Added Dynamic Filter which allows to create own filters with custom behaviour
    * Added DynamicSessionFilter for storing custom field filter in session
    * Added feature for auto loading XML Schema-based configuration filter when it changed on runtime
    * Changed package name from com.json.ignore to com.jfilter 
    * Changed package from advice to components
    * Fixed bug in FileFilter which not allowed to load xml configuration from external source

## Version 1.0.7
    * Added FileWatcher controller which auto reloads modified XML Schema based configurations
    * Refactored Javadoc
    * Fixed incorrect Jackson XML dependency importing

## Version 1.0.6
    * Added JSON/XML converters inherited from HttpMessageConverter
    * Removed native reflection and added Jackson BeanSerializerModifier for field filtering
    * Added EnableJsonFilter annotation for enabling/disabling filtration
    * Added ability to apply filter annotations on whole Spring Rest controller
    * Fixed bugs

## Version 1.0.5
    * Added Filter provider for improving execution speed
    * Fixed bugs 

## Version 1.0.4
    * Added xml Schema-based configuration
    * Fixed bugs 

## Version 1.0.3
    * Added session strategy filtering

## Version 1.0.2
    * Added additional constructors

## Version 1.0.0
    * Initial release