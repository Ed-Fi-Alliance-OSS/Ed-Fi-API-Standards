# Data Strictness
The decisions for this section are to allow the least amount of friction to the
data exchange while still ensuring data are valid.  

## DateTime

All API endpoints with date/time properties should require both the date and
time, not just a date.  The data should include the timezone and should always
be transmitted in ISO format.

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
