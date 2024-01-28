# Data Strictness

The decisions for this section are to allow the least amount of friction to the
data exchange while still ensuring data are valid.  

## DateTime

All API endpoints with date/time properties should require both the date and
time, not just a date. The data should include the timezone/offset. See 
[RFC 3339](https://www.rfc-editor.org/rfc/rfc3339) for detailed guidance on
proper formatting of `datetime` type fields.

## Case Sensitivity

All API endpoints and queries must be case insensitive to ensure full
compatibility with all backend data sources.

## Schema Validation

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
