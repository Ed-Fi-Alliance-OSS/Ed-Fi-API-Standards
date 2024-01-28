# Query Operators

An Ed-Fi API _should_ support searching capabilities with the possible use of
filters, paging, and ordering. These are discussed below.  Information about all
supported capabilities, their configurations, and their default values should be
available from the API Metadata.  

## Search

An Ed-Fi API _should_ support querying capabilities when searching a collection
of Resources. Query operators are applied to the query string using the
following format: `{collectionURI}?{propertyName}={value}`. Additional
`{propertyName}={value}` terms may be appended after an ampersand, &.

For example, to search all available Students having the first name "John" and
surname "Smith":

```none
https://api.example.com/v3/ed-fi/students?firstName=john&lastSurname=Smith
```

The Open API specification documents will show which properties are searchable.
When a searchable parameter is nested under another object in the JSON
payload, the Ed-Fi API specifications require the query string to use only the
parameter name _without_ dereferencing the parent element name. For example, the
JSON document for a `studentSchoolAssociation` includes `schoolId` nested below
`schoolReference`. In the JsonPath query language, the query for a particular
schoolId would then be `?schoolReference.schoolId=xyz`. However, the Ed-Fi API
specifications predate and do not use JsonPath. Instead, they make a simplifying
assumption that `schoolId` by itself is uniquely searchable without
dereferencing the parent entity, `schoolReference`. Thus, the correct HTTP query
term is therefore simply `?schoolId=xyz`.


## Selectors

Selectors allow application developers to be more selective about how much data
is returned in the resource representations. Implementation of selectors in an
Ed-Fi REST API is optional (i.e., a _should_ requirement).

**Table 5.** Selectors Allow Increased Selectivity in the Data that is Returned

| Parameter | Description                               |
| --------- | ----------------------------------------- |
| fields    | Limits the response to the fields listed. |

For example, to retrieve only a Student's first and last names:

```none
https://api.example.com/v1/students/{id}?fields=firstName,lastSurname
```

In addition, the fields selector _should_ be implemented to allow for deep
selection by allowing for properties on (sub-)objects within the API resources
to also be specified using parentheses to indicate the properties on that object
to provide, following this example:

```none
/students?fields=firstName,addresses(latitude,longitude)
```

This query string value would return the first name and the address collection,
but only provide the latitude and longitude properties on that address
collection.

## Paging

Paging is a mechanism that restricts the number of results returned by an
operation and has proven critical to the efficient usage of Ed-Fi APIs.  The
limit parameter _should_ be supported in the query string and allow the client
to set the maximum number of records to return. If no value is supplied, the
limit parameter _should_ default to 25.

The offset parameter _should_ be available to the client to specify how many
records to skip when getting the result set. The value for offset _should_
default to 0.  However, some API hosts may wish to disable offset-based
searching for performance and reliability reasons.

When multiple records are being returned, the total count of all
records _should_ be returned, as part of the HTTP header information.

For example, to get the first name and last name of a collection of available
Students from positions 31 to 40:

```none
https://api.example.com/v1/students?fields=firstName,lastSurname&limit=10&offset=30
```

Alternate approached to paging could be implemented, for example using keyset or
cursor-based parameters.

## Ordering

Ordering is a mechanism that sorts the objects returned by one or more of the
fields returned.  Ordering could be implemented with the use of URL query string
parameters. If implemented, it must:

* Take directionality as a parameter (ascending/descending).
* Respect paging parameters.
* Guarantee that repeated queries with the same order and paging parameters,
  when no data changes have occurred, reliably return the same sequence of
  objects.

For example, an Ed-Fi API could return all Student School Associations from
smallest School Id to largest with the following URL fragment:

```none
/ed-fi/studentSchoolAssociations?orderBy=SchoolId&direction=desc.
```

As an experimental feature, this example is merely one possibility, rather than
a prescribed approach.

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
