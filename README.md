# Adding custom annotations by substituting mustache templates for v3:swagger-codegen-cli spring generator

## Run with
./gradlew build

result can be seen in `codegen/src`

## How does it work?

`@MyClassAnnotation` and `@MySetterAnnotation` are defined in the subproject `common` in the package `mypackage`


`swagger-codegen-cli-ext` subproject depends on `io.swagger.codegen.v3:swagger-codegen-cli`\
and `codegen` subproject depends on `swagger-codegen-cli-ext` instead of `io.swagger.codegen.v3:swagger-codegen-cli` for code generation
```
swaggerCodegen project(':swagger-codegen-cli-ext')
```

In `swagger-codegen-cli` `handlebars/JavaSpring` directory contains mustache templates for `spring` language.\
In `swagger-codegen-cli-ext` `model.mustache` and `pojo.mustache` are redefined. They are base on the [originals](https://github.com/swagger-api/swagger-codegen-generators/tree/master/src/main/resources/handlebars/JavaSpring)\
They take priority due to dependency distance.

`model.mustache` and `pojo.mustache` contain the code that import annotations and adds those annotations conditionally

```
import mypackage.MyClassAnnotation;
import mypackage.MySetterAnnotation;
...
{{#vendorExtensions.x-my-annotation}}@MyAnnotation{{/vendorExtensions.x-my-annotation}}
```

```
{{#vendorExtensions.x-setter-annotation}}@MySetterAnnotation{{/vendorExtensions.x-setter-annotation}}
```

`petstore.yaml` contains the fields that triggers those conditions:
```
x-setter-annotation: true
x-class-annotation: true
```