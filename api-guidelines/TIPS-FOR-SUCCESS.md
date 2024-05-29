# Tips for Success in Building an Ed-Fi Compatible API

An Ed-Fi API application combines REST API principles with the Ed-Fi Data
Standard to create a platform for exchange of K-12 educational data. In the
main, client applications connecting to an Ed-Fi API implementation should not
"know" whose backend service they are connecting with. The [API
Guidelines](./README.md) combined with the [API
specifications](../api-specifications/README.md) provide the information needed
to build an API service that supports interoperability both in the sense of the
_shape_ of the data and in the _mechanism_ of exchange.

This document provides brief practical advice and highlights of normative
aspects of these Guidelines that are sometimes overlooked by developers of both
API hosts and client applications. Please look to the [Guidelines](./README.md)
for additional explanations and context for these topics.

## Case Sensitivity

* The _properties_ of a document _should_ be treated as case sensitive in
  document modification requests (`POST` and `PUT`) and _must_ use proper casing
  in responses to `GET` requests.
* The _values_ for those properties _should not_ be case sensitive.
* URLs, including query string properties and values, _should not_ be case
  sensitive.

### OAuth Token Requests

OAuth 2.0 specifies that the token request should be case sensitive. But the
Ed-Fi ODS/API Platform treats it as case insensitive, as an accidental legacy of
using the default JSON serialization in the .NET Framework.

Furthermore, for many years the documentation provided code examples with bad
casing, such as using `Grant_Type` instead of `grant_type`. These examples have
now been corrected.

But, this means that there is widespread potential for existing API clients to
be using the incorrect case. If your OAuth provider is correctly case sensitive,
then these API clients will not be able to authenticate. _At this time, it is up
to the implementation provider to decide if this is acceptable or not_. It may
be worth noting that Amazon Cognito and Microsoft Entra, for example, are also
relaxed about this casing, accepting `Grant_Type`. Similarly, there may be some
confusion around whether to use "Bearer" or "bearer" in the Authentication
header.

## Three Required API Specifications

See [API Specifications](../api-specifications/) for the currently-supported
specifications, in the form of Open API documents. A compatible application
needs to incorporate at least these three API definitions:

1. **Discovery API**, which provides metadata about the installation, including
   which data model(s) are implemented, OAuth token URL, and the base path for
   accessing Ed-Fi resources and descriptors.
2. **Descriptors API**, which supports queries and modifications of Ed-Fi
   Descriptors.
3. **Resources API**, which supports queries and modifications for Ed-Fi
   Resources - that is, the core entities defined by the Ed-Fi Data Standard.

Other optional specifications, not yet documented in this space, include: Change
Queries, Identity, Enrollment, and others.

Be sure to match the Descriptors and Resources API versions. For example _do not_
accidentally use Descriptors API version 4.0 with the Resources API version 5.0.

## Data Types Inference

Some API clients may be transmitting string values for Boolean or Numeric data
types (e.g. `"1"` for `1` or `"true"` for `true`). The Ed-Fi ODS/API Platform
infers the proper value from these strings. This data type coercion is _not_
recommended, and responses to `GET` requests _must_ have the proper data type.

## GET Response Metadata

All metadata properties in `GET` responses are _optional_.

Inclusion of the following properties is _recommended_:

* `_lastModifiedDate`
* `_etag`

Use of the `link` metadata property is _no longer recommended_.

A `_lineage` property has been introduced and its use is _recommended_ in new
API implementations.
