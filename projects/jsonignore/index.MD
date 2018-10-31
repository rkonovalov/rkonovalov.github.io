---
layout: default
title: Json Ignore module
description: Release notes
---

Main

# Welcome to Json Ignore page
This module could be used in Spring MVC Rest project for filter(exclude) of fields in Service response.
When you used Spring @View interface and need more powerful and flexibility, this module could useful.

## Manuals
* [1.0.6](1.0.6/index.MD)
* [Maven repository](https://search.maven.org/artifact/com.github.rkonovalov/json-ignore/1.0.6/jar)
* [Javadoc](http://www.javadoc.io/doc/com.github.rkonovalov/json-ignore/1.0.6)


# Release notes

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

