# Converting Swagger 2 to Open API 3

To convert Swagger 2 JSON files from the ODS/API to OpenAPI Specification v3
format:

1. Open the [online Swagger Editor](https://editor.swagger.io/).
2. Click on `File > Import File` or `File > Import URL` to load the Swagger
   file.
3. Once the file loads, click on `Edit > Convert to Open API 3`.
4. Click through a couple of buttons that will popup in the page.
5. Save the file locally with `File > Save as YML`.

## Optimization

The files coming from the ODS/API are not well optimized. With a little
find-and-replace logic, we can significantly cut down the file size.

In the `components.responses` section, make sure all possible responses are described _except_ `Success` (because this one will have a different response type for each endpoint).

Then replace the individual response content. Need to use a good find-and-replace editor like the one in VS Code. Examples:

Replace

```yml
        "500":
          description: An unhandled error occurred on the server. See the response
            body for details.
          content: {}
```

with

```yml
        "500":
          $ref: "#/components/responses/Error"
```

Replace

```yml
        "410":
          description: Gone. An attempt to connect to the database for the snapshot
            specified by the Snapshot-Identifier header was unsuccessful (indicating
            the snapshot may have been removed).
          content: {}
```

with

```yml
        "410":
          $ref: "#/components/responses/Gone"
```

and so on.

Similarly, we can create common parameters. Most of the parameters that are commonly used (offset, limit, change versions) are listed in the `parameters` section already, but these are not referenced. Need to add `Snapshot-Identifier` to that list and update the resource paths to reference these parameters instead of duplicating their definitions. Also can add `id` and `IfMatch`.

Replace

```yml
      - name: offset
        in: query
        description: Indicates how many items should be skipped before returning results.
        schema:
          type: integer
          format: int32
          default: 0
```

with

```yml
      - $ref: "#/components/parameters/offset"
```

These modifications shrank the resources-ds-3.3.yml file down by roughly 33%.

> [!WARNING]
> Don't forget to check the modified file for correctness, for example
> using the [online swagger editor](https://editor.swagger.io/).
