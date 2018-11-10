---
layout: default
title: JFilter Documentation
description: Version 1.0.8
---

[Main](../../../index.MD) > [1.0.8](../../index.MD) >  [Examples](../index.MD) > XML Schema-based filter configuration

# At first
Add *@EnableScheduling* annotation in your Spring Service configuration.
This is necessary in order for the auto-reloading algorithm of changed files which will reloaded while the service is running.
Сhanges checking occur every 2 seconds. And you could be sure that all configuration files is up to date.

## Example

```java

```


# XML Schema-based filter configuration
If you have huge count of field or strategy filters configurations you can configure them in XML file
XML configuration uses next [DTD](https://rkonovalov.github.io/json-ignore-schema-1.0.dtd) structure file.

* Example of XML configuration file (filter_configuration.xml)

```xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE config PUBLIC
        "-//json/json-ignore mapping DTD 1.0//EN"
        "https://rkonovalov.github.io/json-ignore-schema-1.0.dtd">
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