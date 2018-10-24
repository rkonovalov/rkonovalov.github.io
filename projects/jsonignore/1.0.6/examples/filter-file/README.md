# Filtration configuration in xml based file
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
* **strategy** - this TAG similar [SessionStrategy](../filter-strategy/README.md) annotation
* **filter** - this TAG similar [FieldFilterSetting](../filter-field/README.md) annotation

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