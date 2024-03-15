# RFC Ed-Fi API Guidelines Revision 4.0

## Synopsis

This RFC describes a set of proposed changes to the Ed-Fi API Design &
Implementation Guidelines v3.1.

The Ed-Fi API Design & Implementation Guidelines (herein after referred to as
the "Guidelines") describe requirements for an Ed-Fi representational state
transfer (REST) application programming interface (API). The guidelines describe
the properties to which an API specification and related implementation must
adhere in order to be considered aligned to Ed-Fi technology standards. They do
not describe a specific implementation technology.

## Proposed Changes

This RFC describes a set of proposed changes to the Ed-Fi API Design &
Implementation Guidelines v3.1.

The proposed changes are intended to ensure the establishment of API standards
that can be relied upon when building or using an Ed-Fi compliant API. To
accomplish this the proposed changes are listed here for comment.

## Document Wide Changes

Any references to a specific implementation of an Ed-Fi API or to an Ed-Fi REST
API should be replaced with "an Ed-Fi API."

## Summary of Modifications

* [Discovery API](v4.0/API-DESIGN-GUIDELINES/DISCOVERY-API.md): new section
* [Data Strictness](v4.0/API-DESIGN-GUIDELINES/DATA-STRICTNESS.md): new section
* [Ed-Fi Descriptors](v4.0/API-DESIGN-GUIDELINES/ED-FI-DESCRIPTORS.md)
  * Now requires implementation of the Descriptor API and explains the expected
    schema.
* [REST API Conventions](v4.0/API-DESIGN-GUIDELINES/REST-API.md):
  * Merger of the old "HTTP Verbs" and "ETags and Other Conventions" pages.
  * Change all usage of "UUID" to the more generic "unique identifier" or
    "identifier", so that the implementation is free to use any scheme, not just
    a UUID, as the identifier for a specific resource.
  * Remove notes on encryption, which is an operational concern rather than a
    design topic.
* [Request Construction](v4.0/API-DESIGN-GUIDELINES/REQUEST-CONSTRUCTION.md)
  * Merger of old "General Request Construction" and "Query Operators" pages.
  * Provide greater clarity on standard and optional path segment requirements.
  * Explains which fields are usually made available for searching, and
    conventions for searching in nested documents.
  * Move URL / query string case sensitivity comments from old "ETags and Other
    Conventions" page.
  * Section on paging introduces the idea of keyset/cursor-based paging to
    complement limit/offset paging and relaxes the requirement to support
    limit/offset.
  * Add a proposal for indicating [sort
    order](v4.0/API-DESIGN-GUIDELINES/REQUEST-CONSTRUCTION.md#ordering).
  * Remove "selectors" paragraph.
* "Response Codes" renamed to [Responding to HTTP Requests](v4.0/API-DESIGN-GUIDELINES/RESPONSES.md)
  * Added more standard status codes.
  * Added recommend error response formatting.

## Deleted Sections

Ed-Fi API applications are intended for use by systems, not end users. Therefore
the following pages have been removed:

* User Authorization
* User Claims
* User Roles
* External User Authorization
* Internal User Authorization

The Bulk Operations section has also been removed, as it was a feature of a
particular application, and not a prescribed part of the Ed-Fi API Standards.
