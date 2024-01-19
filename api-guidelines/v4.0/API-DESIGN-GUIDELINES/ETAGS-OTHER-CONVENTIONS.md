# ETags and Other REST API Conventions

ETags (Entity Tags)[\[9\]](#f9)are mechanisms used to support optimistic
concurrency and efficient bandwidth handling. The use of ETags is _recommended_
for Ed-Fi REST API implementations.

## Other REST API Conventions and Features

Four additional REST API features — case sensitivity, encryption, version, and overposting —
are discussed in the following table.

**Table 7.** Additional REST API Features

| REST Feature     | Ed-Fi Implementation                                                                       | Explanation                                                                                                                        |
| ---------------- | ------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| Case Sensitivity | `/students?firstName=JOHN` or <br /> `/students?FIRSTNAME=John`                                        | URIs, parameter names, and parameter values _must not_ be case sensitive. The two URI’s to the left will produce the same results. |
| Version          | `https://api.example.com/v3/ed-fi/students` | The API standard version number _could_ be specified in the base URI, but is not required.  Note that `v3` here is for the API standard version number, which is represented by these guidelines.                                                               |                                                              |
-----

<a name="f9"></a>9. For more information on ETags, see IETF RFC 7232, Section
2.3 [here](https://tools.ietf.org/html/rfc7232#section-2.3).
 
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
