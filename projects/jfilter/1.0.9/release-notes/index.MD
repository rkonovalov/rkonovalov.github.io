---
layout: default
title: JFilter Documentation
description: Version 1.0.9
header_title: Release notes 1.0.9
---

[Main](../../index.MD) > [1.0.9](../index.MD) > Release notes

# Release notes

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