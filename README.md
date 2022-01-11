# Adding custom annotation by substituting mustache templates for v3:swagger-codegen-cli spring generator

## Run with
./gradlew build

result can be seen in `codegen/src`

## How does it work?

`@MyClassAnnotation` and `@MySetterAnnotation` are defined in the subproject `common` in the package `mypackage`

Subproject `swagger-generator-ext` depends `io.swagger.codegen.v3:swagger-codegen-cli`
In `swagger-codegen-cli` the `resources/handlebars/JavaSpring` directory contains mustache templates for `spring` language.
In `swagger-generator-ext` `model.mustache` and `pojo.mustache` are redefined. They are base on the originals, see `https://github.com/swagger-api/swagger-codegen-generators/tree/master/src/main/resources/handlebars/JavaSpring`
They take priority over the originals due to dependency distance.

`model.mustache` and `pojo.mustache` contain the code that import annotations and adds those annotations conditionally
"""
import mypackage.MyClassAnnotation;
import mypackage.MySetterAnnotation;
...
{{#vendorExtensions.x-my-annotation}}@MyAnnotation{{/vendorExtensions.x-my-annotation}}
"""
"""
{{#vendorExtensions.x-setter-annotation}}@MySetterAnnotation{{/vendorExtensions.x-setter-annotation}}
"""

`petstore.yaml` contains the fields that trigger those conditions:
"""
x-setter-annotation: true
x-class-annotation: true
"""