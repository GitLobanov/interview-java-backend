# Swagger Config

```java

@Configuration  
public class SwaggerConfig {  
  
   @Bean  
   public GroupedOpenApi publicApi(){  
       return GroupedOpenApi.builder()  
               .group("user-service")  
               .pathsToMatch("/user-service/**")  
               .build();  
   }  
  
    @Bean  
    public OpenAPI customOpenApi(  
            @Value("${APPLICATION_NAME:USER-SERVICE}") String appName,  
            @Value("${APPLICATION_DESCRIPTION:A-Tink - banking application}") String appDescription,  
            @Value("${APPLICATION_VERSION: 0.0.1-SNAPSHOT}") String appVersion) {  
  
        return new OpenAPI()  
                .addSecurityItem(new SecurityRequirement().addList("ApiKeyAuth"))  
                .components(new Components().addSecuritySchemes("ApiKeyAuth",  
                        new SecurityScheme()  
                                .name("Authorization")  
                                .in(SecurityScheme.In.HEADER)  
                                .type(SecurityScheme.Type.APIKEY)))  
                .info(new Info().title(appName)  
                        .version(appVersion)  
                        .description(appDescription));  
    }  
}
```

# Dependencies

```xml
<swagger.version>2.0.2</swagger.version>
<dependency>  
    <groupId>org.springdoc</groupId>  
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>  
    <version>${swagger.version}</version>  
</dependency>
```

