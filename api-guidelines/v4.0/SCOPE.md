# Scope

The Ed-Fi Alliance produces several related Application Programming Interface
(API) specifications for exchange of K-12 education data. The essential feature
that characterizes an "Ed-Fi API" is implementation of the Ed-Fi Resource API,
following the REST architectural style.

## Ed-Fi API Specifications

### Resource API

The [Ed-Fi Unifying Data Model
(UDM)](https://techdocs.ed-fi.org/display/ETKB/Ed-Fi+Unifying+Data+Model)
provides the basis for the data transferred and manipulated by an implementation
of the Ed-Fi Resource API specification. The Ed-Fi UDM is a structured,
conceptual model of common K–12 education data. The model includes _entities_
that are easily recognized by educators and administrators: schools, students,
teachers, attendance, grades, assessment results, and many others. These
entities contain _attributes_ (i.e., properties) that are also easily
recognized. For example, assessment results contain data, such as a score and
the date the assessment was administered. The UDM also includes _associations_
(i.e., relationships) between entities, such as the association between students
and schools.

REST interfaces are built around Resources that define nouns. In the education
domain, these nouns include such things as schools, students, and teachers. In
the Ed-Fi UDM these nouns have been rigorously defined as "entities," with
specific attributes and associations. Compositions of entities, with their
attributes and associations, are called "domain aggregates." These are
identified from the Ed-Fi UDM according to the principles of [Domain-Driven
Design
(DDD)](http://www.infoq.com/minibooks/domain-driven-design-quickly). Domain
aggregates are the Resources for an Ed-Fi Resource API. These concepts are
discussed in more detail later in this document.

### Discovery API

The Ed-Fi Discovery API presents clients with configuration information about
a running application, including:

* The software version.
* Which version of the UDM is exposed by its implementation of the Resource API.
* URL's for authentication and access to the various API specification(s)
  implemented by that application.
* And, potentially, other runtime configuration parameters.

The Discovery API allows client developers to build applications that need know
only one base URL, can extract runtime information from it, and alter behavior
as appropriate.

### Defining an Ed-Fi API

An application that exposes some subset of the Ed-Fi Resource API, and adheres
to this guideline document, is said to be "Ed-Fi aligned." An application that
is Ed-Fi aligned, supports the entire Resource API, and implements the Ed-Fi
Discovery API is said to be "Ed-Fi compatible".  In either situation, an
application _may_ expose other API's, whether from Ed-Fi or another source.

Unless otherwise stated, the guidelines provided by this document apply to both
_compatible_ and _aligned_ API applications.

### Other API Specifications

Ed-Fi API software _must_ utilize the OAuth 2.0 API specification for managing
authentication, though the implementation _may_ use non-Ed-Fi software to
provision OAuth-related services.

> ![NOTE]
> The Ed-Fi ODS/API exclusively uses a built-in OAuth 2.0 provider, which cannot
> be replaced by an off-the-shelf component. This need not be true of other
> implementations.

Other specifications managed by the Ed-Fi Alliance include:

* Ed-Fi Admin API
* Ed-Fi Identity API
* Ed-Fi Change Queries API
* Ed-Fi Enrollment API
* various _profiles_ of the Resource API
* and others as may be defined from time to time.

## Architectural Style

The [REST architectural
style](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm) is a
convention-based approach to defining APIs using the HTTP methods (GET, PUT,
POST, DELETE, etc.), as the application protocol.

REST-style architectures consist of clients and servers. Clients initiate
requests to servers; servers process requests and return appropriate responses.
Requests and responses are built around the transfer of representations of
Resources. As depicted below, a data store or server-based application
implements, exposes, or is wrapped with an Ed-Fi API to allow client
applications to exchange and manipulate education data.

![Image showing HTTP request and response between client and REST API service](Client-Server-Figure.png)

**Figure 1.** Interaction between REST API and client application

APIs can be thought of as a "contract" between data sources and client
applications. The underlying platform and application choices are unimportant in
terms of this contract. An Ed-Fi API follows this pattern. An Ed-Fi API imposes
no technical requirements on how data are internally stored or how they are used
by client applications. The API need only provide the technical contract between
a provider of data and its consumer applications, externally representing the
exchanged data Resources in a way that is aligned with the Ed-Fi UDM.

The same resource may be represented to different clients using different
representations. For example, an API application may represent a resource as
JSON for an application that is performing transactions, but may use XML to
represent the same resource for another purpose. The representation is a way to
externally represent the resource, but is not the resource itself. While some
use cases may remain for other format, all Ed-Fi API implementations _must_
support JSON representations.

There may be circumstances where an Ed-Fi API application would diverge from a
pure REST approach to support specific use cases, for example, to support
application-specific operations. Such use cases should be defined by their own
specifications, which can sit alongside the Ed-Fi specification implementation.

## API Guidelines Contents

* [Scope](SCOPE.md)
* [Key Characteristics](KEY-CHARACTERISTICS.md)
* [Requirement Levels](REQUIREMENT-LEVELS.md)
* [API Design Guidelines](API-DESIGN-GUIDELINES/README.md)
* [API Implementation Guidelines](API-IMPLEMENTATION-GUIDELINES/README.md)
