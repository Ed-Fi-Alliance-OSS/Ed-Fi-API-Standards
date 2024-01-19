# HTTP Verbs

HTTP verbs communicate actions that can be taken against a resource. Depending
on the verb, the request can require additional information in the body. The
Ed-Fi REST API supports the following verbs:

* **POST** An HTTP POST creates an individual subordinate resource. If
  successful, the URI to the new resource is returned in the "Location" HTTP
  header of the response. Performing a POST with identical natural keys to a
  resource already in the data store _must_ perform an update rather than create
  a new resource (colloquially known as an "upsert"). A POST operation _must
  not_ allow a desired unique identifier to be provided to the REST API. POST is
  a required verb for non-read-only Resources.
* **GET** An HTTP GET returns existing Resources. Providing the natural key or
  internal unique identifier on the URL specifies an individual resource, while
  omitting the natural keys and identifier retrieves the complete set of
  Resources of the given type. GET _must_ be an idempotent operation. GET is a
  required verb.
* **PUT** An HTTP PUT _must_ perform an idempotent update of an existing
  resource. PUT performs a full replacement of the existing resource with the
  supplied value. A PUT against a nonexistent resource _must not_ create a new
  resource under the provided identifier. PUT is a _required_ verb for non-read-only
  Resources.
* **DELETE** An HTTP DELETE deletes an existing individual resource. DELETE is a
  _required_ verb for non-read-only Resources.
* **PATCH** An HTTP PATCH performs a partial update on an existing individual
  resource. For a partial update, only the properties that are submitted will be
  updated on the target resource. The entire patch will be applied, or none of
  it will. The new representation of the entire resource is returned in the
  response body. Due to a lack of industry standard practice in the use of PATCH
  for REST APIs, it is _not recommended_ that implementations support PATCH in
  Ed-Fi API applications.
  
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
