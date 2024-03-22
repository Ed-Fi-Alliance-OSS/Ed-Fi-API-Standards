# Ed-Fi Resource API (4.0)

The Ed-Fi Resources API enables applications to read and write education data
stored in an Ed-Fi-compatible application through a secure REST interface.

These file represent the core Resource API generated for [Ed-Fi Data Standard
4.0](https://techdocs.ed-fi.org/display/EFDS4X/Ed-Fi+Data+Standard+v4)

* [OpenAPI specification](resources-ds-4.0.yml)

No Postman file has been saved for this specification. The exported file is
close to 20 MB, which is excessive for a Git repository. Furthermore, we do not
have any test automation for that as of yet; the examples in the OpenAPI spec
are insufficient for auto-generating tests. Since there are not tests, there is
no advantage to sharing a Postman file here. Anyone with Postman can import the
OpenAPI specification file directly on their machine.

For local Postman generation, please see
[Import, Cleaning, and Export with Postman](../../docs/IMPORT-EXPORT-POSTMAN.md)
for tips on changing the `baseUrl` variable and setting up token authentication.
