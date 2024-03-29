# Ed-Fi API Specifications

The following specifications files are in OpenAPI 2 (aka Swagger) or OpenAPI 3
format.

* Ed-Fi Discovery API
  * [Discovery API (1.0)](./discovery-api-1.0/) (implemented in all versions of the Ed-Fi ODS/API)
  * [Discovery API (2.0)](./discovery-api-2.0/) (draft proposal)
* Ed-Fi Resource API
  * [Resource API (3.3)](./resources-ds-3.3/) (implemented in Ed-Fi ODS/API v5.3)
  * [Resource API (4.0)](./resources-ds-4.0/) (implemented in Ed-Fi ODS/API v6.x and 7.x)
  * [Resources API (5.0)](./resources-ds-5.0/) (implemented in Ed-Fi ODS/API 7.x)
* Ed-Fi Descriptor API
  * [Descriptor API (3.3)](./descriptor-api-3.3/) (implemented in Ed-Fi ODS/API v5.3)
  * [Descriptor API (4.0)](./descriptor-api-4.0/) (implemented in Ed-Fi ODS/API v6.x and 7.x)
  * [Descriptor API (5.0)](./descriptor-api-5.0/) (implemented in Ed-Fi ODS/API 7.x)

Additionally, there are Postman environment files for some of the supported
"production" services, and some of the specifications have a basic set of
conformance tests for Postman / newman.

## Developer Notes

See [docs](../docs/README.md) for developer notes on generating and managing
OpenAPI specifications and Postman collections.

## OpenAPI Resources

* [Swagger Editor](https://editor.swagger.io/)
* [Swagger UI](https://petstore.swagger.io/) - note that you can paste a json or
  yml spec link from above in here to get a nicely formatted view of the API.
* [OpenAPI Generator](https://openapi-generator.tech/)
