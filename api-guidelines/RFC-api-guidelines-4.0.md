# RFC Ed-Fi API Guidelines Revision 4.0

## Synopsis

This RFC describes a set of proposed changes to the Ed-Fi API Design &
Implementation Guidelines v3.1.

The Ed-Fi API Design & Implementation Guidelines (herein after referred to as
the "Guidelines") describe requirements for an Ed-Fi representational state
transfer (REST) application programming interface (API). The guidelines describe
the properties to which an API specification and related implementation must
adhere in order to be considered aligned to Ed-Fi technology standards. They do
not describe a specific implementation technology.

## Proposed Changes

This RFC describes a set of proposed changes to the Ed-Fi API Design &
Implementation Guidelines v3.1.

The proposed changes are intended to ensure the establishment of API standards
that can be relied upon when using an Ed-Fi compliant API.  To accomplish this
the proposed changes are listed here for comment.

## Document Wide Changes

Any references to a specific implementation of an Ed-Fi API or to an Ed-Fi REST
API should be replaced with "an Ed-Fi API."

## Sections to Add

The following are sections proposed to be added to the current document; these
are new and do not currently exist in any format within the document.

### API Design Guidelines -> API Metadata

#### Version Endpoint

The default response when retrieving the base endpoint on an Ed-Fi
implementation must be a JSON document that provides information about the
application version, supported data model(s), and URLs for additional metadata.
Historically, this root-level metadata resource has been known as the "version
endpoint". The following sample JSON demonstrates the required elements, with an
optional extension (TPDM).

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

While the "suite" string traditionally differentiated between ODS/API 2.x and
3.x+, it could also be used for alternate implementations. For example, the
value could be "meadowlark": "3".

Other URL values can also be included, for example:

```json
"xsdMetadata": "https://api.ed-fi.org/v7.0/api/metadata/xsd",
"changeQueries": "https://api.ed-fi.org/v7.0/api/changeQueries/v1/",
"composites": "https://api.ed-fi.org/v7.0/api/composites/v1/",
"identity": "https://api.ed-fi.org/v7.0/api/identity/v2/"
```

The required URLs are further described in the following sections.

#### Dependencies

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

#### OpenApiMetadata

An Ed-Fi API implementation should declare itself in OpenAPI; the version of
OpenAPI is up to the implementation. All Read-Only data must be labeled using
the OpenAPI specification.

The OpenAPI specification must be a faithful representation of all available
resources supported by the API. A given API application might support multiple
specifications, for example one for core Ed-Fi data model resources, and one for
Ed-Fi descriptors.

#### OAuth

The OAuth URL must be the token URL used by clients for authentication. The URL
is not required to be on the same base address, i.e. it could be hosted on
another server.

#### DataManagementApi

This URL represents the base path for constructing the full URL to access a
resource. To this base path, a client appends the data model namespace

### API Design Guidelines -> Data Strictness

The decisions for this section are to allow the least amount of friction to the
data exchange while still ensuring data are valid.

#### DateTime

All API endpoints with date/time properties should require both the date and
time, not just a date.  The data should include the timezone and should always
be transmitted in ISO format.

#### Case Sensitivity

All API endpoints and queries must be case insensitive to ensure full
compatibility with all backend data sources.

#### Schema Validation

To avoid Mass Assignment attacks and subtle bugs where an optional resource name
has been misspelled, an Ed-Fi API _should_ strictly validate JSON payload
contents, responding with HTTP status 400 when unexpected properties are
present.

For example, the following is a valid `Person` payload:

```json
{
  "id": "string",
  "personId": "string",
  "sourceSystemDescriptor": "string",
}
```

But the following should be rejected:

```json
{
  "id": "string",
  "personId": "string",
  "sourceSystemDescriptor": "string",
  "spurious": "data"
}
```

> [!Note]
> This is a strong recommendation. It is not elevated to a _must_ have
> requirement so that existing version of the Ed-Fi ODS/API do not fall out of
> compliance. This is subject to further refinement in this document drafting
> process.

