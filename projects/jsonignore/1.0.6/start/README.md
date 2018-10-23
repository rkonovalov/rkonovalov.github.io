# Getting started page
For activation of Json Ignore module just add next annotations

## Component scan and enable filter
```text
@ComponentScan({"com.json.ignore.advice"})
@EnableJsonFilter
```

### Example 
```java
@Configuration
@EnableWebMvc

@ComponentScan({"com.json.ignore.advice"})
@EnableJsonFilter
public class AppConfig extends WebMvcConfigurerAdapter {
    
}
```

## Description
* *ComponentScan*  annotation indicates Spring Framework on which folder scan components
* *EnableJsonFilter* annotation enables Json Ignore filter. Without this annotation module won't process Rest responses 
