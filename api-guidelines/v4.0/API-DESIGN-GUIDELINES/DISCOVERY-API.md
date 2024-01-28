# Discovery API

## Version Endpoint

The default response when retrieving the base endpoint on an Ed-Fi
implementation must be a JSON document that provides information about the
application version, suported data model(s), and URLs for additional metadata.
Historically, this root-level metadata resource has been known as the "version
endpoint".  The following sample JSON demonstrates the required elements, with
an optional extension (TPDM).

```json
{
  "version": "7.0",
  "suite": "3",
  "dataModels": [
    {
      "name": "Ed-Fi",
      "version": "5.0.0",
      "informationalVersion": "The Ed-Fi Data Model 5.0"
    },
    {
      "name": "TPDM",
      "version": "1.1.0",
      "informationalVersion": "TPDM-Core"
    }
  ],
  "urls": {
    "dependencies": "https://api.ed-fi.org/v7.0/api/metadata/data/v3/dependencies",
    "openApiMetadata": "https://api.ed-fi.org/v7.0/api/metadata/",
    "oauth": "https://api.ed-fi.org/v7.0/api/oauth/token",
    "dataManagementApi": "https://api.ed-fi.org/v7.0/api/data/v3/",
  }
}
```

The "suite" string traditionally differentiated between ODS/API 2.x and 3.x+.
The numbers "2" and "3" should only be used for the Ed-Fi ODS/API software.
Other Ed-Fi compliant applications should provide their own moniker for the
"suite". This is the one programmatic means for an API client to determine which
software it is working with. In day-to-day interactions the software
implementing the API's should not matter, but this information could be useful
in debugging situations.

Other URL values can also be included, for example:

```json
"xsdMetadata": "https://api.ed-fi.org/v7.0/api/metadata/xsd",
"changeQueries": "https://api.ed-fi.org/v7.0/api/changeQueries/v1/",
"composites": "https://api.ed-fi.org/v7.0/api/composites/v1/",
"identity": "https://api.ed-fi.org/v7.0/api/identity/v2/"
```

The required URLs are further described in the following sections.

## Dependencies

All implementations of an Ed-Fi API should expose an endpoint for dependency
metadata.  This endpoint will provide developers with the data needed to load
resources in the appropriate order.  The endpoint should have a JSON
implementation and optional a GraphML implementation.  A sample of the JSON
output is below:

```json
[
  {
    "resource": "/ed-fi/absenceEventCategoryDescriptors",
    "order": 1,
    "operations": [
      "Create"
    ]
  },
  {
    "resource": "/ed-fi/academicHonorCategoryDescriptors",
    "order": 1,
    "operations": [
      "Create"
    ]
  },
  …
  {
    "resource": "/tpdm/surveySectionResponsePersonTargetAssociations",
    "order": 22,
    "operations": [
      "Create"
    ]
  }
]
```
## OpenApiMetadata

An Ed-Fi API implementation should declare itself in OpenAPI; the version of
OpenAPI is up to the implementation. All Read-Only data must be labeled using
the OpenAPI specification.

The OpenAPI specification must be a faithful representation of all available
resources supported by the API. A given API application might support multiple
specifications, for example one for core Ed-Fi data model resources, and one for
Ed-Fi descriptors.

## OAuth

The OAuth URL must be the token URL used by clients for authentication. The URL
is not required to be on the same base address, i.e. it could be hosted on
another server.

## DataManagementApi

This URL represents the base path for constructing the full URL to access a
resource. To this base path, a client appends the data model namespace


## API Guidelines Contents

* [Scope](../SCOPE.md)
* [Key Characteristics](../KEY-CHARACTERISTICS.md)
* [Requirement Levels](../REQUIREMENT-LEVELS.md)
* [API Design Guidelines](../API-DESIGN-GUIDELINES/README.md)
  * [API Metadata](API-METADATA.md)
  * [Data Strictness](DATA-STRICTNESS.md)
  * [Resources](RESOURCES.md)
  * [HTTP Verbs](HTTP-VERBS.md)
  * [General Request Construction](GENERAL-REQUEST-CONSTRUCTION.md)
  * [Ed-Fi Descriptors](ED-FI-DESCRIPTORS.md)
  * [Query Operators](QUERY-OPERATORS.md)
  * [Response Codes](RESPONSE-CODES.md)
  * [Encryption](ENCRYPTION.md)
  * [ETags and Other REST API Conventions and
  Features](ETAGS-OTHER-CONVENTIONS.md)
* [API Implementation Guidelines](../API-IMPLEMENTATION-GUIDELINES/README.md)
