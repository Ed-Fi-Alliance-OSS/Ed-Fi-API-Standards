# Auto-generating Tests Using Portman

> [!WARNING]
> This process might not work well for the entire Resource API, because of the
> amount of time and effort required to make the generated tests useful. That
> work deserves to be handled properly in a well-scoped ticket in the future.
> At this time, this technique is only recommended for smaller API definitions.

[Portman](http://getportman.com/) is a tool for porting converting OpenAPI specs
to Postman and injecting an automated test suite. It can be used to a certain
extent to build basic verification tests for an API specification. However, the
results will likely need a good deal of extra work before they are truly useful.

To run:

1. Must have NodeJs (tested with version 18).
2. Open a  terminal / command prompt
3. Go to the [api-specifications](../api-specifications/) directory.
4. First time usage, run `npm install`.
5. Now, call `npm run testgen -- <path to spec file>`.
6. This will generate an output file called `output.postman.json` in the same
   directory. Move that file into the same directory as the OpenAPI spec file
   and rename to match the OpenAPI spec, but using extension `.postman.json`
   instead of `.yml`.

Example session:

```shell
cd api-specifications
npm install
npm run testgen -- ./discovery-api/discovery-api-spec.yml
mv output.postman.json ./discovery-api/discovery-api-spec.postman.json
```
