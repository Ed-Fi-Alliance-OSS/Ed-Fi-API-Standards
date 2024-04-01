# GET Requests

An HTTP `GET` returns existing Resource item(s). `GET` _must_ be an idempotent
operation. `GET` is a required verb.

## Request URLs

A `GET` request can specify either a collection of items or a specific item:

* `GET /ed-fi/students` with or without query string parameters retrieves a list
  ("GET All" queries).
* `GET /ed-fi/students/[identifier]` retrieves a specific student ("GET by ID").

If the natural key is provided as a query string, then the result will be a
collection containing only one item.

An Ed-Fi API _should_ support searching capabilities with the possible use of
filters, paging, and ordering. These are discussed below.  Information about all
supported capabilities, their configurations, and their default values should be
available to client applications via the [Discovery API](./DISCOVERY-API.md).

### Search

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

### Paging

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

Alternate approaches to paging could be implemented, for example using keyset or
cursor-based parameters.

> ![WARNING] REVISE THIS
> This change from must to should for the limit and offset parameters is in
> recognition of significant performance degradation in many database platforms,
> when using this technique beyond around 10,000 records. Some implementations
> may wish to disable the functionality to avoid excessive database usage.**

### Ordering

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

## Request Body

`GET` requests do not have a request body.

## Request Headers

> [!NOTE]
> TODO: profiles, snapshot

## Response Body

A "GET by ID" request _must_ return a single item if the identifier exists, or
nothing if the identifier is not found. In the latter case, the request must use
status code 404 (NotFound).

A "GET All" query must return a _collection_, even if the collection is empty.
In a JSON response, an empty collection is represented by `[]`.

### Metadata

The items in a `GET` response body _could_ be modified to include the following metadata:

* A `link` construction for references, which describes the relationship of the
  reference and provides a locator that can be used to construct the URL to the
  referenced item.
* _Last modified date_
* _[ETag](./REST-API.md#etags)_ value
* _Provenance_ describing the source and/or modifications to the data.

The following example demonstrates all forms of metadata.

```json
  {
    "id": "986a44e7cfcf4019b7b2ea4a640c6d20",
    "schoolReference": {
      "schoolId": 255901107,
      "link": {
        "rel": "School",
        "href": "/ed-fi/schools/2af36358c7824afe8b3b88aea077c172"
      }
    },
    "schoolYearTypeReference": {
      "schoolYear": 2022,
      "link": {
        "rel": "SchoolYearType",
        "href": "/ed-fi/schoolYearTypes/337ea29f861b4ee88fa9714863cc0b98"
      }
    },
    "calendarCode": "2010605675",
    "calendarTypeDescriptor": "uri://ed-fi.org/CalendarTypeDescriptor#Student Specific",
    "gradeLevels": [],
    "_etag": "5250159352800270276",
    "_lastModifiedDate": "2024-03-29T18:23:57.2882372Z",
    "_provenance": {
        "sourceSystem": "Example SIS",
        "modifications": []
    }
  }
```

> [!NOTE]
> At the time of publication, _provenance_ is a new proposal that has not been
> implemented in any known Ed-Fi API applications. The example provided above
> is not a required format for optional provenance information.

## Response Headers

There is no restriction on the standard or custom response headers that may be
included in the response to a `GET` request. A special header of note: an Ed-Fi
API _should_ provide a query string option for retrieving a count of all items
in the collection, which is useful in determining if there are additional pages
of data to retrieve. The _recommended_ approach is use query string
`?totalCount=true` and respond with an additional header, `total-count:
[number]` (example: `total-count: 1234`).

## Standard Status Codes

The following status codes _must_ be supported for `GET` responses:

| Status Code | Meaning      | When to Use                                                                                                 |
| ----------- | ------------ | ----------------------------------------------------------------------------------------------------------- |
| 200         | OK           | All successful requests, including "GET All" when there are no results to list                              |
| 401         | Unauthorized | The request requires authentication. The OAuth bearer token was either not provided or is invalid.          |
| 403         | Forbidden    | The request cannot be completed in the current authorization context.                                       |
| 404         | Not Found    | The resource could not be found. If Use-Snapshot header is set to true, the snapshot may have been removed. |
| 500         | Server Error | An unhandled error occurred on the server.                                                                  |

Code 304 (Not Modified) also _must_ be supported _if_ the application supports
[ETags](./REST-API.md#etags). Other [HTTP status
codes](./REST-API.md#status-codes) may be used as needed for specific
situations.

## API Guidelines Contents

* [Scope](../SCOPE.md)
* [Key Characteristics](../KEY-CHARACTERISTICS.md)
* [Requirement Levels](../REQUIREMENT-LEVELS.md)
* [API Design Guidelines](../API-DESIGN-GUIDELINES/README.md)
  * [Resources](RESOURCES.md)
  * [Discovery API](./DISCOVERY-API.md)
  * [Ed-Fi Descriptors](./ED-FI-DESCRIPTORS.md)
  * [REST API Conventions](./REST-API.md)
    * [Uniform Resource Locators (URLs)](./UNIFORM-RESOURCE-LOCATORS.md)
    * [Data Strictness](./DATA-STRICTNESS.md)
    * [POST Requests](./POST-REQUESTS.md)
    * [GET Requests](./GET-REQUESTS.md)
    * [PUT Requests](./PUT-REQUESTS.md)
    * [DELETE Requests](./DELETE-REQUESTS.md)
* [API Implementation Guidelines](../API-IMPLEMENTATION-GUIDELINES/README.md)
