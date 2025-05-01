## 1. Introduction

**Проектирование API (Contract-First):** Позволяет спроектировать и согласовать API до написания кода. **Генерация кода:** Используется инструментами (как OpenAPI Generator) для генерации клиентских SDK, серверных заглушек/интерфейсов.

[Open API](https://swagger.io/specification/) is a specification for designing and documenting RESTful APIs. [OpenAPI generator](https://openapi-generator.tech/) is a tool used in API-first development as it can generate client and server source code from OpenAPI 2.0/3.x documents. It supports multiple languages and frameworks. Although most of the time the generated code is ready to be used without modification, there are scenarios in which we need to customize it.

Примерный путь работы с api:

- Configure “`openapi-generator-maven-plugin`“.
- Create an OpenAPI specification – `products.yaml`.
- Execute the maven `generate-source` command to generate source code from the `products.yaml` file.
- Create an implementation class for the generated interface.

Пример описания плагина в Maven:

```xml
<plugin>  
    <groupId>org.openapitools</groupId>  
    <artifactId>openapi-generator-maven-plugin</artifactId>  
    <version>7.5.0</version>  
    <executions>  
        <execution>  
            <goals>  
                <goal>generate</goal>  
            </goals>  
            <configuration>  
                <inputSpec>${project.basedir}/src/main/resources/api/openapi-recommendation.yaml</inputSpec>  
                <generatorName>spring</generatorName>  
                <apiPackage>com.lobanov.learnopenapi.api.recommendation</apiPackage>  
                <modelPackage>com.lobanov.learnopenapi.model.recommendation</modelPackage>  
                <configOptions>  
                    <interfaceOnly>true</interfaceOnly>  
                    <useJakartaEe>true</useJakartaEe>  
                    <useSpringBoot3>true</useSpringBoot3>  
                    <useBeanValidation>true</useBeanValidation>  
                    <dateLibrary>java8</dateLibrary>  
                </configOptions>  
                <output>${project.build.directory}/generated-sources/openapi</output>  
                <addCompileSourceRoot>true</addCompileSourceRoot>  
            </configuration>  
        </execution>  
  
        <execution>  
            <id>generate-events-api</id>  
            <goals>  
                <goal>generate</goal>  
            </goals>  
            <configuration>  
                <inputSpec>${project.basedir}/src/main/resources/api/openapi-events.yaml</inputSpec>  
                <generatorName>spring</generatorName>  
                <apiPackage>com.lobanov.learnopenapi.api.events</apiPackage>  
                <modelPackage>com.lobanov.learnopenapi.api.model</modelPackage>  
                <configOptions>  
                    <interfaceOnly>true</interfaceOnly>  
                    <useJakartaEe>true</useJakartaEe>  
                    <useSpringBoot3>true</useSpringBoot3>  
                    <useBeanValidation>true</useBeanValidation>  
                    <dateLibrary>java8</dateLibrary>  
                </configOptions>  
                <output>${project.build.directory}/generated-sources/openapi</output>  
                <addCompileSourceRoot>true</addCompileSourceRoot>  
            </configuration>  
        </execution>  
    </executions>  
</plugin>
```

Пример указан с двумя спецификациями.

Стандартное положение спецификации: resources -> api. В данном примере были использованы openapi-events.yaml и openapi-recommendation.yaml

Пример спецификации (также: https://editor.swagger.io/):

```yml
openapi: 3.0.3  
info:  
  title: Banking Product Recommendations API  
  description: Provides personalized banking product recommendations for users.  
  version: 1.0.1  
servers:  
  - url: /api/v1  
tags:  
  - name: Recommendations  
    description: Operations related to product recommendations  
paths:  
  /users/{userId}/recommendations:  
    get:  
      tags:  
        - Recommendations  
      summary: Get product recommendations for a specific user  
      description: Retrieves a list of recommended banking products based on user profile and potentially other factors.  
      operationId: getUserRecommendations  
      parameters:  
        - name: userId  
          in: path  
          required: true  
          description: The unique identifier of the user.  
          schema:  
            type: string  
            example: "user-12345"  
        - name: productType # Необязательный фильтр по типу продукта  
          in: query  
          required: false  
          description: Filter recommendations by product type.  
          schema:  
            type: string  
            enum: [CREDIT_CARD, SAVINGS_ACCOUNT, LOAN, INVESTMENT] # Допустимые значения  
            example: CREDIT_CARD  
        - name: count  
          in: query  
          required: false  
          description: Maximum number of recommendations to return.  
          schema:  
            type: integer  
            format: int32  
            minimum: 1  
            maximum: 50  
            default: 10  
      responses:  
        '200':  
          description: A list of recommended products.  
          content:  
            application/json:  
              schema:  
                type: array  
                items:  
                  $ref: '#/components/schemas/Recommendation' # Ссылка на модель Recommendation  
        '400':  
          description: Invalid input parameters.  
          content:  
            application/json:  
              schema:  
                $ref: '#/components/schemas/ErrorResponse'  
        '404':  
          description: User not found.  
          content:  
            application/json:  
              schema:  
                $ref: '#/components/schemas/ErrorResponse'  
        '500':  
          description: Internal server error during recommendation generation.  
          content:  
            application/json:  
              schema:  
                $ref: '#/components/schemas/ErrorResponse'  
  
components:  
  schemas:  
    Recommendation:  
      type: object  
      properties:  
        productId:  
          type: string  
          description: Unique identifier for the recommended product.  
          example: "CC-PLATINUM-001"  
        productName:  
          type: string  
          description: Human-readable name of the product.  
          example: "Platinum Credit Card"  
        productType:  
          $ref: '#/components/schemas/ProductType' # Ссылка на Enum типа продукта  
        score:  
          type: number  
          format: double  
          description: A score indicating the relevance of the recommendation (e.g., 0.0 to 1.0).  
          example: 0.85  
        detailsUrl:  
          type: string  
          format: url  
          description: A URL pointing to more details about the product.  
          example: "https://bank.example.com/products/cc-platinum"  
      required:  
        - productId  
        - productName  
        - productType  
  
    ProductType:  
      type: string  
      enum: [CREDIT_CARD, SAVINGS_ACCOUNT, LOAN, INVESTMENT, INSURANCE]  
      description: The type of banking product.  
      example: SAVINGS_ACCOUNT  
  
    ErrorResponse:  
      type: object  
      required:  
        - code  
        - message  
      properties:  
        code:  
          type: string  
          example: "USER_NOT_FOUND"  
        message:  
          type: string  
          example: "The specified user ID does not exist."
```

Пример описание плагина в Gradle для 4 файлов спецификации:

```groovy
// Настраиваем все задачи типа GenerateTask  
tasks.withType<org.openapitools.generator.gradle.plugin.tasks.GenerateTask>().configureEach {  
    group = "OpenAPI Tools" // Группируем задачи в Gradle  
    generatorName.set("spring")  
    library.set("spring-cloud")  
    outputDir.set(generatedSourcesPath)  
    configOptions.set(commonApiConfigOptions)  
    globalProperties.set(mapOf("apis" to "", "models" to "")) // Отключаем генерацию по умолчанию, будем задавать через пакеты  
    generateApiTests.set(false)  
    generateModelTests.set(false)  
    logToStderr.set(true) // Выводить лог генератора в stderr  
}  
  
tasks.findByName("openApiGenerate")?.let {  
    logger.lifecycle("Disabling default 'openApiGenerate' task as custom generation tasks are used.")  
    it.enabled = false  
}  
  
tasks.register<org.openapitools.generator.gradle.plugin.tasks.GenerateTask>("generateCustomerProfileClient") {  
    group = "OpenAPI Generation"  
    description = "Generates Customer Profile Feign client"  
    inputSpec.set(project.file("src/main/resources/openapi/customer-profile-api.yaml").absolutePath)  
    apiPackage.set("$generatedApiPackage.profile")  
    modelPackage.set("$generatedModelPackage.profile")  
    typeMappings.put("double", "BigDecimal")  
    importMappings.put("BigDecimal", "java.math.BigDecimal")  
}  
  
tasks.register<org.openapitools.generator.gradle.plugin.tasks.GenerateTask>("generateAccountBalanceClient") {  
    group = "OpenAPI Generation"  
    description = "Generates Account Balance Feign client"  
    inputSpec.set(project.file("src/main/resources/openapi/account-balance-api.yaml").absolutePath)  
    apiPackage.set("$generatedApiPackage.balance")  
    modelPackage.set("$generatedModelPackage.balance")  
    typeMappings.put("double", "BigDecimal")  
    importMappings.put("BigDecimal", "java.math.BigDecimal")  
}  
  
tasks.register<org.openapitools.generator.gradle.plugin.tasks.GenerateTask>("generateIncomeClient") {  
    group = "OpenAPI Generation"  
    description = "Generates Income Verification Feign client"  
    inputSpec.set(project.file("src/main/resources/openapi/income-verification-api.yaml").absolutePath)  
    apiPackage.set("$generatedApiPackage.income")  
    modelPackage.set("$generatedModelPackage.income")  
    typeMappings.put("double", "BigDecimal")  
    importMappings.put("BigDecimal", "java.math.BigDecimal")  
}  
  
tasks.register<org.openapitools.generator.gradle.plugin.tasks.GenerateTask>("generateSpendingClient") {  
    group = "OpenAPI Generation"  
    description = "Generates Spending Analysis Feign client"  
    inputSpec.set(project.file("src/main/resources/openapi/spending-analysis-api.yaml").absolutePath)  
    apiPackage.set("$generatedApiPackage.spending")  
    modelPackage.set("$generatedModelPackage.spending")  
    typeMappings.put("double", "BigDecimal")  
    importMappings.put("BigDecimal", "java.math.BigDecimal")  
}
  
sourceSets["main"].java {  
    srcDir("$generatedSourcesPath/src/main/java")  
}  
  
// --- Гарантируем, что код генерируется перед компиляцией ---  
tasks.withType<JavaCompile>().configureEach {  
    dependsOn(tasks.withType<org.openapitools.generator.gradle.plugin.tasks.GenerateTask>())  
}
```

## Resources

- [OpenAPI Generator Custom Templates](https://www.javacodegeeks.com/openapi-generator-custom-templates.html)