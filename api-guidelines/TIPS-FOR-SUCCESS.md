# Tips for Success in Building an Ed-Fi Compatible API

_March, 2024_

An Ed-Fi API application combines REST API principles with the Ed-Fi Data
Standard to create a platform for exchange of K-12 educational data. In the
main, client applications connecting to an Ed-Fi API implementation should not
"know" whose backend service they are connecting with. The [API
Guidelines](./README.md) combined with the [API
specifications](../api-specifications/README.md) provide the information needed
to build an API service that supports interoperability both in the sense of the
_shape_ of the data and in the _mechanism_ of exchange.

The API Guidelines are currently being revised (target release: second quarter
of 2024) to capture lessons learned and codify some of the implementation
details from the canonical example: the Ed-Fi ODS/API. The tips below call out a
few of the upcoming changes, common "gotchas", and potentially overlooked
features.

## Deletions from the API Guidelines

Ignore statements about _XML and bulk loading_. An Ed-Fi API application does
not need to support bulk data exchange or use of XML. "Ed-Fi 1.0" was based on
exchange of XML files. Some organizations may still be producing XML files based
on the Ed-Fi Data Standard. The Ed-Fi Alliance produces a [client
application](https://edfi.atlassian.net/wiki/x/sQCFAQ)
that will process XML files and upload them through the REST API when needed.
Ultimately, the Ed-Fi Alliance feels that direct integration with the API is a
much stronger approach to interoperability than asynchronous, batch, file
exchange.

Ignore statements about _end-users_. An Ed-Fi API application typically does not
directly support end-users. It is instead intended for system-to-system
interaction. Any organization might choose to add authentication flows that
would enable end-user support, but this is not a requirement.

## Relaxed Requirements

The doc mentions using _UUID's as identifiers_. That statement will be relaxed,
though every resource should still be assigned a unique identifier. It just
doesn't have to be a UUID.

The existing guidance on Ed-Fi Descriptors _discourages_ creating an API
interface. This is contrary to the Ed-Fi Alliance's actual practices. The
alternative would be loading Descriptors directly in the database. We do not
encourage the practice of loading data directly into the database, bypassing the
API.

## Case Sensitivity

### JSON Payloads for Ed-Fi Resources

Case sensitivity is a delicate topic and we have not decided how to handle this
yet. The current application is not case sensitive, being built in C#, and this
can be problematic for applications that assume case sensitivity. The Guidelines
technically only talk about sensitivity for URL parameters, not for the body.
It is **best to treat the schema as case sensitive**.

This tip is especially relevant when storing and sharing raw JSON. If you store
a JSON payload as received in a POST or PUT request, and re-share that exact
payload in a GET request, then accepting an improperly-cased payload could cause
problems for downstream systems that are expecting strict casing.

### JSON Payloads for OAuth

OAuth 2.0 specifies that the token request should be case sensitive. But the
ODS/API treats it as case insensitive, as an accidental legacy of using the
default JSON serialization in the .NET Framework.

For many years our documentation provided code examples with bad casing, such as
using `Grant_Type` instead of `grant_type`. These examples have now been
corrected. But, this means that there is potential for existing API clients to
be have `Grant_Type`. If your OAuth provider is correctly case sensitive, then
these API clients will not be able to authenticate. _At this time, it is up to
you to decide if that is acceptable or not_. It may be worth noting that Amazon
Cognito and Microsoft Entra, for example, are also relaxed about this casing,
accepting `Grant_Type`. Similarly, there may be some confusion around whether to
use "Bearer" or "bearer" in the Authentication header.

## Three Required API Specifications

See [API Specifications](../api-specifications/) for the currently-supported
specifications, in the form of Open API documents. A compatible application
needs to incorporate at least these three API definitions: Discovery API,
Descriptors API, and Resources API. (Other optional specifications, not yet
documented in this space, include: Change Queries, Identity, Enrollment, and
others).

### Discovery API

The JSON document returned by the root `/` endpoint is now called the Discovery
API, encompassing also the dependencies and metadata sections. An Ed-Fi API
needs to implement this same schema, which has just recently been captured in
the form of an Open API document.

Some client applications might not be fully exploiting the Discovery API; they
may be hard-coding some of the URLs that are discoverable in this API. The Ed-Fi
Alliance encourages all client apps to use this API to discover the base URL for
Resources, the OAuth token URL, etc.

While it is not necessary to reproduce the exact URL path segments used by the
ODS/API, please be warned that many applications might not be ready for using
something different. For example, the Ed-Fi ODS/API uses `/data/v3/` in the URL,
and this is not required. The one portion that _is_ required is the data model
namespace, e.g. `/ed-fi/`

### Descriptors API

Descriptors have a simple and common payload and they should be loaded into the
application through the API interface. Organizations may have varying
requirements for authorization. For example, an organization might choose to let
their SIS vendor control their own Descriptors, or the organization might lock
those down for only their own staff to modify. Many organizations do not modify
or delete Descriptors (except to set an end date).

### Resources API

The core set of entities from the Ed-Fi Data Standard are collectively
represented under the banner of the "Resources API". The Ed-Fi ODS/API Platform
supports the entire breadth of the Resources API. There are a few "profiles" of
the Resources API, which isolate specific domains; :exclamation: these profiles
are currently offline and will soon be loaded into this code repository. An
Ed-Fi compatible API does not necessarily need to implement the entire surface
of the Resources API. However, there is some danger in trying to pick and choose
which resources to support. Please tread cautiously when decomposing the API.

### Choosing Which Specification Version to Implement

Be sure to match the Descriptors and Resources API versions, for example do not
accidentally use Descriptors API version 4.0 with the Resources API version 5.0.
A revision to the Discovery API is underway; either version could be chosen.

## EducationOrganizationId

Each entity that is a _child_ of `EducationOrganization` has its own version of
an `EducationOrganizationId` value, and these values _must_ be unique. For
example, an Ed-Fi API must not allow creation of a `LocalEducationAgency` and a
`School` with the same value for their respective `LocalEducationAgencyId` and
`SchoolId` properties. For more background, see [API Design Guidelines >
Resources > Education
Organizations](v4.0/API-DESIGN-GUIDELINES/RESOURCES.md#education-organizations).
