# General Request Construction

For all Ed-Fi API transactional requests and responses, JSON _must_ be the
default format. If a media-type header is not provided, "application/json" is
presumed. Alternate packet formats _may_ also be supported.

## Resource Collections and Individual Resources

For each resource, there are two base forms for the URI: one for a collection of
resources and the other for a specific resource in the collection. The
collection form for the URI is referred to by the pluralized name of the
individual resource. A specific resource is referenced by the collection name,
followed by a slash and the resource's unique identifier. For example:

* `/students` refers to a collection of students
* `/students/ffc0…a272` refers to a specific student with an assigned identifier
  of `ffc0…a272`.

## URI Construction and HTTP Verb Usage for Individual Records and Transactions

Individual record and transaction URIs in an Ed-Fi API take a convention-based
approach to construction and HTTP verb usage.

**Table 2.** Example of Convention-Based Approach to Construction

| Resource         | POST               | GET                           | PUT                           | DELETE                        |
| ---------------- | ------------------ | ----------------------------- | ----------------------------- | ----------------------------- |
| `/students`      | Adds a new Student | Gets a collection of Students | Error                         | Error                         |
| `/students/{id}` | Error              | Gets an individual Student    | Updates an individual Student | Deletes an individual Student |

  
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
  * [Bulk Operations](BULK-OPERATIONS.md)
  * [Query Operators](QUERY-OPERATORS.md)
  * [Response Codes](RESPONSE-CODES.md)
  * [Encryption](ENCRYPTION.md)
  * [ETags and Other REST API Conventions and
  Features](ETAGS-OTHER-CONVENTIONS.md)
* [API Implementation Guidelines](../API-IMPLEMENTATION-GUIDELINES/README.md)
