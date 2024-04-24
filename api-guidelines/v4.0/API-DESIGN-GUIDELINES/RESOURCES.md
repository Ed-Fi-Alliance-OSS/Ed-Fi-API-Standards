# Resources

The Resources that are used by an Ed-Fi API are compositions of entities,
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
number of course-related entities in the Ed-Fi UDM. A complete list of Resources
is included in the [Ed-Fi API specification
documents](../../../api-specifications/).

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

Each resource exposed by an Ed-Fi API _must_ be referenced by an
internally-assigned unique identifier. While the specific algorithm for
generating these identifiers is not prescribed in these guidelines, the
identifiers _could_ be generated using a [UUID
implementation](http://en.wikipedia.org/wiki/Globally_unique_identifier) such as
Microsoft's GUID (globally unique identifier). An Ed-Fi API _should_ generate
unique identifiers for its clients, and _should not_ accept client-generated
identifiers when inserting or updating data. This unique identifier _must_ be
immutable: it does not change on modification of a document.

All Resources _must_ be created with and also be retrievable by one or more
externally defined primary key values. Those values _must_ be natural keys of
the resource. For example, a Session is uniquely identified by the Session Name,
School Year, and a reference to a School. Resources _must_ be accessible by
primary key values using the standard HTTP GET query string search syntax:

```none
{resourceURI}?keyField1={value1}&keyField2={value2}
```

PUT, PATCH, and DELETE operations _must_ be identified using their URI
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
UDM _must_ be implemented in an Ed-Fi  API.

For example, the two `schoolId` values, nested within `schoolReference` and
`sessionReference`, must have the same value in the following `CourseOffering`
resource.

```json
{
  "id": "5394aabc256d4b938a8137d3b971b372",
  "courseReference": {
    "courseCode": "ALG-1",
    "educationOrganizationId": 255901001
  },
  "schoolReference": {
    "schoolId": 255901001
  },
  "sessionReference": {
    "schoolId": 255901001,
    "schoolYear": 2022,
    "sessionName": "2021-2022 Fall Semester"
  },
  "localCourseCode": "ALG-1"
}
```

## Education Organizations

The Ed-Fi UDM contains an _abstract entity_ called `EducationOrganization`. This
entity does not exist in the Ed-Fi API specifications. Instead, it provides a
common set of properties that other entities _inherit_. For example,
`EducationOrganization` has a property called `nameOfInstitution`. All entities
that inherit from `EducationOrganization` ("child entities") automatically have
the `nameOfInstitution` property, without needing to redefine that property on
the child entity.

`EducationOrganization` has one property that must be handled carefully in any
Ed-Fi API: the `EducationOrganizationId`. This value _must be unique_ across all
child entities. Furthermore, this value receives a different property name for
each child entity.

For example, `LocalEducationAgency` and `School` both inherit from
`EducationOrganization`. Respectively, they have properties
`LocalEducationAgencyId` and `SchoolId`, which _are_ `EducationOrganizationId`
values, albeit by a different name. The uniqueness constraint means this: if
there is a `LocalEducationAgency` with `LocalEducationAgencyId = 123`, then
there _must not_ also exist a `School` with `SchoolId = 123`.

The necessity of this requirement becomes more apparent when looking at the
`/ed-fi/studentEducationOrganizationAssociation` resource in the Ed-Fi Resources
API specification. This resource describes a single student in relation to a
single `EducationOrganization`. A student can thus have multiple records in this
resource: one each for `LocalEducationAgency`, `School`, or any other child
object of `EducationOrganization`. If the `<childEntity>Id` value were allowed
to repeat, then it would be impossible to determine _which_ specific child
resource is intended.

The following snippet comes from a single
`StudentEducationOrganizationAssociation` document. The value `255901` must
uniquely describe a single `EducationOrganizationAssociation`.

```json
{
  "id": "bb82e4c1433547f3bedf97c133e8148a",
  "educationOrganizationReference": {
    "educationOrganizationId": 255901
  },
  "studentReference": {
    "studentUniqueId": "604824"
  },
...
```

## Resource Extensions

The Ed-Fi UDM can be extended to augment the structure of existing entities or
create entirely new entities (cf [Ed-Fi Data Standard Extension
Framework](https://edfi.atlassian.net/wiki/x/LYBgAw)). In the context of an
Ed-Fi REST API, these are called resource extensions. Consumers of an Ed-Fi REST
API interact with resource extensions just as they interact with other
Resources. For example, if the Student resource has been extended in the data
model supported by the API platform host, an API consumer requesting a student
will receive the extended resource.

An Ed-Fi API resource _must_ follow a specific pattern for extensibility in its
structure in order to distinguish extension attributes from native attributes.
Extensions _must_ be denoted by use of the reserved term `_ext` and namespaced.
For API users, this both clearly distinguishes these attributes from native
attributes that originate from the UDM, and provides (via the namespace)
information as to the origin or governance of the extension. The example below
shows the JSON for a Staff resource that has been extended using a "_grandbend_"
namespace.

```json
{
  "id": "string",
  "staffUniqueId": "string",
  ...
  "yearsOfPriorProfessionalExperience": 0,
  "yearsOfPriorTeachingExperience": 0,
  "_ext": {
    "grandbend": {
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

Ed-Fi resources utilize Descriptors to enumerate small sets of valid values for
some elements. This enumeration ensures higher quality in the data compared to
allowing free-text strings. For example, a student can be associated with one or
more languages. The list of languages is specified in the `languageDescriptors`.
Each descriptor has a `namespace` and `codeValue`. These two are combined into
`<namespace>#<codeValue>` when filling in a Descriptor value on a resource.

Example: the Aromanian language with codeValue `rup` is in the
`uri://ed-fi.org/LanguageDescriptor` namespace. When assigning a language to a
student, an API client uses the string `uri://ed-fi.org/LanguageDescriptor#rup`.

See [Ed-Fi Descriptors](./ED-FI-DESCRIPTORS.md) for more information on use and
management of Descriptors.

## API Guidelines Contents

* [Scope](../SCOPE.md)
* [Key Characteristics](../KEY-CHARACTERISTICS.md)
* [Requirement Levels](../REQUIREMENT-LEVELS.md)
* [API Design Guidelines](../API-DESIGN-GUIDELINES/README.md)
  * [Resources](RESOURCES.md)
  * [Discovery API](./DISCOVERY-API.md)
  * [Ed-Fi Descriptors](./ED-FI-DESCRIPTORS.md)
  * [REST API Conventions](./REST-API.md)
    * [Uniform Resource Locators (URLs)](./UNIFORM-RESOURCE-LOCATORS.md)
    * [Data Strictness](./DATA-STRICTNESS.md)
    * [POST Requests](./POST-REQUESTS.md)
    * [GET Requests](./GET-REQUESTS.md)
    * [PUT Requests](./PUT-REQUESTS.md)
    * [DELETE Requests](./DELETE-REQUESTS.md)
* [API Implementation Guidelines](../API-IMPLEMENTATION-GUIDELINES/README.md)