### API Design Guidelines -> HTTP Verbs

Change all usage of "UUID" to the more generic "unique identifier" or
"identifier", so that the implementation is free to use any scheme, not just a
UUID, as the identifier for a specific resource.

### API Design Guidelines -> Query Operators

#### Ordering

Ordering is a mechanism that sorts the objects returned by one or more of the
fields returned.  Ordering could be implemented with the use of URL query string
parameters. If implemented, it must:

* Take directionality as a parameter (ascending/descending).
* Respect paging parameters.
* Guarantee that repeated queries with the same order and paging parameters,
  when no data changes have occurred, reliably return the same sequence of
  objects.

For example, an Ed-Fi API could return all Student School Associations from
smallest School Id to largest with the following URL fragment:

```none
/ed-fi/studentSchoolAssociations?orderBy=SchoolId&direction=desc.
```

As an experimental feature, this example is merely one possibility, rather than
a prescribed approach.

### API Implementation Guidelines -> Encryption

All client requests to an Ed-Fi API over a public network must use Transport
Layer Security (TLS). Data at rest should be stored with disk level encryption.

## Updates to Existing Sections

The following updates are proposed to existing section of the Ed-Fi API Design &
Implementation Guidelines v3.1.

### API Design Guidelines -> ETags and Other REST API Conventions and Features

#### Other REST API Conventions and Features

Replace Table 7 with the following list:

* Case sensitivity
  * Example

    ```none
    /students?firstName=JOHN
    /students?FIRSTNAME=John
    ```

  * Explanation: URIs, parameter names, and parameter values must not be case
    sensitive. The two URI’s to the left will produce the same results.
* ~~Encryption~~ - MOVE TO IMPLEMENTATION SECTION
* Version
  * Example: `https://api.example.com/v3/ed-fi/students`
  * Explanation: The API standard version number _could_ be specified in the
    base URI, but is not required. Note that `v3` here is for the API standard
    version number, which is represented by these guidelines.

Summary:

* Case sensitivity is unchanged and further confirmed as a requirement.

> [!WARNING]
> Details of case sensitivity are still being worked out. Does this
> extend to payloads on PUT/POST? If so, what do we expect on GET payloads, or
> in streaming output - should the property names be normalized to the "correct"
> casing?
* Use of SSL (now TLS) is important, but it is a deployment feature, not a design feature.
* Clarified that the URI version number is for this API Standard, not for the Data Standard.

### API Design Guidelines -> Ed-Fi Descriptors

Descriptors in the Ed-Fi Data Standard are a set of mechanisms to support flexible enumerations or code tables.  Add the following table defining the attributes of a descriptor.

| Attribute           |  Return from GET | Needed for PUT/POST |	Notes                                               |
| ------------------- |  --------------- | --------------- | ----------------------------------------------------------------------- |
| namespace                    | required       | required                                                               |                                                                                                    |
| codeValue           | required       | required                                                               | Value should be human readable, e.g. prefer one or more words over a numeric code or random string |
| shortDescription             | required       | required                                                               |                                                                                                    |
| description                  | required | optional       | Longer description that may contain additional normative usage guidance |
| effectiveBeginDate           | required | optional      | Date for display only, not validation                                   |
| effectiveEndDate             | required | optional       | Date for display only, not validation                                   |
| id                           | required       | Must not have on POST<br>required on PUT                               |                                                                                                    |
| _etag                      | required   |                                                        optional                 |                                                                                                    |
| xyzDescriptorId             | optional      | Must Not Have                                                           | Retained only for backward-compliance with existing ODS/API implementations.                       |
| priorDescriptorId           | optional      | optional                                                              | Retained only for backward-compliance with existing ODS/API implementations.                       |
| lastModfiedDateTime | optional      | Must Not Have                                                           | Date for display only, not validation                                                              |

### API Design Guidelines -> Query Operators

Modify the following paragraph, with changes highlighted in bold.

> An Ed-Fi API should support searching capabilities with the possible use of **filters, paging, and ordering**. These are discussed below. Information about all supported capabilities, their configurations, and their default values should be available from the API Metadata.
#### Search

