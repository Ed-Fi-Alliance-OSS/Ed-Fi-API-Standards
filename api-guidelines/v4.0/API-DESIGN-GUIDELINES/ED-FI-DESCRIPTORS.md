# Ed-Fi Descriptors

Descriptors in the Ed-Fi Data Standard are a set of mechanisms to support
flexible enumerations or code sets. The following table defined the attibuts of an Ed-Fi Descriptor:

| Attribute                | Return from GET | Needed for PUT/POST   | Notes                                                        |
| ------------------------ | --------------- | -------------------- | ------------------------------------------------------------ |
| namespace                | required        | required             |                                                              |
| codeValue                | required        | required             | Value should be human readable, e.g. prefer one or more words over a numeric code or random string.|
| shortDescription         | required        | required             |                                                              |
| description              | required        | optional             | Longer description that may contain additional normative usage guidance.|
| effectiveBeginDate       | required        | optional             | Date for display only, not validation                        |
| effectiveEndDate         | required        | optional             | Date for display only, not validation                        |
| id                       | required        | Must not have on POST <br /> required on PUT|                                                              |
| _etag                    | required        | optional             |                                                              |
| xyzDescriptorId          | optional        | Must not have        | Retained only for backward-compliance with existing ODS/API implementations.|
| priorDescriptorId        | optional        | optional             | Retained only for backward-compliance with existing ODS/API implementations.|
| lastModfiedDateTime      | optional        | Must not have        | Date for display only, not validation                        |

## URI Construction and HTTP Verb Usage for Ed-Fi Descriptors

Descriptors are also exposed as Resources of an Ed-Fi API and can be
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

## Ed-Fi Descriptor API

An Ed-Fi API _should_ expose all Descriptors via a REST interface, using the
[standard verbs](./HTTP-VERBS.md). However, it is _recommended_ that most API
clients be granted read-only access to these Descriptors, so that they may
ascertain the full set of values available in the given implementation.

Furthermore, it is _recommended_ that access to the POST verb be granted only in
carefully controlled situations, as most API clients will not need to have this
access. The community best practice is to avoid modifying or deleting existing
Descriptors; thus an Ed-Fi API might not provide PUT or POST capability on
Descriptors. However, the PUT verb might be used to modify the `endDate` on an
existing Descriptor, or clarify the `description`, without otherwise modifying
the `namespace` and `codeValue`.

Also see: [Descriptor API
specification](../../../api-specifications/descriptor-api).

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
