# Resources

The Resources that are used by an Ed-Fi REST API are compositions of entities,
attributes, and associations. Resources are domain aggregates that have been
identified from the Ed-Fi Unifying Data Model (UDM) according to the principles
of Domain-Driven Design (DDD). Use cases and events in the domain typically
center on individual domain aggregates.

Domain aggregates are organized along transactional boundaries, where the data
contained should "live" and "die" together. For example, the Discipline Incident
domain aggregate contains details about a discipline incident, and also captures
data related to the behaviors and students involved. A Discipline Action is not
generally captured at the same time as the Discipline Incident. Therefore, each
is separated into its own domain aggregate.

Each domain aggregate has an aggregate root. An aggregate root is an entity (and
in some cases an association) that includes other entities, their attributes,
and associations. The subordinate entities, attributes, and associations of a
domain aggregate are not directly accessible and can only be referenced through
the aggregate root.

Most entities in the Ed-Fi UDM are aggregate roots (e.g., Student, School,
Course). They contain no other entities. In some cases, an association that
represents a significant domain concept is also represented as an aggregate
root. For example, the StudentSchoolAssociation is represented as an aggregate
root because it reflects enrollment.

In the table below, the domain aggregate for a Course is constructed from a
number of course-related entities in the Ed-Fi UDM. A complete list of
Resources, Types, and Descriptors is included in the [API Resource
Listing](https://api.ed-fi.org).

**Table 1.** Sample Domain Aggregate

| Domain Aggregate | Entities                  |
| ---------------- | ------------------------- |
| Course           | Course                    |
|                  | CourseCompetencyLevel     |
|                  | CourseGradeLevel          |
|                  | CourseIdentificationCode  |
|                  | CourseLearningObjective   |
|                  | CourseLearningStandard    |
|                  | CourseLevelCharacteristic |

Each resource exposed by an Ed-Fi REST API _must_ be referenced by an
internally-assigned universally unique identifier (UUID). While the specific
algorithm for generating these identifiers is not prescribed in these
guidelines, the identifiers _should_ be generated using a UUID implementation
such as Microsoft's GUID (globally unique identifier)[\[5\]](#f5). An Ed-Fi REST
API _should_ generate unique identifiers for its clients, and _should not_
accept client-generated identifiers when inserting or updating data.

All Resources _must_ be created with and also be retrievable by one or more
externally defined primary key values. Those values _must_ be natural keys of
the resource. For example, a Session is uniquely identified by the Session Name,
School Year, and a reference to a School. Resources _must_ be accessible by
primary key values using the standard HTTP GET query string search syntax:

```none
{resourceURI}?keyField1={value1}&keyField2={value2}
```

PUT, PATCH, and DELETE operations _must_ be identified using their UUID
(i.e., `{resourceURI}/{id}`). PUT, PATCH, and DELETE operations _should_ also be
identifiable using their primary key values (natural keys).

## Field and Key Unification

The Ed-Fi UDM describes cases in which fields on a resource are merged with
other fields on a resource, producing a situation in which two fields must have
the same value. This most commonly occurs in references to other API resources,
as these references use the (often composite) primary key fields. In such cases,
key fields from the reference may be defined — via the UDM — to be unified with
existing fields on the resource.

This unification has proven extremely valuable to ensuring basic data quality in
field operations. All field and key unification scenarios defined in the
UDM _must_ be implemented in an Ed-Fi REST API.

## Resource Extensions

The Ed-Fi UDM can be extended to augment the structure of existing entities or
create entirely new entities.[\[6\]](#f6)In the context of an Ed-Fi REST API,
these are called resource extensions. Consumers of an Ed-Fi REST API interact
with resource extensions just as they interact with other Resources. For
example, if the Student resource has been extended in the data model supported
by the API platform host, an API consumer requesting a student will receive the
extended resource.

An Ed-Fi REST API resource _must_ follow a specific pattern for extensibility in
its structure in order to distinguish extension attributes from native
attributes. Extensions _must_ be denoted by use of the reserved term `_ext` and
namespaced. For API users, this both clearly distinguishes these attributes from
native attributes that originate from the UDM, and provides (via the namespace)
information as to the origin or governance of the extension. The example below
shows the JSON for a Staff resource that has been extended using a "_GrandBend_"
namespace.

```json
{
  "id": "string",
  "staffUniqueId": "string",
  ...
  "yearsOfPriorProfessionalExperience": 0,
  "yearsOfPriorTeachingExperience": 0,
  "_ext": {
    "GrandBend": {
      "probationCompleteDate": "2019-05-14",
      "tenured": true
    }
  }
}
```

Likewise, new domain aggregates created via extensions must follow a similar
pattern by using the namespace in the path of the new resource.

The example below shows the path for resources in the "_ed-fi_" and
"_grand-bend_" namespaces.

```none
/ed-fi/staffs
/grand-bend/applicants
```

It is _not recommended_ that namespaces be fully dereferenceable URIs due to the
complications in the path and resource models that this would create. Rather,
namespaces are intended to be lightweight. If public repositories of namespaces
exist for Ed-Fi-aligned APIs, the creator of an extension _should_ register that
namespace.

## Descriptors

Ed-Fi Descriptors exposed through the API are analogous to enumerations in XML
or lookup values in a database. Descriptors _must_ include a namespace, and the
namespace should follow the URI format. 

It is _recommended_ that Descriptors be read-only from the API's perspective.
However, an implementation _may_ allow them to be created using POST and updated
using PUT.

-----

<a name="f5"></a>5 For additional information on UUIDs, see
[here](http://en.wikipedia.org/wiki/Globally_unique_identifier).

<a name="f6"></a>6 Extensions to the Ed-Fi data model in an API context are
discussed in the Ed-Fi ODS / API documentation, available
[here]([/pages/viewpage.action?pageId=43583558](https://techdocs.ed-fi.org/x/RgiZAg)).

## API Guidelines Contents

* [Scope](../SCOPE.md)
* [Key Characteristics](../KEY-CHARACTERISTICS.md)
* [Requirement Levels](../REQUIREMENT-LEVELS.md)
* [API Design Guidelines](../API-DESIGN-GUIDELINES/README.md)
  * [Resources](RESOURCES.md)
  * [HTTP Verbs](HTTP-VERBS.md)
  * [General Request Construction](GENERAL-REQUEST-CONSTRUCTION.md)
  * [Ed-Fi Descriptors](ED-FI-DESCRIPTORS.md)
  * [Bulk Operations](BULK-OPERATIONS.md)
  * [Query Operators](QUERY-OPERATORS.md)
  * [Response Codes](RESPONSE-CODES.md)
* [ETags and Other REST API Conventions and
  Features](ETAGS-OTHER-CONVENTIONS.md)
* [API Implementation Guidelines](../API-IMPLEMENTATION-GUIDELINES/README.md)