Modify the following paragraph, with changes highlighted in bold. Note the removal of any notion of operators other than equal sign.

> An Ed-Fi REST API should support querying capabilities when searching a collection of Resources. Query terms are applied to the query string using the following format: `{collectionURI}?{propertyName}={value}`. **Additional `{propertyName}={value}` terms may be appended after an ampersand, &.**
>
> For example, to search all available Students having the exact first name "John" and sur name "Smith":
>
> `/ed-fi/students?firstName=john&lastSurname=Smith`
>
> **Not all properties are guaranteed to be searchable. The Open API specification for the Ed-Fi API instance will define which properties are available. All searchable property in embedded objects must be dereferenced. For example, a Student School Association document contains schoolReference: `{ schoolId: 12345 }`. The equivalent query must be `/ed-fi/studentSchoolAssociations?schoolId=12345`, not `/ed-fi/studentSchoolAssociations?schoolReference.schoolId=12345`.**
#### Paging

Modify the following paragraph, with changes highlighted in bold.

> Paging is a mechanism that restricts the number of results returned by an operation and has proven critical to the efficient usage of Ed-Fi APIs.  The limit parameter should be supported in the query string and allow the client to set the maximum number of records to return. If no value is supplied, the limit parameter should default to 25.
> The offset parameter **_should_** be available to the client to specify how many records to skip when getting the result set. The value for offset should default to 0. However, some API hosts may wish to disable offset-based searching for performance and reliability reasons.
> When multiple records are being returned, the total count of all records should be returned, as part of the HTTP header information.
> For example, to get the first name and last name of a collection of available Students from positions 31 to 40:
>
> `/ed-fi/students?fields=firstName,lastSurname&limit=10&offset=30`
>
> **Alternate approaches to paging could be implemented, for example using keyset or cursor-based parameters.**
This change from must to should for the limit and offset parameters is in
recognition of significant performance degradation in many database platforms,
when using this technique beyond around 10,000 records. Some implementations may
wish to disable the functionality to avoid excessive database usage.**

### API Design Guidelines -> Response Codes

#### Optional Response Codes

Add a new table with the following optional response codes:

| HTTP Response Code | Name                                                                                                         | Reason(s)                                                                                           |
| ------------------ | ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------- |
| 406                | Not Acceptable                                                                                               | Used when the "accept" header requested cannot be supported on a GET request.                       |
| 415                | Unsupported Media Type                                                                                       | Used when the "content-type" header or "content-encoding" header is not supported on a PUT or POST. |
| 492                | Too Many Requests                                                                                            | Rate limiting                                                                                       |
| 501                | Not Implemented | Example: a read-only API implementation could use 501 for any POST, PUT, or DELETE requests. |
| 502                | Bad Gateway                                                                                                  | An upstream service is unavailable; the client application should stop sending requests for a time. |

#### Errors

Modify the following text (modifications are in bold):

 > **If an error with a response code not listed above occurs on the server, a
 > 500 (Internal Server Error) code can be returned.** A message in the body,
 > containing the error details, should be provided. However, raw errors
 > generated by system failures must not returned to the client to avoid
 > inadvertently exposing any sensitive data or technical information to an
 > attacker.
>
> For example:
>
> ```json
> {
>     "code": 500,
>     "type": "Internal Server Error",
>     "message": "Unable to communicate with database"
> }
> ```
## Existing Sections to Remove
The following obsolete sections are recommended to be removed.
### User Authorization
Recommended that this section is removed as the document is focused on systems
and not end users.
### User Claims
Recommended that this section is removed as the document is focused on systems
and not end users.
### User Roles
Recommended that this section is removed as the document is focused on systems
and not end users.
### External User Authorization
Recommended that this section is removed as the document is focused on systems
and not end users.
### Internal User Authorization
Recommended that this section is removed as the document is focused on systems
and not end users.
### Bulk Operations
Recommended that this section is removed as the document is focused API
Standards and not usage of a specific API implementation.
