# POST Requests

An HTTP POST creates an individual subordinate resource. If successful, the URI
to the new resource is returned in the "Location" HTTP header of the response.
Performing a POST with identical natural keys to a resource already in the data
store _must_ perform an update rather than create a new resource (colloquially
known as an "upsert"). A POST operation _must not_ allow a desired unique
identifier to be provided to the REST API. POST is a required verb for
non-read-only Resources.

## Request URLs

## Request Body

`GET` requests do not have a request body.

## Response Body

## Applicable Status Codes
