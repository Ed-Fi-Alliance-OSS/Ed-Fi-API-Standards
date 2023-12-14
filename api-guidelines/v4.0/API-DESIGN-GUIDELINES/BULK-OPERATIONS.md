# Bulk Operations

Bulk operations may be implemented for the purpose of moving a large amount of
data at one time without the overhead of individual calls for each resource.

Externally administered test scores (for example, SAT, ACT, or statewide
standardized tests) are often received as data files (not individual
transactions) and thus will require bulk loading. An online assessment product
may not support transactional updates through an Ed-Fi REST API from within
their software, and will instead find it necessary to supply a bulk upload of
data at regular intervals. Even SIS products that do support transactional
updates may still find it useful to do a one-time bulk loading of data when
initially connecting to a data store through an API.

Bulk export operations are useful when clients need an efficient way to obtain a
large data set from the hosting platform. Export is also useful where clients of
the API host need data files to adhere to a particular format (e.g., exchange
with another system that is not capable of its own direct exchange with the API
host).

## Bulk Operation Recommendations

When bulk operations are supported through an Ed-Fi REST API, consider the
following guidelines:

* **File Management.** Bulk operations work on files that can take a non-trivial
  amount of time to load or, in the case of an export, for the server to
  generate. A bulk import operation API _should_ allow files to be uploaded
  incrementally and for file uploads to be resumed.
* **Control Protocol.** Bulk operations implemented as part of an Ed-Fi REST API
  _should_ be managed over HTTPS.
* **File Transfer.** Using HTTPS for file transfer is _recommended_ for
  consistency with the REST portion of the API as well as with the encryption.
  Other secure protocols may be used.
* **Status.** Status information _should_ be available (pulled via GET) or
  provided (pushed via POST) regarding the progress and results of batch
  operations.

## Bulk Packet Format

Bulk data exchange performed through Ed-Fi REST API operations leverage
interchanges built from the Ed-Fi Core XML Schema.

XML documents are considered well formed when they conform to the XML
specification, they are considered valid when they can be validated against a
specific XML schema, and they are considered correct when the contained
information may be correctly interpreted by other systems. For the bulk loading
operations of an Ed-Fi REST API, correct implies that applicable references to
Resources exist at the conclusion of a bulk load operation.

XML is the recommended  packet format for the bulk operations of an Ed-Fi REST
API. It is further recommended that the Ed-Fi Standard Interchange Schemas be
used where appropriate. Where applicable, bulk exports of data should align with
bulk loading formats to enable a round trip of information.

## API Guidelines Contents

* [Scope](../SCOPE.md)
* [Key Characteristics](../KEY-CHARACTERISTICS.md)
* [Requirement Levels](../REQUIREMENT-LEVELS.md)
* [API Design Guidelines](../API-DESIGN-GUIDELINES/README.md)
  * [Resources](RESOURCES.md)
  * [HTTP Verbs](HTTP-VERBS.md)
  * [General Request Construction](GENERAL-REQUEST-CONSTRUCTION.md)
  * [Ed-Fi Descriptors](ED-FI-DESCRIPTORS.md)
  * [Bulk Operations](BULK-OPERATIONS.md)
  * [Query Operators](QUERY-OPERATORS.md)
  * [Response Codes](RESPONSE-CODES.md)
* [ETags and Other REST API Conventions and
  Features](ETAGS-OTHER-CONVENTIONS.md)
* [API Implementation Guidelines](../API-IMPLEMENTATION-GUIDELINES/README.md)
