_Author_:  @DimuthuMadushan \
_Created_: 2026/10/06 \
_Updated_: 2026/10/06 \
_Edition_: Swan Lake

# Sanitation for OpenAPI specification

This document records the sanitation done on top of the official OpenAPI specification from Commercetools.
The OpenAPI specification is obtained from [https://github.com/wso2/api-specs/blob/main/openapi/commercetools/auditlog/v1/openapi.yaml](https://github.com/wso2/api-specs/blob/main/openapi/commercetools/auditlog/v1/openapi.yaml).
These changes are done in order to improve the overall usability, and as workarounds for some known language limitations.

1. **Subset the spec to the Audit Log connector's previous scope.** The source spec is the full commercetools Composable Commerce API (299 paths). Only `GET /{projectKey}` (`ByProjectKeyGet`) and `POST /{projectKey}` (`ByProjectKeyPost`) were kept. `HEAD /{projectKey}` (`ByProjectKeyHead`) and every other path were dropped to match the previous scope of the connector. `components` was pruned to the transitive closure of the schemas and responses those two operations reference, plus the `oauth_2_0` security scheme (52 schemas, 8 responses). The spec remains self-contained.

2. **Add missing operation summaries and descriptions.** `GET /{projectKey}` is summarised as "Get project settings" and `POST /{projectKey}` as "Update project settings". Both operations also gained a description, the `200` responses replaced the bare `'200'` description, and the `POST` request body got a description and `required: true`, since `version` and `actions` are required.

3. **Remove a bogus required property from `ErrorObject`.** The `required` list of `ErrorObject` contained `//`, which is not a property of the schema (it comes from a comment in the vendor's source definition). It was removed so the schema only requires `code` and `message`.

4. **Replace the templated server URL with a concrete one.** The `https://api.{region}.commercetools.com` server variable was replaced by the default region host `https://api.us-central1.gcp.commercetools.com`, so the generated client has a matching default `serviceUrl`. Connectors for other regions pass their own `serviceUrl`.

## OpenAPI cli command

The following command was used to generate the Ballerina client from the OpenAPI specification. The command should be executed from the repository root directory.

```bash
bal openapi -i docs/spec/aligned_ballerina_openapi.json -o ballerina --mode client --client-methods remote --license docs/license.txt
```

Note: The license year is hardcoded to 2026, change if necessary.
