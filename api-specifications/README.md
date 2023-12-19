# Ed-Fi API Specifications

The following specifications files are in OpenAPI 2 (aka Swagger) or OpenAPI 3
format.

* [Ed-Fi Discovery API](./discovery-api/)

Additionally, there are Postman environment files for the supported "production"
services, and some of the specifications have a basic set of conformance tests
for Postman / newman.

## Automating Compliance Testing with Postman

Importing an OpenAPI specification into Postman is trivial, but it does not
create compliance tests for you automatically. For that, we can use
[portman](https://github.com/apideck-libraries/portman).

```shell
npm install
npm run testgen -- metadata-api-spec.yml
```

The output file will simply be `output.postman.json`, and it should be renamed
to match the API specification file before committing it. However, Portman does
not arrange its contract tests into folders, as Postman does. Folders improve
the organization in Postman. Therefore, it may be beneficial to import the
original spec file into Postman - thus giving you the folders - and also import
the `output.postman.json` file side-by-side. Either re-arrange the Portman-based
collection to match the one from the Postman import, or copy the tests from the
Portman collection into the Postman-based collection. Re-export that collection
into this folder with a name matching the spec file, e.g.
`metadata-api-spec.postman.json`.

## OpenAPI Resources

* [Swagger Editor](https://editor.swagger.io/)
* [Swagger UI](https://petstore.swagger.io/) - note that you can paste a json or
  yml spec link from above in here to get a nicely formatted view of the API.
* [OpenAPI Generator](https://openapi-generator.tech/)
