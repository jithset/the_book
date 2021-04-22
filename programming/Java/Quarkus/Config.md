# Configuration

```
greeting.message = hello
greeting.name = quarkus

@ConfigProperty( name = "greeting.message")
public String message;

@ConfigProperty(name = "greeting.message") 
String message;

@ConfigProperty(name = "greeting.suffix", defaultValue="!") 
String suffix;

@ConfigProperty(name = "greeting.name")
Optional<String> name; 
```