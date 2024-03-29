# Data Strictness

The recommendations for this section are designed to allow the least amount of
friction in the data exchange while still ensuring data are valid.  

## DateTime

All API endpoints with date/time properties _should_ require both the date and
time, not just a date. The data should include the timezone/offset. See
[RFC 3339](https://www.rfc-editor.org/rfc/rfc3339) for detailed guidance on
proper formatting of `datetime` type fields.

Typical examples of validate `datetime` fields:

* `2021-09-28T15:00:00Z`, that is, 3:00 PM in UTC on September 28, 2021.
* `2021-09-28T15:00:00-06:00`, which is 3:00 PM in CST (central standard) on
  September 28, 2021.

## Case Sensitivity

API routes and query string parameters _should_ be case insensitive.

For example, the following pairs pairs should be treated equivalently:

* `/ed-fi/students` and `/ed-FI/stUDeNTs`
* `/ed-fi/students?lastName=John` and `/ed-fi/students?LASTNAME=John`

> [!WARNING]
> Revisit special interest group and survey results. This can be hard to achieve
> in some platforms. Is it really necessary?  Does this extend to payloads on
> PUT/POST? If so, what do we expect on GET payloads, or in streaming output -
> should the property names be normalized to the "correct" casing?

## Data Type Validation

> [!WARNING]
> The ODS/API infers data type. That should be allowed but do we
> really want to recommend it? If we recommend this, it may be hard to manage in
> some platforms. If we deny it, then we might cause problems for vendors who
> have been sending the wrong data type. Currently favoring being more strict
> about this.

## Schema Validation

With respect to a given API Specification document, an Ed-Fi API:

* _Must_ reject any POST or PUT request body that is missing a _required_
  attributes.
* _Must_ accept and store all _required_ and _optional_ attributes in a POST or PUT
  request body, and serve those fields as a valid document in response to GET
  requests.
* _Should_ ignore extra attributes in a POST or PUT request body that are not
  defined in the API specification.
* _Should not_ store extra attributes or include them in response to GET
  requests.

In many API applications, extra attributes would be rejected with status code
400 (BadRequest). In an Ed-Fi API, this allowing these extra attributes is
encouraged so that API clients are more likely to remain compatible across
variations in the API specification. For example:

* A source system vendor operates in two states. One of those states has
  extended `student` to include extra attributes. The vendor is able to send the
  same request body in both cases, knowing that the API application in the state
  without the extension will simply ignore the extra attributes.
* A minor update is made to the Ed-Fi Data Standard, adding an optional attribute
  to a `student`. An API client is programmed to send the new attribute, and
  it continues to interoperate without fail when accessing older API implementations
  that have yet to accept hte minor Data Standard update.

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
* [API Implementation Guidelines](../API-IMPLEMENTATION-GUIDELINES/README.md)
