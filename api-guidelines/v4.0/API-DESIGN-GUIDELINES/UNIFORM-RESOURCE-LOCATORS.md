# Uniform Resource Locators (URLs)

---

## Work in Progress

Maybe combine with [Request Construction](./REQUEST-CONSTRUCTION.md). Wrote
these two weeks apart and did not initially realize the duplication of topical
areas. Some elements of Request Construction probably belong in the GET page.

---

## URL Path Segments

All URLs _must_ follow this pattern:

```none
[scheme]://[host](:[port])/[as-needed-path-segments]/[model namespace]/[resource name](/[identifier])(?[key=value])
```

* The scheme, host, and port are standard features of HTTP (examples:
  `https://example.com` or `https://example.com:443`).
* These _may_ optionally be followed by any path segments desired by the API
  host. Example: `/api/data/v3`.
* The _required_ model namespace is a feature of an Ed-Fi API; in the core Ed-Fi
  Unifying Data Model, the namespace is always "ed-fi". Other values are used by
  Extensions ot the Data Model.
* Examples of resource names, which are always plural: `students`,
`studentEducationOrganizationAssociations`.
* `PUT` and `DELETE` requests always specify a unique identifier after the
  resource; `GET` request may also specify that identifier.
* Finally, query string key-value pairs may be appended after the rest of the
  URL, prefixed by `?` and separated by `&`. Examples: `?firstName=John`,
  `?limit=200&offset=200`.

Given the potential variability in base paths, an API client should utilize the
[Discovery API](./ED-FI-DISCOVERY.md) to construct the full path segment
preceding the namespace component.

## Case Sensitivity

API routes and query string parameters _should not_ be case sensitive, although
this may be not be realistic in some API frameworks and case sensitivity may be
required.

For example, the following pairs pairs should be treated equivalently:

* `/ed-fi/students` and `/ed-FI/stUDeNTs`
* `/ed-fi/students?lastName=Doe` and `/ed-fi/students?LASTNAME=Doe`

This extends to query string values, not only the keys. Thus it is preferred
that the query strings `?lastname=DOE` and `?lastname=Doe` return the same
results. While this is the default behavior in some database, it may be
difficult to achieve in others. Thus this is not a required feature.
