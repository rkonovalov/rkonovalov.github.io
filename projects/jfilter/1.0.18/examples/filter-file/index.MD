---
layout: default
title: JFilter Documentation
description: Version 1.0.18
menu_title: XML Schema-based filter configuration
---

[Main](../../../index.MD) > [1.0.18](../../index.MD) >  [Examples](../index.MD) > XML Schema-based filter configuration

# XML Schema-based filter configuration
If you have huge count of field or strategy filters configurations you can configure them in XML file
XML configuration uses next [DTD](https://rkonovalov.github.io/jfilter-schema-1.1.dtd) structure file.

* Example of XML configuration file (filter_configuration.xml)

```xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE config PUBLIC
        "-//jfilter mapping DTD 1.0//EN"
        "https://rkonovalov.github.io/jfilter-schema-1.1.dtd">
<config>
    <controller class-name="com.example.SessionService">
        <strategy attribute-name="ROLE" attribute-value="USER">

            <filter class="com.example.User">
                <field name="password"/>
            </filter>
            
        </strategy>       
    </controller>
</config>
```

## Description of XML tags
* **config** - main tag
* **controller** - in this TAG you may set **class-name** property. Set Service class name where used this configuration
* **strategy** - this TAG similar [SessionStrategy](../filter-strategy/index.MD) annotation
* **filter** - this TAG similar [FieldFilterSetting](../filter-field/index.MD) annotation

## Example of using

```java
@FileFilterSetting(fileName = "filter_configuration.xml")

@RequestMapping(value = "/users/signIn",
             params = {"email", "password"}, method = RequestMethod.POST,
             consumes = {MediaType.APPLICATION_FORM_URLENCODED_VALUE},
             produces = {MediaType.APPLICATION_JSON_VALUE})            
     public User signIn(@RequestParam("email") String email, @RequestParam("password") String password) {
         return userController.signInUser(email, password);
     }
```

Where
* **fileName** - is xml configuration file name. XML file should be in /resources/ or sub folders

## Auto reloading changed XML files
Also has been added FileWatcher controller which auto reloads changed xml files.
Therefore xml configuration files could be change after application starts 
and controller will correctly reloads all changed files.

# XML configuration example with keep fields behaviour
If you need instead of hide some fields keep them, you can use next configuration example

```xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE config PUBLIC
        "-//jfilter mapping DTD 1.0//EN"
        "https://rkonovalov.github.io/jfilter-schema-1.1.dtd">
<config>
    <controller class-name="com.example.SessionService">
        <strategy attribute-name="ROLE" attribute-value="USER">

            <filter class="com.example.User" behaviour="KEEP_FIELDS">
                <field name="password"/>
            </filter>
            
        </strategy>       
    </controller>
</config>
```
* **behaviour** - this is behaviour parameter, it could have next values:
  1. **KEEP_FIELDS** - hide all fields bot not specified in field list
  2. **HIDE_FIELDS**(by default) - hide fields specified in field list