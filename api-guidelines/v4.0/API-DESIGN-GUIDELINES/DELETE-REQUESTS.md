# DELETE Requests

An HTTP DELETE deletes an existing individual resource. DELETE is a _required_
verb for non-read-only Resources.

## Request URLs

The URL _must_ end with a unique identifier for the given item to be replaced. A
`DELETE` request _could_ support query string parameters in the form of the natural
key that uniquely identifies the item to be modified.

> [!NOTE]
> The Ed-Fi Alliance's ODS/API Platform does not support deletes by natural key,
> though other implementations could choose to do so.

## Request Headers

If the application supports [ETags](./REST-API.md#etags), then the application
_must_ support the `If-Match` request header. Example:

`If-Match: 5250159352800270276`

## Request Body

`DELETE` requests do not have a request body.

## Response Body

`DELETE` requests do not have a response body.

## Applicable Status Codes

The following status codes _must_ be supported for `POST` responses:

| Status Code | Meaning      | When to Use                                                                                        |
| ----------- | ------------ | -------------------------------------------------------------------------------------------------- |
| 200         | OK           | The item was updated ("upsert").                                                                   |
| 201         | Created      | The item was created.                                                                              |
| 401         | Unauthorized | The request requires authentication. The OAuth bearer token was either not provided or is invalid. |
| 403         | Forbidden    | The request cannot be completed in the current authorization context.                              |
| 404         | NotFound     | The resource could not be found.                                                                   |
| 405         | Not Allowed  | Special cases where a verb is not allowed, for example in a read-only API.                         |
| 409         | Conflict     | The request cannot be completed because it would result in an invalid state.                       |
| 500         | Server Error | An unhandled error occurred on the server.                                                         |

If ETags are supported, then code `412` (Precondition Failed) _must_ also be
supported, when the item's current server-side ETag value does not match the
supplied If-Match header value in the request.

Other [HTTP status codes](./REST-API.md#status-codes) may be used as needed for
specific situations.

## API Guidelines Contents

* [Scope](../SCOPE.md)
* [Key Characteristics](../KEY-CHARACTERISTICS.md)
* [Requirement Levels](../REQUIREMENT-LEVELS.md)
* [API Design Guidelines](../API-DESIGN-GUIDELINES/README.md)
  * [Resources](RESOURCES.md)
    * [Validation of Natural and Foreign Keys](./NATURAL-FOREIGN-KEYS.md)
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
