# Ed-Fi Descriptors

Descriptors in the Ed-Fi Data Standard are a set of mechanisms to support
flexible enumerations or code tables. Each Descriptor has the following
attributes:

* namespace
* codeValue
* shortDescription
* description
* effectiveBeginDate
* effectiveEndDate

The GET of a resource _must_ return the namespace and codeValue for Descriptor
enumerations. Other components of the Descriptor can be retrieved from the
Descriptor resource.

The PUT or POST of a resource _must_ specify the namespace and codeValue for
each Descriptor value in a descriptor reference (described below).

## URI Construction and HTTP Verb Usage for Ed-Fi Descriptors

Descriptors are also exposed as Resources of an Ed-Fi REST API and can be
accessed and manipulated as follows:

**Table 3.** Accessing and Manipulating Descriptors

| Resource                 | POST                  | GET                                              | PUT                              | DELETE                           |
| ------------------------ | --------------------- | ------------------------------------------------ | -------------------------------- | -------------------------------- |
| `/[abc]Descriptors`      | Adds a new Descriptor | Gets all Descriptors for the subtype             | Error                            | Error                            |
| `/[abc]Descriptors/{id}` | Error                 | Gets all attributes for an individual Descriptor | Updates an individual Descriptor | Deletes an individual Descriptor |

## Descriptor References

References to a Descriptor value _must_ be constructed in the following format:

```none
uri://[namespace]/[name of descriptor]#[descriptor value]
```

For example, to refer to the academicSubject value in the Ed-Fi namespace
("ed-fi.org") with a codeValue of "Chemistry," the reference would be the
following URI:

```none
uri://ed-fi.org/AcademicSubjectDescriptor#Chemistry
```

Values _must_ be sent as-is and _must not_ be URI or otherwise encoded.

For example, a descriptor whose codeValue has spaces _must_ be sent thus:

```none
uri://ed-fi.org/AcademicSubjectDescriptor#English Language Arts
```

...and _must not_ be sent as, e.g., 

```none
uri://ed-fi.org/AcademicSubjectDescriptor#English%20Language%20Arts
```

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
