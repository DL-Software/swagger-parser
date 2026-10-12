# DL-Software fork of swagger-parser

A narrow fork so openapi-generator can read OpenAPI 3.2 documents before swagger-parser supports them.
Used by DL-Software/godel-public-api, whose Gradle build substitutes `swagger-parser-v3`,
`swagger-parser-core` and `swagger-parser-safe-url-resolver` in the openapi-generator plugin's classpath.

Branch `godel/oas-3.2`, from the upstream tag of the version openapi-generator bundles (`v2.1.37` for
openapi-generator 7.20.0), plus:

1. swagger-api/swagger-parser#2401, unchanged: a 3.2 document is read through the 3.1 model.
2. The 3.2 members that only describe (`itemSchema`, `itemEncoding`, `prefixEncoding`; tag `summary`,
   `parent`, `kind`; response `summary`; server `name`; example `dataValue`, `serializedValue`;
   security scheme `deprecated`) are accepted instead of reported as unexpected. The 3.1 model has no
   place for them, so they are not in the parse result. Members whose loss changes what a document means
   (`$self`, `query`, `additionalOperations`, `mediaTypes`, the new OAuth members) are still reported.
3. Published to GitHub Packages as `<upstream version>-godel.<n>` (`.github/workflows/godel-publish.yml`,
   on a `v*-godel.*` tag), never to Maven Central; upstream's workflows are removed.

Retire the fork when a swagger-parser release reads 3.2 and openapi-generator ships it.
To move to another upstream version: branch from its tag, re-apply 1 and 2, `mvn versions:set`, tag.

`2.1.37-godel.1` is a partial publish (a flaky upstream test failed after three modules had uploaded; versions are immutable). Don't use it; `2.1.37-godel.2` is the first complete one.
