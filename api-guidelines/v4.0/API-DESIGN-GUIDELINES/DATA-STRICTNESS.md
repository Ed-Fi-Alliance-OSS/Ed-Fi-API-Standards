# Data Strictness

> [!TIP]
> This document is still a work in progress.

The decisions for this section are to allow the least amount of friction to the
data exchange while still ensuring data are valid.  

## DateTime

> [!WARNING] TODO
> Should this have some examples?

All API endpoints with date/time properties should require both the date and
time, not just a date. The data should include the timezone/offset. See
[RFC 3339](https://www.rfc-editor.org/rfc/rfc3339) for detailed guidance on
proper formatting of `datetime` type fields.

## Case Sensitivity

> [!WARNING] TODO
> Should this have some examples?

All API routes and query string parameters must be case insensitive to ensure full
compatibility with all backend data sources.

> [!WARNING]
> Details of case sensitivity are still being worked out. Does this
> extend to payloads on PUT/POST? If so, what do we expect on GET payloads, or
> in streaming output - should the property names be normalized to the "correct"
> casing?

## Data Type Validation

> [!WARNING] TODO
> The ODS/API infers data type. That should be allowed but do we
> really want to recommend it? If we recommend this, it may be hard to manage in
> some platforms. If we deny it, then we might cause problems for vendors who
> have been sending the wrong data type. Currently favoring being more strict
> about this.

## Schema Validation

> [!WARNING] TODO
> TAG recommended that we walk this back. Re-evaluate before publishing to the
> `main` branch.

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

> [!NOTE]
> This is a strong recommendation. It is not elevated to a _must_ have
> requirement so that existing version of the Ed-Fi ODS/API do not fall out of
> compliance. This is subject to further refinement in this document drafting
> process.

## API Guidelines Contents

* [Scope](../SCOPE.md)
* [Key Characteristics](../KEY-CHARACTERISTICS.md)
* [Requirement Levels](../REQUIREMENT-LEVELS.md)
* [API Design Guidelines](../API-DESIGN-GUIDELINES/README.md)
  * [Discovery API](DISCOVERY-API.md)
  * [Data Strictness](DATA-STRICTNESS.md)
  * [Resources](RESOURCES.md)
  * [HTTP Verbs](HTTP-VERBS.md)
  * [General Request Construction](GENERAL-REQUEST-CONSTRUCTION.md)
  * [Ed-Fi Descriptors](ED-FI-DESCRIPTORS.md)
  * [Query Operators](QUERY-OPERATORS.md)
  * [Response Codes](RESPONSE-CODES.md)
  * [ETags and Other REST API Conventions and
  Features](ETAGS-OTHER-CONVENTIONS.md)
* [API Implementation Guidelines](../API-IMPLEMENTATION-GUIDELINES/README.md)
